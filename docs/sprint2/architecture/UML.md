# UML Diagrams

## 1. Document Information

| Field | Value |
| --- | --- |
| Project | AI-Assisted Phishing Triage System |
| Course | SENG 321 – Software Engineering Project |
| Document | UML Diagrams |
| Version | 1.0 draft |
| Status | Sprint 2 working document |
| Related issue | GitHub Issue #24 |

This document describes the main structural and behavioral models of the AI-Assisted Phishing Triage System. The diagrams are written in Mermaid so their source remains editable, reviewable, and version-controlled in the repository.

The models are aligned with `docs/sprint2/SRS.md`, `docs/sprint2/architecture/C4.md`, `docs/PROJECT_PROPOSAL.md`, and `docs/SYSTEM_VISION_BOUNDARY.md`.

## 2. Modeling Conventions

- `Report` represents one submitted or mailbox-ingested suspicious email.
- `Case` represents the investigation record for exactly one accepted Report.
- `RiskAssessment` is advisory automation and is separate from the analyst's final decision.
- `Case.status` and `RiskAssessment.analysisStatus` have independent lifecycles.
- Email content, headers, URLs, and attachment metadata are untrusted data.
- Mermaid blocks are the editable diagram sources and can be rendered by a Mermaid-compatible Markdown viewer.

## 3. UML Use Case Diagram

The use case diagram identifies the primary actors and the principal system capabilities.

```mermaid
flowchart LR
    employee[Employee / Reporter]
    analyst[SOC / IT Analyst]
    admin[System Administrator]
    mailbox[Reporting Mailbox]
    model[Optional Model Provider]
    ti[Optional Threat Intelligence]

    subgraph system[AI-Assisted Phishing Triage System]
        UC1((Authenticate))
        UC2((Submit suspicious email))
        UC3((View own report status))
        UC4((Ingest forwarded email))
        UC5((Parse and normalize email))
        UC6((Assess risk))
        UC7((View analyst queue))
        UC8((Inspect case evidence))
        UC9((Update case status))
        UC10((Add analyst note))
        UC11((Record final classification))
        UC12((Resolve case))
        UC13((Manage users and roles))
        UC14((Manage mailbox settings))
        UC15((Review operational logs))
    end

    employee --> UC1
    employee --> UC2
    employee --> UC3
    analyst --> UC1
    analyst --> UC7
    analyst --> UC8
    analyst --> UC9
    analyst --> UC10
    analyst --> UC11
    analyst --> UC12
    admin --> UC1
    admin --> UC13
    admin --> UC14
    admin --> UC15
    mailbox --> UC4
    UC2 -.->|includes| UC5
    UC4 -.->|includes| UC5
    UC2 -.->|creates| UC6
    UC4 -.->|creates| UC6
    UC6 -.->|may call| model
    UC6 -.->|may call| ti
    UC12 -.->|requires| UC11
    UC9 -.->|records| UC15
    UC10 -.->|records| UC15
    UC11 -.->|records| UC15
    UC12 -.->|records| UC15
```

### 3.1 Use case notes

| Use case | Main actor | Important rule |
| --- | --- | --- |
| Authenticate | All human actors | Protected operations require valid authentication. |
| Submit suspicious email | Employee | Creates a Report and exactly one linked Case after validation. |
| View own report status | Employee | Only verified reports attributed to that employee are visible; analyst details are hidden. |
| Ingest forwarded email | Reporting Mailbox | Duplicate provider messages must not create duplicate processing results. |
| Parse and normalize email | System | Content is never executed; unavailable evidence remains explicitly unavailable. |
| Assess risk | System / optional providers | Produces an advisory `Low`, `Medium`, or `Critical` recommendation. |
| View analyst queue | Analyst | Shows authorized Cases and relevant status/risk information. |
| Inspect case evidence | Analyst | Separates observed evidence, unavailable checks, and automated interpretations. |
| Update case status | Analyst | Uses `New`, `Under Review`, and `Resolved`. |
| Add analyst note | Analyst | Creates an attributable CaseHistory record. |
| Record final classification | Analyst | Uses `Phishing`, `Legitimate`, or `Inconclusive`; remains separate from risk. |
| Resolve case | Analyst | Requires a final classification before changing status to `Resolved`. |
| Manage users/roles/settings/logs | Administrator | Administrator privileges do not automatically grant analyst privileges. |

## 4. Sequence Diagram – Phishing Report Processing

This sequence covers both the authenticated web submission and the common processing path after mailbox ingestion. The `channel` participant represents either the web form/API or the controlled mailbox adapter.

```mermaid
sequenceDiagram
    actor Reporter as Employee / Mailbox
    participant UI as Web UI / Mailbox Adapter
    participant API as REST API / Intake
    participant Auth as Auth & Authorization
    participant Parser as Email Parser
    participant DB as Application Database
    participant Case as Case Management
    participant Risk as Risk Assessment
    participant Model as Optional Model Provider
    participant TI as Optional Threat Intelligence

    Reporter->>UI: Submit suspicious email / deliver forwarded message
    UI->>API: Send report payload and provenance
    API->>Auth: Authenticate and authorize request
    Auth-->>API: Authorized / rejected

    alt Request is unauthorized
        API-->>UI: 401/403 error
        UI-->>Reporter: Submission rejected
    else Request is authorized or mailbox adapter is trusted
        API->>API: Validate payload and determine idempotency key
        API->>DB: Check provider message/idempotency key

        alt Duplicate provider message
            DB-->>API: Existing processing result
            API-->>UI: Existing Report/Case reference
        else New report
            API->>DB: Persist Report and provenance
            API->>Parser: Parse and normalize supported content

            alt Parser succeeds partially or completely
                Parser-->>API: Headers, body, URLs, attachments, hashes
                API->>DB: Persist parsed evidence and availability states
            else Parser fails
                Parser-->>API: Parsing error and available metadata
                API->>DB: Persist error and partial evidence
            end

            API->>Case: Create one Case for the accepted Report
            Case->>DB: Persist Case with status New
            Case->>Risk: Start assessment
            Risk->>DB: Persist status Pending
            Risk->>Model: Optional structured content analysis
            Risk->>TI: Optional URL/hash lookup
            Model-->>Risk: Result or provider error
            TI-->>Risk: Evidence or provider error
            Risk->>DB: Persist score, category, evidence and status
            API-->>UI: Report and Case created
            UI-->>Reporter: Accepted / processing status
        end
    end
```

### 4.1 Processing invariants

1. A duplicate mailbox message does not create a second Report or Case.
2. A parser or external-provider failure does not delete an accepted Report.
3. A Case is created with status `New`; risk completion does not resolve the Case.
4. Automated risk is stored as an advisory assessment and cannot set final classification.
5. Missing authentication evidence is represented as unavailable, not as a successful check.

## 5. Sequence Diagram – Case Review and Status Update

This sequence shows the human-in-the-loop review workflow and the requirement that a final classification exists before resolution.

```mermaid
sequenceDiagram
    actor Analyst as SOC / IT Analyst
    participant UI as Analyst Dashboard
    participant API as REST API
    participant Auth as Auth & Authorization
    participant Case as Case Management
    participant DB as Application Database
    participant Risk as Risk Assessment

    Analyst->>UI: Open analyst queue
    UI->>API: GET /cases
    API->>Auth: Verify analyst role
    Auth-->>API: Authorized / rejected

    alt Not authorized
        API-->>UI: 401/403 error
    else Authorized
        API->>DB: Query permitted Cases
        DB-->>API: Prioritized queue
        API-->>UI: Queue data
        Analyst->>UI: Open Case detail
        UI->>API: GET /cases/{id}
        API->>Case: Request evidence and history
        Case->>DB: Load Report, assessment and history
        DB-->>Case: Case data
        Case-->>API: Authorized Case detail
        API-->>UI: Evidence, status and analysis state

        Analyst->>UI: Start review
        UI->>API: Change status to Under Review
        API->>Case: Validate transition
        Case->>DB: Save status and CaseHistory
        DB-->>Case: Saved
        Case-->>API: Updated Case
        API-->>UI: Under Review

        opt Add analyst note
            Analyst->>UI: Enter note
            UI->>API: POST note
            API->>Case: Validate analyst permission
            Case->>DB: Save note and CaseHistory
            DB-->>Case: Saved
        end

        Analyst->>UI: Record final classification
        UI->>API: Set Phishing / Legitimate / Inconclusive
        API->>Case: Validate classification
        Case->>DB: Save classification and CaseHistory
        DB-->>Case: Saved

        Analyst->>UI: Resolve Case
        UI->>API: Change status to Resolved
        API->>Case: Validate final classification exists

        alt Classification exists
            Case->>DB: Save Resolved status and CaseHistory
            DB-->>Case: Saved
            Case-->>API: Resolved Case
            API-->>UI: Resolution confirmed
        else Classification missing
            Case-->>API: Validation error
            API-->>UI: Cannot resolve without final classification
        end
    end
```

## 6. UML Class Diagram

The class diagram models the core domain entities, their relationships, and the main service abstractions. Exact database field types may be refined during implementation.

```mermaid
classDiagram
    class User {
        +UUID id
        +String email
        +Role role
        +Boolean active
        +authenticate()
    }

    class Report {
        +UUID id
        +IngestionChannel ingestionChannel
        +AttributionStatus submitterAttributionStatus
        +String providerMessageId
        +DateTime receivedAt
        +String rawContentReference
        +accept()
    }

    class ParsedEvidence {
        +List~Header~ headers
        +List~URLIndicator~ urls
        +List~AttachmentMetadata~ attachments
        +String bodyTextReference
        +ParseStatus parseStatus
        +String error
    }

    class RiskAssessment {
        +UUID id
        +Decimal score
        +RiskCategory category
        +AnalysisStatus analysisStatus
        +List~EvidenceSignal~ signals
        +String ruleOrModelVersion
        +DateTime assessedAt
    }

    class Case {
        +UUID id
        +CaseStatus status
        +FinalClassification finalClassification
        +UUID analystOwnerId
        +DateTime createdAt
        +DateTime resolvedAt
        +startReview()
        +recordClassification()
        +resolve()
    }

    class CaseHistory {
        +UUID id
        +HistoryAction action
        +String previousValue
        +String newValue
        +String note
        +DateTime occurredAt
    }

    class ReportIntakeService {
        +acceptWebSubmission()
        +ingestMailboxMessage()
        +checkIdempotency()
    }

    class EmailParser {
        +parse()
        +extractHeaders()
        +extractUrls()
        +extractAttachments()
    }

    class RiskAssessmentService {
        +startAssessment()
        +combineSignals()
        +recordFailure()
    }

    class CaseManagementService {
        +createCase()
        +changeStatus()
        +addNote()
        +recordFinalClassification()
        +resolveCase()
    }

    class ModelProvider {
        <<interface>>
        +analyzeContent()
    }

    class ThreatIntelligenceProvider {
        <<interface>>
        +lookupUrl()
        +lookupHash()
    }

    class Role {
        <<enumeration>>
        EMPLOYEE
        ANALYST
        ADMINISTRATOR
    }

    class CaseStatus {
        <<enumeration>>
        NEW
        UNDER_REVIEW
        RESOLVED
    }

    class FinalClassification {
        <<enumeration>>
        PHISHING
        LEGITIMATE
        INCONCLUSIVE
    }

    class RiskCategory {
        <<enumeration>>
        LOW
        MEDIUM
        CRITICAL
    }

    class AnalysisStatus {
        <<enumeration>>
        PENDING
        COMPLETED
        FAILED
    }

    User "1" --> "0..*" Report : submits
    Report "1" *-- "0..1" ParsedEvidence : contains
    Report "1" --> "1" Case : creates
    Case "1" *-- "0..1" RiskAssessment : has advisory result
    Case "1" *-- "0..*" CaseHistory : records
    User "1" --> "0..*" CaseHistory : performs
    Case "0..1" --> "1" User : assigned analyst
    ReportIntakeService ..> Report : creates
    ReportIntakeService ..> EmailParser : uses
    EmailParser ..> ParsedEvidence : produces
    RiskAssessmentService ..> RiskAssessment : creates/updates
    RiskAssessmentService ..> ModelProvider : optionally uses
    RiskAssessmentService ..> ThreatIntelligenceProvider : optionally uses
    CaseManagementService ..> Case : manages
    CaseManagementService ..> CaseHistory : writes
    User --> Role
    Case --> CaseStatus
    Case --> FinalClassification
    RiskAssessment --> RiskCategory
    RiskAssessment --> AnalysisStatus
```

## 7. Class Responsibilities and Invariants

| Class | Responsibility | Key invariant |
| --- | --- | --- |
| `User` | Stores identity, role, and active status. | Role checks are enforced by the backend. |
| `Report` | Stores one submission and its provenance. | One accepted Report maps to exactly one Case. |
| `ParsedEvidence` | Stores safely extracted indicators and parse state. | Missing evidence is not treated as a positive or negative verdict. |
| `RiskAssessment` | Stores the automated advisory result. | Risk category is not the analyst's final classification. |
| `Case` | Stores investigation state and final decision. | `Resolved` requires a final classification. |
| `CaseHistory` | Stores attributable case actions and notes. | Significant actions include actor and timestamp. |
| `ReportIntakeService` | Coordinates web and mailbox intake and idempotency. | Duplicate provider messages are not processed twice. |
| `EmailParser` | Extracts safe message evidence. | Attachments and active content are never executed. |
| `RiskAssessmentService` | Orchestrates technical and optional external signals. | Provider failure leaves a human-review path. |
| `CaseManagementService` | Applies workflow rules and records history. | Invalid status transitions are rejected. |

## 8. State Models

### 8.1 Case state

```mermaid
stateDiagram-v2
    [*] --> New
    New --> UnderReview : analyst starts review
    UnderReview --> UnderReview : add note / update evidence
    UnderReview --> Resolved : final classification exists
    Resolved --> [*]
```

### 8.2 Risk assessment state

```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending --> Completed : assessment succeeds
    Pending --> Failed : parser/provider failure
    Failed --> Pending : permitted retry
    Completed --> [*]
    Failed --> [*]
```

Case status and risk-assessment status are independent. A `Failed` assessment does not resolve, delete, or hide a Case.

## 9. Consistency with C4 and Requirements

| UML model | Related architecture or requirement |
| --- | --- |
| Use Case diagram | SRS actors, FR-001–FR-020, and C4 system context |
| Report processing sequence | SRS FR-003–FR-012 and C4 intake/parser/risk containers |
| Case review sequence | SRS FR-013–FR-017 and analyst human-in-the-loop rule |
| Class diagram | C4 core data model and Report–Case 1:1 relationship |
| Case state model | SRS case status transition and resolution requirements |
| Risk state model | SRS analysis lifecycle and failure-handling requirements |

The diagrams deliberately do not show automatic analyst classification, automatic Case resolution, or autonomous mailbox remediation because those behaviors are outside the approved system boundary.

## 10. Open Decisions

- Confirm the final API endpoint names against the OpenAPI draft.
- Confirm whether `RiskAssessment` is stored as one current record or as a versioned assessment history.
- Confirm the concrete authentication/session classes after the implementation technology is selected.
- Export rendered diagram images if the course submission requires standalone image files in addition to the editable Mermaid source.

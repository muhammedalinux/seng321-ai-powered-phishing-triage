# Software Requirements Specification (SRS)

## 1. Document Information

| Field | Value |
| --- | --- |
| Project | AI-Assisted Phishing Triage System |
| Course | SENG 321 – Software Engineering Project |
| Document | Software Requirements Specification |
| Version | 1.0 draft |
| Status | Sprint 2 working document |
| Related issue | GitHub Issue #17 |

This document defines the functional requirements, actors, use cases, workflows, and acceptance criteria of the AI-Assisted Phishing Triage System. It is based on the current `PROJECT_PROPOSAL.md` and `SYSTEM_VISION_BOUNDARY.md` documents.

Non-functional requirements are summarized only where they affect system behavior. The detailed non-functional requirements will be maintained separately in the Sprint 2 NFR document.

## 2. Purpose and Scope

The system assists an organization's employees and SOC/IT analysts with the initial triage of suspicious email reports. It accepts reported messages, safely extracts available evidence, produces an advisory risk assessment, and provides an authenticated analyst workflow for investigation and resolution.

The system does not replace a corporate email security gateway, make autonomous remediation decisions, or allow an AI component to make the final case classification. Final decisions remain under the control of an authorized analyst.

### 2.1 In scope

- Authenticated web-based suspicious email reporting.
- Controlled reporting-mailbox ingestion.
- `.eml`/MIME parsing and safe normalization.
- Extraction of headers, URLs, attachment metadata, and hashes where available.
- Advisory risk assessment using technical and content-based signals.
- Analyst queue, case details, notes, classification, and resolution workflow.
- Role-based access control and auditable case history.
- Idempotent processing of the same provider mailbox message.

### 2.2 Out of scope

- Automatic deletion, blocking, quarantine, or other mailbox remediation.
- Attachment detonation or behavioral malware execution.
- Outlook/Gmail one-click add-ins.
- Production-scale multi-tenancy and enterprise directory synchronization.
- Automatic retraining or policy changes based only on analyst feedback.

## 3. Definitions and Constraints

| Term | Definition |
| --- | --- |
| Report | One employee-submitted or mailbox-ingested suspicious email submission. |
| Case | The investigation record created for an accepted Report. The MVP uses a 1:1 Report–Case relationship. |
| Risk assessment | An automated, advisory evaluation containing a score, category, evidence, and analysis status. |
| Case status | The investigation lifecycle state: `New`, `Under Review`, or `Resolved`. |
| Final classification | The authorized analyst's decision: `Phishing`, `Legitimate`, or `Inconclusive`. |
| Risk category | The automated advisory priority: `Low`, `Medium`, or `Critical`. |
| Verified attribution | A reporter identity supported by trustworthy authentication or ingestion-channel metadata. |
| Unverified attribution | A submission for which the system cannot reliably establish the employee's identity. |

The displayed `From` header of the suspicious email is not sufficient evidence of the reporter's identity. Original email content, URLs, headers, and attachments are untrusted input and must not control authorization, tool use, or case decisions.

## 4. Actors and External Systems

| Actor / system | Responsibilities and interaction |
| --- | --- |
| Employee / Reporter | Submits a suspicious email and views the status of their own verified reports. |
| SOC / IT Analyst | Reviews cases, examines evidence, records notes and final classifications, and resolves cases. |
| System Administrator | Manages users, roles, mailbox settings, and operational logs. Administrator privileges do not automatically include analyst privileges. |
| Email provider / reporting mailbox | Delivers forwarded reports to the controlled mailbox ingestion adapter. It is outside the application boundary. |
| Optional model provider | May provide content-analysis signals through a controlled integration. It is outside the application boundary and is never trusted with authorization decisions. |
| Optional threat-intelligence provider | May provide URL or hash evidence. Its response is evidence with an availability status, not an unconditional verdict. |

## 5. System Overview and Boundaries

The application boundary contains the web UI, REST backend, authentication and authorization, report intake, mailbox adapter, email parser, risk-assessment orchestration, case management, and application database.

External email infrastructure, optional model and threat-intelligence providers, and hosting operations are outside the application boundary. The system owns the integration interfaces but not the availability or internal behavior of those external services.

The primary workflow is:

```text
Employee or mailbox
        |
        v
   Report intake --> Safe parsing and normalization --> Report + Case persistence
                                                        |
                                                        v
                                                Risk assessment
                                                        |
                                                        v
                         Analyst queue --> Evidence review --> Final decision --> Resolution
```

Each accepted Report creates exactly one Case. Reports containing the same suspicious email from different employees remain separate cases. Reprocessing the same provider mailbox message must be prevented through idempotency checks.

## 6. Functional Requirements

### 6.1 Authentication and authorization

#### FR-001 – User authentication

The system shall authenticate users before granting access to protected operations.

**Acceptance criteria**

- Valid credentials produce an authenticated session or access token.
- Invalid credentials are rejected without revealing which credential was incorrect.
- Missing, invalid, or expired authentication credentials produce an unauthorized response.

#### FR-002 – Role-based authorization

The system shall enforce permissions based on the user's assigned role.

**Acceptance criteria**

- Employees cannot access the analyst queue, analyst notes, or risk-analysis details.
- Analysts can access the queue and permitted case-review operations.
- Administrators can manage accounts, roles, mailbox settings, and operational logs.
- An administrator is not granted analyst privileges unless separately assigned.
- An unauthorized operation is rejected and does not change stored data.

### 6.2 Report submission and ingestion

#### FR-003 – Web report submission

The system shall allow an authenticated employee to submit a suspicious email through the web interface using supported manual fields or a supported `.eml` upload.

**Acceptance criteria**

- The system validates required submission data before accepting the report.
- A valid submission receives a unique Report identifier and creation timestamp.
- Malformed or unsupported input is rejected with a useful validation error.
- The original submitted content is preserved or an explicit ingestion error is recorded.

#### FR-004 – Mailbox report ingestion

The system shall accept reports from the configured reporting mailbox through a controlled ingestion adapter.

**Acceptance criteria**

- A valid mailbox message is converted into a Report.
- The ingestion channel and available provenance metadata are stored.
- The original suspicious sender is kept distinct from the employee or mailbox identity that forwarded the report.
- A mailbox or parsing failure is recorded without deleting the source case data.

#### FR-005 – Idempotent ingestion

The system shall not create duplicate processing results when the same provider mailbox message is received more than once.

**Acceptance criteria**

- A stable provider message identifier or equivalent idempotency key is stored.
- Reprocessing the same message does not create a second Report or Case.
- Reports from different employees are not merged solely because their email contents appear identical.

### 6.3 Parsing and evidence extraction

#### FR-006 – Safe email parsing

The system shall parse supported email messages without executing active content.

**Acceptance criteria**

- The parser extracts available headers, body text, URLs, attachment names, extensions, and hashes.
- Malformed MIME structures are handled as an error or partial result rather than crashing the ingestion workflow.
- Attachments and message content are treated as data and are not executed.

#### FR-007 – Provenance and attribution

The system shall record the report's ingestion channel, provenance information, and submitter attribution status.

**Acceptance criteria**

- Attribution is stored as `Verified` or `Unverified` according to trustworthy channel evidence.
- A displayed `From` header alone cannot change an employee's attribution to `Verified`.
- Unverified forwarded reports remain available to authorized analysts.
- Employees can see only the status of their own verified reports.

#### FR-008 – Evidence availability

The system shall distinguish available evidence from unavailable or failed checks.

**Acceptance criteria**

- Missing original headers do not produce a false claim that SPF, DKIM, or DMARC was successfully validated.
- External lookup failures are represented with an availability or error status.
- Analysts can identify which evidence was observed, unavailable, or generated by an automated analysis component.

### 6.4 Risk assessment

#### FR-009 – Advisory assessment

The system shall generate an advisory risk assessment from available technical and content-based signals.

**Acceptance criteria**

- The assessment stores a score or equivalent structured result, supporting evidence, model/rule version, and analysis status.
- The risk category is one of `Low`, `Medium`, or `Critical`.
- The assessment is clearly labeled as an automated recommendation.
- The assessment does not directly set the analyst's final classification or resolve a Case.

#### FR-010 – Analysis lifecycle

The system shall represent analysis status separately from Case investigation status.

**Acceptance criteria**

- Analysis status supports `Pending`, `Completed`, and `Failed`.
- Completing an analysis does not automatically change the Case status.
- A failed analysis does not delete or close the Case.
- A Case with a failed or partial assessment remains available for human review.

#### FR-011 – Untrusted content handling

The system shall treat email bodies, headers, URLs, and attachment metadata as untrusted data.

**Acceptance criteria**

- Submitted content cannot change user permissions or system instructions.
- Automated analysis cannot authorize its own actions, close a Case, or perform remediation.
- Structured results are validated before being stored or displayed.

### 6.5 Case management

#### FR-012 – Report–Case creation

The system shall create exactly one Case for each accepted Report.

**Acceptance criteria**

- The Report and Case identifiers are stored as a one-to-one relationship.
- The Case is created with status `New`.
- A failed risk assessment does not prevent the accepted Report and Case from remaining available for review.

#### FR-013 – Analyst queue

The system shall provide authorized analysts with a prioritized Case queue.

**Acceptance criteria**

- The queue displays relevant Case status and available risk category information.
- Analysts can open a Case detail view from the queue.
- The queue does not expose sensitive analysis details to unauthenticated users or employees.

#### FR-014 – Case status transitions

The system shall support the Case investigation statuses `New`, `Under Review`, and `Resolved`.

**Acceptance criteria**

- An authorized analyst can move a `New` Case to `Under Review`.
- An authorized analyst can resolve a Case only after recording a final classification.
- Invalid status transitions are rejected.
- Every successful status transition is recorded in CaseHistory.

#### FR-015 – Final classification

The system shall allow an authorized analyst to record a final classification of `Phishing`, `Legitimate`, or `Inconclusive`.

**Acceptance criteria**

- Final classification is stored separately from automated risk category.
- A Case may be resolved with `Inconclusive` as its final classification.
- Employees cannot create or modify an analyst's final classification.
- The actor and timestamp of the classification are recorded.

#### FR-016 – Analyst notes

The system shall allow an authorized analyst to add notes to a Case.

**Acceptance criteria**

- Notes are associated with the correct Case and author.
- Notes are timestamped and retained in the case history.
- Notes are not visible to employees through the employee report-status view.

#### FR-017 – Case history and auditability

The system shall maintain an auditable history of significant Case actions.

**Acceptance criteria**

- Case creation, status changes, final classifications, and analyst notes produce history records.
- Each history record contains an actor or system source and timestamp.
- History records cannot be silently changed through normal Case operations.

### 6.6 Employee status access and administration

#### FR-018 – Employee report status

The system shall allow an employee to view the status of their own verified reports without exposing analyst-only information.

**Acceptance criteria**

- An employee sees only reports securely attributed to their account.
- The view includes report and Case status but excludes analyst notes, internal evidence, and action history.
- An employee cannot query another employee's reports by changing an identifier in a request.

#### FR-019 – Administrative configuration

The system shall allow an authorized System Administrator to manage user accounts, roles, reporting-mailbox settings, and operational logs.

**Acceptance criteria**

- Only an authorized administrator can perform configuration operations.
- Configuration changes are validated before being applied.
- Administrative actions are auditable.

#### FR-020 – Error and partial-result handling

The system shall preserve an accepted Report and Case when a downstream parser, model, or threat-intelligence operation fails.

**Acceptance criteria**

- The failure is stored with an appropriate error or analysis status.
- The Case remains visible to an authorized analyst.
- The system does not present a failed check as a successful low-risk result.

## 7. Use Cases

### UC-01 – Submit suspicious email

| Item | Description |
| --- | --- |
| Primary actor | Employee / Reporter |
| Preconditions | The employee is authenticated and the reporting form is available. |
| Main flow | The employee enters message details or uploads an `.eml` file; the system validates, stores, parses, and creates a Report and Case. |
| Alternative flows | Invalid input is rejected; parsing failure is recorded and the accepted Case remains available for review. |
| Postconditions | A unique Report and one linked Case exist. |

### UC-02 – Ingest forwarded mailbox report

| Item | Description |
| --- | --- |
| Primary actor | Reporting mailbox adapter |
| Preconditions | A message is available in the configured mailbox. |
| Main flow | The adapter reads the message, checks idempotency, stores provenance, and creates a Report and linked Case. |
| Alternative flows | A duplicate message is ignored; malformed content is stored with an ingestion error where possible. |
| Postconditions | The message is processed once and is available to authorized analysts. |

### UC-03 – Review analyst queue

| Item | Description |
| --- | --- |
| Primary actor | SOC / IT Analyst |
| Preconditions | The analyst is authenticated and authorized. |
| Main flow | The analyst opens the queue, filters or sorts Cases, and opens a Case detail view. |
| Postconditions | The analyst can inspect available evidence and analysis status. |

### UC-04 – Investigate and resolve Case

| Item | Description |
| --- | --- |
| Primary actor | SOC / IT Analyst |
| Preconditions | The analyst can access the Case. |
| Main flow | The analyst moves the Case to `Under Review`, examines evidence, adds notes, records a final classification, and resolves the Case. |
| Alternative flows | The analyst records `Inconclusive`; an analysis failure leaves the Case open for manual review. |
| Postconditions | The Case has an auditable final classification and, when resolved, status `Resolved`. |

### UC-05 – View own report status

| Item | Description |
| --- | --- |
| Primary actor | Employee / Reporter |
| Preconditions | The employee is authenticated and the report has verified attribution. |
| Main flow | The employee requests their report list and views report and Case status. |
| Alternative flows | Unverified or another employee's report is not shown. |
| Postconditions | No analyst-only evidence, notes, or history is disclosed. |

### UC-06 – Manage system configuration

| Item | Description |
| --- | --- |
| Primary actor | System Administrator |
| Preconditions | The administrator is authenticated and authorized. |
| Main flow | The administrator manages accounts, roles, mailbox settings, or operational logs. |
| Postconditions | The validated change is applied and recorded for audit. |

## 8. Cross-Requirement Acceptance Rules

The following rules apply across the SRS:

1. Automated risk is advisory and never substitutes for an analyst's final decision.
2. `Risk category`, `Case status`, and `Final classification` are separate fields with separate lifecycles.
3. Every accepted Report has exactly one Case, and duplicate mailbox delivery does not create duplicate processing results.
4. Authorization is enforced independently of values supplied in email content or client requests.
5. Analysis, parsing, and external-service failures preserve the human-review path.
6. Security-sensitive and analyst actions are attributable and timestamped.

## 9. Requirements Traceability

| SRS requirements | Source documents / related work |
| --- | --- |
| FR-001–FR-002, FR-018–FR-019 | `SYSTEM_VISION_BOUNDARY.md` access scope and roles; Project Proposal Sections 5 and 7 |
| FR-003–FR-008 | `SYSTEM_VISION_BOUNDARY.md` MVP scope and trust boundaries; Project Proposal Sections 4, 6, and 10 |
| FR-009–FR-011 | `SYSTEM_VISION_BOUNDARY.md` risk and AI authority rules; Project Proposal Section 7 |
| FR-012–FR-017 | `SYSTEM_VISION_BOUNDARY.md` workflow, Case model, and auditability rules |
| FR-020 | `SYSTEM_VISION_BOUNDARY.md` fault handling and reliability requirements |
| UC-01–UC-06 | Project Proposal stakeholders and System Vision workflows |

Before implementation, these requirements should be reconciled with the OpenAPI draft and the detailed Sprint 2 non-functional requirements.

## 10. Open Questions

- Which authentication mechanism and identity provider will be used in the MVP?
- Will risk assessment run synchronously during submission or asynchronously through a background job?
- Which mailbox provider and provider message identifier will be used for the demonstration?
- Which optional model and threat-intelligence integrations, if any, will be enabled for the MVP?
- What exact instructor rubric or template must be applied before final submission?

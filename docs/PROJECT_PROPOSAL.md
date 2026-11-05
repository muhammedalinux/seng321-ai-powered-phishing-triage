# Project Proposal

- **Project:** AI-Assisted Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026
- **Prepared by:** Ezgi Naz Aydugan
- **Version:** v1.3
- **Status:** Revised draft, aligned with System Vision & Boundary v1.1 (reference document). OpenAPI Draft alignment still to be verified before submission.

---

## 1. Problem Statement

Organizations depend on employees to report suspicious emails, but security teams often have to inspect those reports manually. A mixed stream of phishing attempts, spam, legitimate messages, and duplicate reports can delay investigation of genuinely dangerous messages.

- **Employee perspective:** Staff need a straightforward way to report suspicious messages without interrupting their work.
- **SOC / IT perspective:** Analysts need to distinguish potentially dangerous reports from low-priority submissions and understand why a report was flagged.
- **Operational problem:** Unstructured reports, duplicate submissions, and manual initial assessment create avoidable triage effort and can delay human investigation.

The problem addressed here is **the prioritization and handling of employee-reported suspicious emails**, not the replacement of an enterprise email security gateway or the autonomous remediation of attacks.

## 2. Rationale & Necessity

Employee reports provide useful security signals even when organizations use email protection systems. However, the submitted messages can vary significantly in risk and quality of evidence. Reviewing every item with the same urgency is inefficient.

The proposed system performs preliminary, evidence-based assessment and shows prioritized cases to security analysts. AI-assisted text analysis complements, rather than substitutes for, deterministic email and URL checks. **Only authorized analysts make final case decisions.** The expected benefit is more consistent triage and clearer visibility into incoming reports; the size of any time savings must be measured rather than assumed.

## 3. Stakeholders

| Stakeholder | Interest and responsibility |
| --- | --- |
| **Employees / reporters** | Submit suspicious emails through the web form or by forwarding to the reporting mailbox; view the status of reports securely attributed to their account. |
| **SOC / IT security analysts** | Review and prioritize cases, inspect evidence, record notes and final classifications, and resolve cases. |
| **Security leadership / CISO** | Evaluate workflow efficiency, review volumes, and operational risk. |
| **System administrators / organization IT** | Configure the reporting inbox, access controls, service credentials, and integrations, where available. |
| **Development team** | Implement, test, document, and maintain the academic prototype. |

## 4. Expected Benefits

- **Faster initial assessment:** Reduce manual effort by automatically extracting indicators and preparing an initial risk recommendation.
- **Consistent prioritization:** Apply a documented scoring policy to place potentially urgent cases higher in the analyst queue.
- **Improved analyst context:** Display header results, detected URLs, attachment metadata, and NLP signals together, including missing or unavailable evidence.
- **Traceable decisions:** Record analyst notes, status changes, final classifications, and resolution outcomes.
- **Idempotent ingestion:** Avoid processing the same provider mailbox message twice; reports of the same suspicious email from different employees remain separate cases.

These are expected benefits, not guaranteed production outcomes.

## 5. Target Users & Access Scope

The MVP includes two primary operational roles, plus a System Administrator for configuration:

### 5.1 Employee / Reporter

- May submit a suspicious email through the authenticated web form (manual entry or a supported `.eml` upload) or by forwarding it to a configured reporting mailbox.
- May view the status of reports securely attributed to their account (report and case status only; AI results, analyst notes, and action history are not shown). Cannot view unverified forwarded reports or other employees' reports.
- A forwarded message's displayed `From` header or an address match against a directory **does not establish the reporter's identity**. The system may record a `submitter_attribution_status` of `Verified` or `Unverified` based on the actual ingestion channel's trustworthy metadata. For the academic MVP, forwarded submissions can remain unverified when trustworthy identity evidence is absent.
- The original suspicious email's sender must be distinguished from the employee who forwarded the report.

### 5.2 SOC / IT Security Analyst

- Must authenticate before viewing the SOC dashboard and case information.
- Can view the prioritized queue, supporting evidence, risk recommendation and analysis status.
- Can review cases, add notes, record a final classification, and resolve cases as permitted by the defined workflow. The advisory risk category is not edited; the analyst's decision is recorded separately.
- Analyst actions must be auditable; AI-generated recommendations must remain distinguishable from human decisions.

### 5.3 System Administrator

- Manages accounts, roles, mailbox settings, and operational logs.
- Has no analyst privileges unless separately assigned.

**Access boundary:** The MVP does not expose cases or sensitive analysis details to unauthenticated reporters, and employees see only report and case status for verified reports. The role-based authorization policy for API routes must match the OpenAPI Draft and System Vision & Boundary documents.

## 6. Proposed Solution & Architecture

The system is a modular pipeline that accepts a report, stores its evidence and metadata, performs automated analysis (synchronous or asynchronous execution to be decided in EN-01), and exposes an analyst-reviewed case in the dashboard.

1. **Report ingestion:** Receive reports through the authenticated web form (manual entry or `.eml` upload via `POST /reports`) or as forwarded email messages through a controlled mailbox adapter. API authentication and payload requirements are specified separately in the OpenAPI Draft. Preserve sufficient message provenance and distinguish forwarded message headers from the original suspicious message, when recoverable.
2. **Safe parsing and normalization:** Extract textual content, original headers where available, URLs, attachment names/extensions and file hashes without executing active content. Handle malformed or incomplete messages safely.
3. **Technical checks:** Evaluate available SPF/DKIM/DMARC evidence, suspicious domains, URL reputation, and attachment metadata. Do not claim successful authentication validation when original headers or verification context are missing. Treat each external threat-intelligence response as evidence with an availability/status field, not as a guaranteed verdict.
4. **NLP/LLM analysis:** Assess language patterns associated with social engineering. Treat all submitted email content as untrusted data; it must not control tool use, system instructions, case closure, or authorization.
5. **Risk recommendation:** Combine available signals using a documented, testable scoring policy and recommend **Low / Medium / Critical** priority. Distinguish an unavailable analysis from a confidently low-risk result.
6. **Report and case management:** Persist each incoming `Report` and create exactly one `Case` for it (1:1). Reports of the same suspicious email from different employees remain separate cases; cross-employee case merging is out of MVP scope. A case stores its investigation status, final classification, analyst ownership, and history; the linked `RiskAssessment` stores the advisory risk category and analysis status.
7. **SOC dashboard:** Show an ordered queue, case details, supporting indicators, analysis failures, and manual review/resolution controls.

### 6.1 Suggested domain model

- **Report:** Individual submission (one-to-one with a Case), ingestion channel, timestamps, reporter attribution status, original-message provenance, parsed artifacts, and ingestion errors.
- **Case:** Investigation unit linked one-to-one to a report; investigation status, final classification, and analyst ownership.
- **RiskAssessment:** Advisory score, risk category, evidence (technical/NLP signals with source status), model/rule version, analysis status, and errors.
- **CaseHistory:** Timestamped actions and analyst notes, with actor and relevant previous/new values.

Entity names, relationships, and enum values follow the System Vision & Boundary (which also defines `User`); the API specification must match them.

### 6.2 Workflow and exception states

Case investigation status is `New → Under Review → Resolved`. Analysis status belongs to the `RiskAssessment` and is separate (`Pending`, `Completed`, `Failed`); analysis completion does not change the case status, and a failed analysis does not close or delete a case. Cases remain available for human review if risk assessment, an LLM service, or a threat-intelligence lookup fails. The final classification (`Phishing`, `Legitimate`, or `Inconclusive`) is **separate from** risk category and investigation status, and is required to mark a case `Resolved` (Resolved + Inconclusive is permitted).

## 7. AI Authority & Human-in-the-Loop Safeguards

- **Recommendation only:** Automated components extract indicators and suggest queue priority; they do not independently mark a case as definitively phishing/legitimate, resolve it, delete messages, or perform mailbox remediation.
- **Human review:** An authenticated SOC analyst records the final classification and resolution decision. Analyst decisions are stored with attribution and timestamps.
- **Evidence-based explanation:** Show observed indicators (e.g., a known-malicious URL lookup result) separately from model interpretations (e.g., urgency-related language). Generated explanations are not guaranteed faithful descriptions of a model's internal reasoning.
- **Untrusted content:** Prompt injection within email bodies, URLs, headers, and attachments must not be treated as system commands. Structured outputs, permission checks, and independent validation are required; prompt wording alone is not a security boundary.

## 8. Measurable Success Criteria

The academic prototype will be evaluated against a held-out, labeled collection of phishing and benign emails. Dataset origin, class distribution, train/test separation, and labeling limitations will be documented. Where available, tests using realistic employee-report samples will be distinguished from generic phishing datasets.

**Primary binary evaluation rule:** For prioritization evaluation, a **positive prediction** means an email is placed into **Medium or Critical** risk, and the **positive ground-truth class** is a labeled phishing email. Precision, Recall, F1 and FPR below all use this same rule. A separate Critical-only precision measure may be reported as a secondary metric, but must not be mixed into the primary F1 calculation.

| Metric | Proposed target | Calculation / measurement |
| --- | --- | --- |
| **Precision** | ≥ 85% (aspirational) | `TP / (TP + FP)` for Medium/Critical predictions |
| **Recall** | ≥ 90% (aspirational) | `TP / (TP + FN)` for phishing emails |
| **F1-score** | ≥ 87% (aspirational) | `2 × Precision × Recall / (Precision + Recall)` using the same predictions and test set |
| **False positive rate** | ≤ 10% (aspirational) | `FP / (FP + TN)` among benign emails |
| **Automated triage latency** | p95 ≤ 15 seconds under defined demo conditions (aspirational) | Time from accepted submission to a completed/partial scoring result available to the dashboard; report sample size and dependency conditions. Complements System Vision NFR-07, which covers API request latency and excludes asynchronous risk assessment |
| **Review traceability** | 100% of analyst case decisions recorded | Verify that review, status-change, final-classification, and resolution events create case-history records in integration tests |

**Important:** These thresholds are project goals, not claims that the system already meets them. If precision/recall targets conflict with measured results, report the actual numbers and limitations. **Mean Time to Respond (MTTR) reduction** requires a comparable human-workflow baseline and is not equivalent to automated triage latency; it will be a future operational evaluation unless that baseline is collected.

## 9. Assumptions and Dependencies

1. **Demo environment:** A controlled academic mailbox or simulated mail ingestion source is available. Full enterprise email tenancy integration is not assumed.
2. **External services:** The project may optionally use a third-party model-inference provider (LLM API) and a threat-intelligence service, subject to credentials, cost, rate limits, usage policies and outages; neither is required, since the System Vision allows rule-based, ML-based or hybrid assessment. Local fixtures or deterministic fallback checks can support repeatable demonstrations.
3. **Reporter identity:** A mock directory may assist with address lookup, but cannot by itself prove who submitted a forwarded report. Only trustworthy channel authentication evidence supports `Verified` attribution.
4. **Evaluation data:** Publicly available, legally usable datasets or synthetic/curated samples are available. Their representativeness of real employee reports is limited and will be disclosed.
5. **Security/privacy:** Sensitive source messages will be restricted to authorized users. External services will receive only the minimal approved data; redaction may not eliminate all privacy risks.
6. **Header availability:** The original email and its authentication evidence may be missing or altered by forwarding, preventing reliable SPF/DKIM/DMARC conclusions for some reports.

## 10. Initial Scope (MVP vs. Future Phases)

### 10.1 In Scope — MVP

- Report ingestion through the web form (`POST /reports`, manual entry or `.eml` upload) and a controlled test-mailbox forwarding adapter demonstrated end to end. Recorded forwarded-email fixtures support repeatable tests but do not replace the required mailbox ingestion demonstration.
- Safe parsing of supported email samples, with metadata extraction, URL extraction, attachment hashes/extensions, and error handling. The first implementation targets `.eml`/MIME; native Outlook `.msg` conversion is not required.
- Header inspection of available evidence, explicitly marking checks as `unavailable` when necessary.
- Content assessment (rule-based, ML-based or hybrid; LLM-assisted where used) with a predictable structured result and deterministic fallback.
- Threat-intelligence URL/hash lookups where available, with mocked or cached examples for tests/demos.
- Documented composite score and Low/Medium/Critical recommendation; incomplete analysis is visible.
- One-to-one Report–Case creation, idempotent ingestion, case status management, case history, analyst notes, and final classification.
- Authenticated SOC dashboard with priority queue, case detail, indicator summary, and manual closure workflow.
- Basic tests for authentication/authorization, parsing failures, scoring boundaries, integrations, and case workflows.

### 10.2 Out of Scope — Future Work

- Outlook/Gmail one-click reporting add-ins and organization-wide deployment.
- Automatic mailbox deletion, quarantining, blocking, or other autonomous remediation.
- Detonation or behavioral execution of attachments in a malware sandbox.
- Deep SIEM/SOAR integrations and production enterprise directory synchronization.
- End-user notifications and an employee portal beyond viewing the status of their own verified reports.
- Production-scale SLA, full multi-tenancy, and a formally validated forensic email-analysis platform.
- Automatic online retraining or automatic policy changes based solely on analyst feedback.

## 11. Feasibility Analysis

### 11.1 Technical feasibility

A modular-monolith implementation is appropriate for a university MVP: **FastAPI** for the backend, **React** for the dashboard, **PostgreSQL** for durable records, and optional **Redis/Celery** for background processing if the simplest database-backed job approach is insufficient. LLM and threat-intelligence providers are wrapped behind replaceable interfaces, allowing test doubles and offline demonstration paths.

The largest feasibility risks are email provenance/authentication, reliable data for evaluation, safe handling of malicious content, and time-consuming external integrations. The MVP deliberately limits those dependencies.

### 11.2 Six-person academic team and delivery responsibilities

The project team has six assigned SENG 321 roles; the following describes **responsibility categories**, not a new assignment of all implementation work to different people:

| Team member | Assigned role | Main responsibility in this project |
| --- | --- | --- |
| **Ezgi** | Product Owner | Problem definition, proposal, requirements priorities and acceptance criteria |
| **Nil** | Scrum Master | Sprint planning, issue tracking, coordination and impediment follow-up |
| **Muhammed Ali** | DevOps | Repository workflow, CI, development environment, integration/deployment support |
| **Sema** | QA | Test strategy, test cases, acceptance verification and defect tracking |
| **Çağan** | Implementation Engineer | Agreed backend/frontend/pipeline implementation tasks |
| **Burak** | Architect | System vision and boundaries, component/data design, API and security architecture consistency |

**Implementation is a shared team responsibility** according to sprint issues and capacity. The RACI Matrix, not this proposal, is authoritative for accountable/consulted/informed designations.

### 11.3 Indicative implementation sequence

The following is a planning estimate, **not a claim about the course's fixed sprint calendar**:

- **Phase 1:** Agree on proposal, scope, boundary, data model, API draft and evaluation approach.
- **Phase 2:** Implement report ingestion, safe parsing, case storage and minimal dashboard workflow.
- **Phase 3:** Integrate scoring and available technical/LLM checks, with mockable external dependencies.
- **Phase 4:** Add case history, idempotent ingestion, security/error cases, tests and end-to-end demo.

Detailed sprint durations and work assignments will follow the course delivery plan and team backlog.

## 12. Risk Assessment & Mitigation

| Risk | Impact | Mitigation |
| --- | --- | --- |
| **Spoofed reporter identity** | High | Never trust a displayed `From` field as identity proof; distinguish verified from unverified attribution. |
| **Prompt injection in emails** | High | Treat message content as data; restrict tools and permissions; validate structured outputs and enforce human-only closure. |
| **Malicious/malformed payloads** | High | Parse without execution, restrict file sizes/types, avoid active HTML rendering, safely encode dashboard content and isolate risky parsers where feasible. |
| **Privacy leakage to LLM/Threat Intel providers** | High | Minimize and redact sent data, review provider policies, protect stored content, use demo/synthetic data where practical; disclose residual risk. |
| **Incorrect or misleading scoring** | High | Evaluate on held-out labeled data, show uncertainty/missing evidence, expose evidence separately from model interpretations, and require analyst verdicts. |
| **Original authentication evidence unavailable** | Medium | Show `unknown`/`unavailable`; do not present an unverified SPF/DKIM/DMARC result as a valid check. |
| **External API outages/rate limits** | Medium | Timeouts, bounded retries, caching and mock/deterministic fallback; retain reports for analyst review. |
| **Duplicate ingestion** | Medium | Ingest idempotently by provider message identity where available; keep each report as its own case. |
| **Scope/time overrun** | High | Prioritize a demonstrable end-to-end vertical slice and defer enterprise integrations. |
| **Unauthorized SOC access / audit gaps** | High | Authenticate analyst endpoints, enforce backend authorization and log state changes; test access boundaries. |

## 13. Open-Source License Selection

The intended **source-code license is MIT**, subject to confirming that the repository contains only materials the team has the right to redistribute.

- **Permissive use:** MIT permits reuse and modification, including commercial integration, subject to license and notice conditions.
- **Ease of collaboration:** Its straightforward terms are suitable for an academic open-source prototype.
- **Disclaimer:** The license includes warranty and liability disclaimers, but it does **not** guarantee immunity from all legal claims or remove duties regarding privacy, third-party licenses, or data rights.

**Repository note:** Do not publish real employee emails, sensitive indicators, credentials, or evaluation datasets without the necessary permissions. External API services and datasets have terms separate from the project's source-code license.

---

## Cross-Document Consistency Checklist (team review before final submission)

- [ ] Match `Report`, `Case`, `RiskAssessment`, `CaseHistory` names and cardinalities to System Vision & Boundary.
- [ ] Match API authentication, `POST /reports`, role access, and error/response schemas to the OpenAPI Draft.
- [ ] Align workflow states, final classifications, risk labels, and the one-to-one Report–Case rule across all deliverables.
- [ ] Align any NFR identifiers and measurable thresholds to the System Vision & Boundary (do not introduce conflicting NFR IDs).
- [ ] Demonstrate controlled test-mailbox ingestion end to end; use fixtures only for supplemental automated tests and document any unmet integration requirement as an unresolved limitation.
- [ ] Ensure the six actual team members and responsibilities match the RACI Matrix.
- [ ] Verify course sprint dates and deliverables before replacing the indicative phases with calendar weeks.

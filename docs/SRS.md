# Software Requirements Specification (SRS)

- **Project:** AI-Powered Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026
- **Author:** Ezgi Naz Aydugan (Product Owner)
- **Version:** 1.0
- **Status:** Draft for Sprint 2 Review
- **Baseline Documents:** `SYSTEM_VISION_BOUNDARY.md` (v1.1), `PROJECT_PROPOSAL.md` (v1.3), `openapi.yaml` (v1.1.0), `INITIAL_PRODUCT_BACKLOG.md` (v1.3)

---

## 1. Introduction

### 1.1 Purpose
This Software Requirements Specification (SRS) defines the functional requirements, user roles, scope boundaries, and behavioral rules for the **AI-Powered Phishing Triage System** MVP. It serves as the authoritative product specification aligning architecture, UI/UX design, implementation, and quality assurance.

### 1.2 Product Overview
The system is a web-based decision-support application that enables employees to report suspicious emails through a web form or a designated forwarding mailbox, performs safe content extraction and advisory AI risk assessment, and provides IT/SOC security analysts with a prioritized triage workflow to investigate, document, and resolve cases. Automated risk scores are strictly advisory; final case classification remains exclusively under human analyst control.

---

## 2. Project Scope

### 2.1 In-Scope (MVP)
- **Authentication & Role-Based Access Control (RBAC):** Credential-based login (`POST /auth/login`) issuing JWT bearer tokens and enforcing strict role separation across endpoints.
- **Dual-Channel Report Intake:**
  - Manual web submission via JSON payload (`POST /reports`) or raw `.eml` (MIME) file upload.
  - Automated ingestion from a controlled test mailbox for forwarded suspicious emails (`US-22`) with idempotent duplicate prevention (`US-07`).
- **1:1 Report-to-Case Mapping:** Every accepted `Report` automatically creates exactly one linked `Case` (`investigation_status = New`) and one `RiskAssessment` (`analysis_status = Pending`). Reports of the same suspicious email submitted by different employees remain separate `Report` and `Case` records.
- **Safe Extraction & Non-Execution:** Text-only parsing of sender metadata, subject, body text, and URLs without opening attachments, executing scripts, or navigating to embedded links.
- **Advisory AI Risk Assessment:** Generation of a `0–100` risk score, advisory `risk_category` (`Low`, `Medium`, `Critical`), explainable `evidence` indicators, and `model_version`, plus analyst-triggered retry (`POST /reports/{id}/analysis`) when `analysis_status = Failed`.
- **Analyst Triage Dashboard & Case Management:** Paginated case queue (`GET /cases`) sorted by risk priority (`Critical > Medium > Low`, with unscored cases listed after scored cases and never hidden) or creation date; filtering by status and risk; case assignment (`PATCH /cases/{id}`); investigation notes (`POST /cases/{id}/notes`); and append-only audit history (`GET /cases/{id}/history`).
- **Human-in-the-Loop Case Resolution:** Atomic transition from `Under Review` to `Resolved` requiring an explicit analyst `final_classification` (`Phishing`, `Legitimate`, or `Inconclusive`).

### 2.2 Out-of-Scope (Deferred / Won't in MVP)
- Autonomous email quarantine, deletion, blocking, or mailbox remediation actions.
- Native email client add-ins or one-click reporting buttons for Outlook/Gmail.
- Production enterprise directory (SSO/Active Directory) synchronization or organization-wide mailbox monitoring.
- Cross-employee case merging or deduplication across different reporters.
- Dynamic malware detonation in a sandbox or training custom LLMs from scratch.
- Full administrative management UI (System Administrator tasks are handled via controlled operational configuration) and automated employee resolution notifications (marked as `Could` priority, not required for MVP core).

---

## 3. User Roles & Authorization Boundaries

| Role | Description | Permitted Operations | Restrictions & Trust Boundaries |
| --- | --- | --- | --- |
| **Employee** | Authenticated staff member reporting suspicious messages. | • Sign in via `POST /auth/login`.<br>• Submit suspicious emails via web form or `.eml` upload (`POST /reports`).<br>• Forward suspicious emails to the designated reporting inbox.<br>• View own `Verified` reports (`GET /reports`, `GET /reports/{id}`) and their `investigation_status`. | • Cannot view other employees' reports (`403`).<br>• Cannot view `Unverified` forwarded reports (a `From` header or directory match alone does not prove identity).<br>• Cannot view AI risk scores, evidence, analyst notes, or case history.<br>• Cannot access `/cases` or `/metrics` endpoints (`403`). |
| **IT/SOC Security Analyst** | Authorized security responder investigating and resolving reported cases. | • Sign in and view all reports (`GET /reports`) and cases (`GET /cases`, `GET /cases/{id}`).<br>• Filter and sort the triage queue.<br>• Retry failed AI risk assessments (`POST /reports/{id}/analysis`).<br>• Transition cases (`New` → `Under Review` → `Resolved`), assign cases to analysts, record `final_classification`, and add notes (`PATCH /cases/{id}`, `POST /cases/{id}/notes`).<br>• View append-only case history (`GET /cases/{id}/history`). | • Cannot edit or delete AI risk assessment results.<br>• Cannot edit or delete existing `CaseHistory` records.<br>• Cannot manage user accounts or system roles unless separately granted System Administrator privileges. |
| **System Administrator** | Operational administrator configuring accounts and environment settings. | • Manage user accounts, role assignments, mailbox polling/ingestion configuration, and operational logs outside the MVP triage UI. | • Does not automatically inherit IT/SOC Analyst case investigation or resolution privileges unless explicitly assigned the `Analyst` role. |

---

## 4. Core Domain & Lifecycle Rules

1. **Separated Status Lifecycles:**
   - `Case.investigation_status` (`New`, `Under Review`, `Resolved`) tracks human investigation progress.
   - `RiskAssessment.analysis_status` (`Pending`, `Completed`, `Failed`) tracks automated analysis processing.
   - Completion or failure of `RiskAssessment` never automatically alters `Case.investigation_status`, nor does a failed analysis close or delete a `Case`.
2. **Mandatory Human Verdict:**
   - `RiskAssessment.risk_category` (`Low`, `Medium`, `Critical`) is strictly advisory.
   - A `Case` can transition from `Under Review` to `Resolved` only when a valid `final_classification` (`Phishing`, `Legitimate`, or `Inconclusive`) is provided in the same request or already stored on the case.
3. **Attribution & Privacy:**
   - Manual submissions via authenticated API session receive `submitter_attribution_status = Verified` and link to `submitter_id`.
   - Forwarded mailbox submissions lacking cryptographic/channel proof receive `submitter_attribution_status = Unverified` (`submitter_id = null`) and are excluded from employee-facing report queries.

   ---

## 5. Functional Requirements

### FR-01: User Authentication
- **Description:** The system shall authenticate registered users via email and password (`POST /auth/login`) and issue a bearer access token with role information (`Employee`, `Analyst`, or `Administrator`)[cite: 3, 4].
- **Behavior & Exceptions:**
  - Valid credentials return `200` with an access token, token type (`Bearer`), expiration time, and role[cite: 3].
  - Invalid credentials return `401` without revealing whether the email or password was incorrect[cite: 3, 4].
  - Requests to protected endpoints with missing, malformed, or expired tokens return `401`[cite: 3, 4].
  - Passwords shall be stored only as salted hashes (e.g., bcrypt or Argon2)[cite: 4].

### FR-02: Role-Based Access Control (RBAC)
- **Description:** The backend shall enforce role and ownership checks on every protected operation[cite: 3, 4, 5].
- **Behavior & Exceptions:**
  - An `Employee` can view only reports securely attributed (`Verified`) to their own authenticated account[cite: 3, 4, 5].
  - An `Employee` requesting another user's report or calling an analyst-only endpoint (`/cases`, `/metrics/summary`, or `/reports/{id}/analysis`) receives `403`[cite: 3, 4].
  - An `Analyst` can access all reports, cases, notes, and case history[cite: 3, 4, 5].
  - `Administrator` privileges do not automatically grant analyst case-investigation or case-resolution authority unless separately assigned[cite: 4, 5, 8].

### FR-03: Manual & `.eml` Report Submission (`POST /reports`)
- **Description:** Authenticated employees shall be able to submit suspicious emails either as structured JSON (`sender_address`, `subject`, optional `body_text` and `urls`) or by uploading an original `.eml` file (`multipart/form-data`)[cite: 3, 4, 8].
- **Behavior & Exceptions:**
  - A valid submission creates one `Report`, one linked `Case` (`investigation_status = New`), and one `RiskAssessment` (`analysis_status = Pending`), returning `201` with the created `Report` and `case_id`[cite: 3, 4].
  - Missing or invalid required fields return `400` and create no report[cite: 3, 4].
  - Files exceeding the configured size limit return `413` and create no report[cite: 3, 4].
  - Uploaded files other than `.eml` return `415` and create no report[cite: 3, 4].

### FR-04: Safe Content Parsing & Non-Execution
- **Description:** The system shall extract sender metadata, subject, body text, and URLs as text/data without opening attachments, executing scripts/code, or visiting links[cite: 3, 4, 5, 8].
- **Behavior & Exceptions:**
  - Clients shall render extracted email content as plain text rather than active HTML or executable content[cite: 3, 4, 5, 8].
  - Malformed or incomplete email content shall not crash the application; if indicator extraction fails, `analysis_status` is set to `Failed` with a diagnostic while preserving the `Report` and `Case`[cite: 3, 4, 5].

### FR-05: Forwarded Mailbox Ingestion & Idempotency
- **Description:** An internal inbox worker shall ingest emails forwarded to a controlled test mailbox and normalize them into `Report` and `Case` records[cite: 3, 4, 5].
- **Behavior & Exceptions:**
  - Ingestion shall be idempotent by provider message identifier where available so redelivery of the same mailbox message does not create duplicate `Report` or `Case` records[cite: 3, 4, 5].
  - Different employees reporting the same suspicious email shall result in separate `Report` and `Case` records (`1:1` cardinality)[cite: 3, 4, 5, 8].
  - A forwarding `From` header or directory address match alone does not establish `Verified` attribution; unverified forwarded reports have `submitter_attribution_status = Unverified` and are excluded from employee report lists[cite: 3, 4, 5, 8].

### FR-06: Employee Report Tracking (`GET /reports`, `GET /reports/{id}`)
- **Description:** Employees shall be able to list and inspect their own `Verified` reports in reverse chronological order (newest first)[cite: 3, 4].
- **Behavior & Exceptions:**
  - Each item displays report metadata, attribution status, linked `case_id`, and `investigation_status`, without exposing AI risk scores, analyst notes, or case history[cite: 3, 4, 8].
  - Empty result sets return `200` with an empty list so the UI can show an empty state[cite: 3, 4].

### FR-07: Advisory AI Risk Assessment & Retry (`POST /reports/{id}/analysis`)
- **Description:** The system shall evaluate reported emails to produce an advisory `RiskAssessment` (`score` from `0` to `100`, `risk_category` of `Low`, `Medium`, or `Critical`, `evidence` list, `model_version`, and `completed_at`)[cite: 3, 4, 5].
- **Behavior & Exceptions:**
  - Email content is treated strictly as untrusted data; instructions embedded in emails must not alter system behavior or automatically resolve a case[cite: 4, 5, 8].
  - On analysis failure or timeout, `analysis_status` becomes `Failed` with a `failure_reason`, while the `Report` and `Case` remain intact for manual triage[cite: 3, 4, 5].
  - Analysts may retry analysis (`POST /reports/{id}/analysis`) only when `analysis_status` is `Failed` (returning `202` and setting `analysis_status` to `Pending`); retrying while `Pending` or `Completed` returns `409`[cite: 3, 4].

### FR-08: Analyst Case Queue, Filtering & Sorting (`GET /cases`)
- **Description:** Analysts shall be able to retrieve a paginated list of cases with optional filters (`investigation_status`, `analysis_status`, `risk_category`) and sorting (`priority` or `newest`)[cite: 3, 4].
- **Behavior & Exceptions:**
  - Default `priority` sort orders `Critical` before `Medium` before `Low`, placing unscored/pending/failed cases after scored cases without hiding them[cite: 3, 4].
  - Invalid query parameter or filter values return `400`[cite: 3, 4].

### FR-09: Case Inspection, Status Transitions, Assignment & Resolution (`GET /cases/{id}`, `PATCH /cases/{id}`)
- **Description:** Analysts shall be able to inspect full case details (including linked report and risk assessment) and update case status, assignment, or final classification[cite: 3, 4].
- **Behavior & Exceptions:**
  - `New` to `Under Review` is permitted regardless of `analysis_status` (`Pending`, `Completed`, `Failed`)[cite: 3, 4].
  - `Under Review` to `Resolved` is permitted only when `final_classification` (`Phishing`, `Legitimate`, or `Inconclusive`) is supplied in the request or already present on the case, saved atomically[cite: 3, 4].
  - Disallowed lifecycle transitions return `409` without modifying the case[cite: 3, 4].
  - Assigning `assignee_id` to a non-analyst or nonexistent user returns `400`; reassignment among analysts is supported[cite: 3, 4].
  - Advisory `risk_assessment` data is never overwritten by case updates[cite: 3, 4].

### FR-10: Investigation Notes & Append-Only Audit History (`POST /cases/{id}/notes`, `GET /cases/{id}/history`)
- **Description:** Analysts shall be able to add investigation notes to a case and view its chronological, append-only audit trail[cite: 3, 4].
- **Behavior & Exceptions:**
  - Empty or whitespace-only notes return `400`[cite: 3, 4].
  - State changes and notes are recorded with actor ID (or `null` for system events), action type, old/new values where applicable, and timestamp[cite: 3, 4].
  - History entries cannot be edited or deleted via the API[cite: 3, 4].

### FR-11: Triage Workload Summary (`GET /metrics/summary`)
- **Description:** Analysts may retrieve aggregate case counts by `investigation_status` (`New`, `Under Review`, `Resolved`) and `risk_category` (`Low`, `Medium`, `Critical`, `Unscored`)[cite: 3, 4].
- **Behavior & Exceptions:**
  - Counts match the current case dataset[cite: 3, 4].
  - Marked as `Could` priority in the backlog and may be deferred if higher-priority MVP items require effort[cite: 3, 4].
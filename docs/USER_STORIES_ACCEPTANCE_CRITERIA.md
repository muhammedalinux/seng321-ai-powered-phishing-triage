# User Stories & Acceptance Criteria

- **Project:** AI-Powered Phishing Triage System
- **Role:** Product Owner (Ezgi Naz Aydugan)
- **Sprint:** 2
- **Aligned With:** `INITIAL_PRODUCT_BACKLOG.md` (v1.3), `openapi.yaml` (v1.1.0), `SYSTEM_VISION_BOUNDARY.md` (v1.1)[cite: 3, 4, 5]

---

## 1. Primary Stories Owned by Product Owner (Ezgi)

### US-05: Upload the Original Email File
> As an **Employee**, I want to upload the original `.eml` file so that analysts receive its headers and links unchanged[cite: 4].

- **Endpoint:** `POST /reports` (`multipart/form-data`, `eml_file`)[cite: 3, 4]
- **Acceptance Criteria:**
  - A valid `.eml` file submitted by an authenticated employee creates one `Report`, one linked `Case` (`investigation_status = New`), and one `RiskAssessment` (`analysis_status = Pending`), returning HTTP `201`[cite: 3, 4].
  - Uploading a non-`.eml` file returns HTTP `415` (`UnsupportedMediaType`) and creates no `Report` or `Case`[cite: 3, 4].
  - Uploading a file exceeding the agreed size limit returns HTTP `413` (`PayloadTooLarge`) and creates no `Report` or `Case`[cite: 3, 4].
  - The system parses the `.eml` content safely as text/data; email attachments are never opened/executed and embedded URLs are never visited automatically[cite: 3, 4, 5, 8].

### US-06: Track My Reports
> As an **Employee**, I want to see the status of my reports so that I know whether they were reviewed[cite: 4].

- **Endpoints:** `GET /reports`, `GET /reports/{id}`[cite: 3, 4]
- **Acceptance Criteria:**
  - A signed-in employee sees only reports securely attributed (`submitter_attribution_status = Verified`) to their own account, ordered newest first[cite: 3, 4, 5].
  - Unverified forwarded reports (`Unverified`) are never automatically linked to an employee by `From` header or directory lookup and never appear in the employee's list[cite: 3, 4, 5, 8].
  - Attempting to access another employee's report via `GET /reports/{id}` returns HTTP `403` (`Forbidden`)[cite: 3, 4].
  - Each item displays report ID, submission timestamp, safely extracted summary/content, linked `case_id`, and `investigation_status` (`New`, `Under Review`, `Resolved`), without exposing AI risk scores/evidence, internal analyst notes, or `CaseHistory`[cite: 3, 4, 8].
  - When an employee has no verified reports, the API returns HTTP `200` with an empty list and the UI displays an empty-state message[cite: 3, 4].

### US-10: View Risk Level and Reasons
> As a **Security Analyst**, I want to see the risk level and the reasons behind it so that I can judge whether the AI result is reliable[cite: 4].

- **Endpoint:** `GET /reports/{id}` (and `GET /cases/{id}` for full case view)[cite: 3, 4]
- **Acceptance Criteria:**
  - When `analysis_status = Completed`, the view displays the advisory `risk_category` (`Low`, `Medium`, `Critical`), `score` (`0–100`), `evidence` indicators, `model_version`, and `completed_at`[cite: 3, 4].
  - The AI assessment is explicitly labeled as advisory and kept separate from the analyst's `final_classification`; automated scoring never resolves a case automatically[cite: 3, 4, 5, 8].
  - When `analysis_status = Pending`, the UI displays an in-progress/pending state rather than an invented score or risk category[cite: 3, 4].
  - When `analysis_status = Failed`, the UI displays a failed state and `failure_reason` (and allows an authorized analyst to trigger retry via `POST /reports/{id}/analysis`), without closing or deleting the case[cite: 3, 4, 5].

### US-13: View Report Details
> As a **Security Analyst**, I want to open a report so that I can see all of its information[cite: 4].

- **Endpoints:** `GET /reports/{id}`, `GET /cases/{id}`[cite: 3, 4]
- **Acceptance Criteria:**
  - An authorized analyst can view the full report details, `source_channel` (`Manual` or `Forwarded`), `submitter_attribution_status` (`Verified` or `Unverified`), submission timestamp, linked `case_id`, `investigation_status`, and `risk_assessment` state[cite: 3, 4, 5].
  - Requesting a nonexistent report ID returns HTTP `404` (`NotFound`); an employee requesting the endpoint without ownership returns HTTP `403` (`Forbidden`)[cite: 3, 4].
  - Extracted email subject, body text, and URLs are rendered as plain text so scripts do not execute and links are not clickable/active links[cite: 3, 4, 5, 8].

---

## 2. Core MVP Stories Alignment (Cross-Role Reference)

To keep all sprint deliverables consistent, the remaining core `Must` stories align with the following rules[cite: 4]:
- **US-01 (`POST /auth/login`):** Valid credentials return `200` with token and role; invalid credentials return `401` without revealing which field failed; missing/expired tokens return `401`[cite: 3, 4].
- **US-02 (RBAC):** Employees receive `403` on analyst-only routes (`/cases`, `/metrics/summary`, `/reports/{id}/analysis`) and unauthorized report IDs[cite: 3, 4].
- **US-03 (Data Protection):** Email content is excluded from logs, only approved fields go to AI analysis, retention rules apply, and non-local traffic uses HTTPS[cite: 4].
- **US-04 (`POST /reports` JSON):** Valid submission creates `1` `Report`, `1` `Case` (`New`), and `1` `RiskAssessment` (`Pending`) with HTTP `201`; invalid/missing fields return `400` without creating records[cite: 3, 4].
- **US-07 & US-22 (Forwarded Ingestion & Idempotency):** Controlled test mailbox ingests forwarded messages without trusting `From` alone for `Verified` status; duplicate redelivery of the same provider message ID does not create duplicate `Report`/`Case` records, while distinct employees' reports remain separate (`1:1`)[cite: 3, 4, 5, 8].
- **US-08 & US-09 (Indicator Extraction & Risk Analysis):** Extracts sender/domain, subject, and URLs without opening links; handles malformed inputs by setting `analysis_status = Failed` without altering `investigation_status`; restricts retry (`POST /reports/{id}/analysis`) to `Failed` state (`409` if `Pending` or `Completed`)[cite: 3, 4].
- **US-11 & US-12 (`GET /cases`):** Supports `investigation_status`, `analysis_status`, and `risk_category` filters plus `priority` (`Critical > Medium > Low`, unscored after scored and never hidden) and `newest` sorting; invalid filters return `400`[cite: 3, 4].
- **US-14, US-15 & US-16 (`PATCH /cases/{id}`):** `New` → `Under Review` is allowed regardless of `analysis_status`; `Under Review` → `Resolved` requires `final_classification` (`Phishing`, `Legitimate`, `Inconclusive`) saved atomically; invalid transitions return `409`; non-analyst `assignee_id` returns `400`; AI score is never overwritten[cite: 3, 4, 5].
- **US-17, US-18 & US-19 (Notes & History):** Analysts can post non-empty notes (`POST /cases/{id}/notes`, empty returns `400`) and inspect chronological append-only history (`GET /cases/{id}/history`) that cannot be edited or deleted via API[cite: 3, 4].
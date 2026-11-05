# Initial Product Backlog

- **Project:** AI-Assisted Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026
- **Team:** Ezgi, Nil, Burak, Muhammed Ali, Sema, Çağan
- **Prepared by:** Sema
- **Version:** v1.3
- **Status:** Revised draft, aligned with System Vision & Boundary v1.1 (reference document). Product Owner approval, team estimates/owners and OpenAPI sign-off remain pending.
- **Revision basis:** Original backlog and QA findings Q-01–Q-29; not yet verified against the OpenAPI file.

## 1. Purpose

This backlog defines the product work for our phishing triage system. Employees report suspicious emails, the system analyzes them with AI, and security analysts use the results to manage each report. It covers the product we will build, not the Sprint 1 documents, which we track separately in GitHub Issues.

## 2. How to Read This Backlog

**Hierarchy:** Epic > Feature > User Story > Task.

**User roles:** `Employee` submits reports and may see only reports tied to their authenticated identity; `Security Analyst` (IT/SOC) investigates cases and makes final decisions; `System Administrator` manages accounts, roles and operational configuration without automatically inheriting analyst case-decision authority. Forwarded emails do not establish employee identity from the `From` field alone. Admin UI and enterprise directory integration remain out of scope.

**Item types:** `US` is a user story. `EN` is a technical enabler: work that users do not see but that the stories depend on.

**Priority:** We use MoSCoW. An epic or feature takes the priority of its highest-priority item.

| Priority | Meaning |
|---|---|
| Must | Required for the MVP or the final submission |
| Should | Important, but the product works without it |
| Could | Useful if time remains |
| Won't (this release) | Deliberately left for a later release |

**Delivery dates:** The supplied QA report attributes a Sprint 3 (end-of-Week-8) MVP and Week-10 final deadline to the course specification; verify actual dates and requirements from the instructor’s current documents.

**Estimate:** Story points on the scale 1, 2, 3, 5, 8. They compare relative size and are not hours. An item above 8 points must be split.

**Rank:** The proposed build order. Every item comes after the items it depends on.

**Owner:** Proposed implementation coordinator for the item, **not** its sole coder or RACI Accountable by default. The Product Owner confirms priorities; the team confirms estimates and owners.

**Core vocabulary (proposed contract; confirm against System Vision & Boundary and OpenAPI):** `Report` is an individual submitted email notification; `Case` is the SOC investigation record; each Report creates exactly one Case (1:1), and reports of the same email from different employees remain separate Cases. `analysis_status` (`pending`, `completed`, `failed`) is separate from `case_status` (`New`, `Under Review`, `Resolved`). `risk_level` is `Low`, `Medium`, or `Critical`, and is **advisory**. `verdict` (the System Vision's final classification: `phishing`, `legitimate`, `inconclusive`) belongs to an authorized analyst. Only an analyst can resolve a Case, and only with a recorded verdict. Reports from unverified forwarding sources remain unattributed and cannot appear in an employee’s personal history without independent attribution. API representations may use normalized enum strings, but one mapping must be agreed in the API contract.

## 3. Summary

| Priority | Items | Story points |
|---|---:|---:|
| Must | 27 | 95 |
| Should | 0 | 0 |
| Could | 2 | 6 |
| **Total** | **29** | **101** |

| Owner | Must SP | All SP |
|---|---:|---:|
| Burak | 18 | 18 |
| Çağan | 17 | 17 |
| Muhammed Ali | 21 | 24 |
| Nil | 14 | 14 |
| Sema | 12 | 15 |
| Ezgi | 13 | 13 |


## 4. Epics and Features

Epics are listed in build order, using the rank of each epic's first item.

| Epic | Priority | Items | SP |
|---|---|---:|---:|
| E6 Delivery Foundation | Must | 7 | 25 |
| E1 Access and Security | Must | 3 | 9 |
| E2 Email Reporting | Must | 5 | 21 |
| E4 Triage and Case Management | Must | 7 | 19 |
| E3 AI Analysis | Must | 3 | 16 |
| E5 Audit and Insight | Must | 4 | 11 |

| Feature | Epic | Priority | Items | SP |
|---|---|---|---|---:|
| F6.1 Technical decisions | E6 | Must | EN-01 | 2 |
| F6.2 Environment and data | E6 | Must | EN-02, EN-05 | 8 |
| F6.3 Automation and testing | E6 | Must | EN-03, EN-04, EN-06, EN-07 | 15 |
| F1.1 Authentication | E1 | Must | US-01 | 3 |
| F1.2 Role-based access | E1 | Must | US-02 | 3 |
| F1.3 Data protection | E1 | Must | US-03 | 3 |
| F2.1 Report submission | E2 | Must | US-04, US-05, US-07, US-22 | 18 |
| F2.2 Report tracking | E2 | Must | US-06 | 3 |
| F4.1 Report queue | E4 | Must | US-11, US-12, US-13 | 8 |
| F4.2 Case workflow | E4 | Must | US-14, US-15, US-16, US-17 | 11 |
| F3.1 Indicator extraction | E3 | Must | US-08 | 5 |
| F3.2 Risk analysis | E3 | Must | US-09 | 8 |
| F3.3 Analysis results | E3 | Must | US-10 | 3 |
| F5.1 Action history | E5 | Must | US-18, US-19 | 5 |
| F5.2 Notifications and metrics | E5 | Could | US-20, US-21 | 6 |

## 5. Prioritized Backlog

Implementation starts with EN-01 and EN-02, and the first user story is US-01.

| Rank | ID | Item | Priority | SP | Owner | Depends on |
|---:|---|---|---|---:|---|---|
| 1 | EN-01 | Decide AI component approach | Must | 2 | Burak | None |
| 2 | EN-02 | Set up environment and data model | Must | 5 | Çağan | None |
| 3 | EN-03 | Set up CI pipeline | Must | 5 | Muhammed Ali | EN-02 |
| 4 | EN-05 | Containerize and deploy the product | Must | 3 | Muhammed Ali | EN-02, EN-03 |
| 5 | EN-04 | Set up test strategy and API contract tests | Must | 3 | Sema | EN-03 |
| 6 | EN-07 | Add static code analysis | Must | 2 | Muhammed Ali | EN-03 |
| 7 | US-01 | Sign in | Must | 3 | Çağan | EN-02 |
| 8 | US-02 | Restrict access by role | Must | 3 | Çağan | US-01 |
| 9 | US-04 | Submit a suspicious email | Must | 5 | Nil | EN-02, US-01 |
| 10 | US-11 | List reported emails | Must | 3 | Nil | US-02, US-04 |
| 11 | US-13 | View report details | Must | 2 | Ezgi | US-02, US-04 |
| 12 | US-08 | Extract email indicators | Must | 5 | Burak | EN-01, US-04 |
| 13 | US-09 | Run AI risk analysis | Must | 8 | Burak | EN-01, EN-04, US-08, US-13 |
| 14 | US-10 | View risk level and reasons | Must | 3 | Ezgi | US-09, US-13 |
| 15 | US-14 | Start and manage analyst case review | Must | 3 | Çağan | US-13 |
| 16 | US-03 | Protect reported email data | Must | 3 | Burak | EN-01, US-04 |
| 17 | US-15 | Record human verdict and resolve Case | Must | 3 | Çağan | US-10, US-14 |
| 18 | US-18 | Record action history | Must | 3 | Muhammed Ali | US-04, US-09, US-15 |
| 19 | US-12 | Filter and sort cases (MVP priority ordering) | Must | 3 | Nil | US-09, US-11 |
| 20 | US-22 | Ingest forwarded email reports safely | Must | 5 | Muhammed Ali (proposed) | EN-02, US-04 |
| 21 | US-07 | Prevent duplicate ingestion | Must | 3 | Nil | US-22 |
| 22 | US-05 | Upload the original email file | Must | 5 | Ezgi | US-04 |
| 23 | US-06 | Track my reports | Must | 3 | Ezgi | US-02, US-04 |
| 24 | US-16 | Assign a report to an analyst | Must | 3 | Muhammed Ali | US-14 |
| 25 | US-19 | View action history | Must | 2 | Sema | US-18 |
| 26 | US-17 | Add analyst notes | Must | 2 | Sema | US-13 |
| 27 | EN-06 | Add integration and end-to-end tests | Must | 5 | Sema | EN-04, EN-05, US-10, US-15, US-22, US-07 |
| 28 | US-20 | Notify the employee of the outcome | Could | 3 | Muhammed Ali | US-15 |
| 29 | US-21 | View triage metrics | Could | 3 | Sema | US-09, US-11 |

## 6. Item Details

Status names, risk levels, and API paths marked with `*` are proposals. They must match `openapi.yaml` once that file is final.

### E6 Delivery Foundation

#### EN-01: Decide AI component approach

> As the development team, I want to decide how the AI analysis component will work so that the architecture and API can be finalized.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Burak | None | None |

**Acceptance criteria**

- `SYSTEM_VISION_BOUNDARY.md` states how the AI component is realized: the System Vision keeps risk-assessment orchestration inside the boundary and treats any third-party model-inference provider as optional and external, so the decision records whether the approach is rule-based, ML-based or hybrid and whether an external provider is used.
- The chosen model or service is named, with the reason for choosing it.
- The decision states whether analysis is synchronous or asynchronous.
- The decision lists which email data is sent to the AI component.

**Tasks**

- Compare at least two AI options by cost, accuracy, and data privacy.
- Record the decision and review it with Çağan.

#### EN-02: Set up environment and data model

> As a developer, I want a shared environment and data model so that everyone builds against the same setup.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Çağan | None | None |

**Acceptance criteria**

- A new team member can start the application locally by following the README.
- The data model covers users, reports, cases (one-to-one with reports), risk assessments, and append-only case history.
- Analysis and case lifecycle statuses are modeled separately; attribution status is recorded on incoming forwarded reports.
- Database changes are applied through versioned migrations.
- Secrets are not stored in the repository.

**Tasks**

- Choose and scaffold the application stack.
- Design the database schema and create the first migration.
- Document local setup in the README.

#### EN-03: Set up CI pipeline

> As a developer, I want automatic checks on every pull request so that problems are found before merging.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Muhammed Ali | EN-02 | None |

**Acceptance criteria**

- GitHub Actions runs linting, tests, and OpenAPI validation on every pull request to `develop` and `main`.
- The pipeline reports unit-test coverage and fails when coverage is below 50 percent.
- A failed check blocks the merge.
- The check results are visible on the pull request.

**Tasks**

- Create the GitHub Actions workflow.
- Add the coverage report and the 50 percent threshold.
- Add OpenAPI validation to the workflow.
- Make the checks required in the branch protection rules.

#### EN-04: Set up test strategy and API contract tests

> As the QA owner, I want a test strategy and contract tests so that the implementation stays consistent with the API contract.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Sema | EN-03 | All |

**Acceptance criteria**

- The test plan defines the unit, integration, and end-to-end test levels.
- A contract test job compares each implemented endpoint with `openapi.yaml`.
- Test data contains no real email content or personal data.
- The contract test job runs in the CI pipeline, and every new endpoint is added to it.

**Tasks**

- Write the test plan.
- Prepare safe sample phishing and legitimate emails.
- Add the contract test job to CI.

#### EN-05: Containerize and deploy the product

> As a developer, I want to start the whole product with one command so that it runs the same way in every environment.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | EN-02, EN-03 | None |

**Acceptance criteria**

- One Docker or docker-compose command starts the whole product.
- Stored data remains available after the containers restart.
- A merge to `main` triggers the delivery pipeline.
- The product runs in a live or demonstrable environment.

**Tasks**

- Write the Docker and docker-compose configuration.
- Add the delivery step to GitHub Actions.
- Document the start command in the README.

#### EN-06: Add integration and end-to-end tests

> As the QA owner, I want integration and end-to-end tests so that the primary user journey is verified before the demonstration.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Sema | EN-04, EN-05, US-10, US-15, US-22, US-07 | All |

**Acceptance criteria**

- Integration tests cover the API, the database, and the AI component boundary.
- End-to-end scenarios cover web-form submission and controlled test-mailbox forwarding, followed by analysis, analyst review and resolution; forwarded-message redelivery must not create a duplicate Report or Case.
- A repeatable load test checks the System Vision NFR-07 targets (10 concurrent users, 1,000 stored reports: 95% of manual report-creation requests within 3 seconds and 95% of dashboard list requests within 2 seconds).
- The tests run automatically in the pipeline.
- The tests produce the report required for the final submission.

**Tasks**

- Write the integration tests.
- Write the end-to-end test for the primary user journey.
- Write the NFR-07 load test.
- Generate the test report.

#### EN-07: Add static code analysis

> As a developer, I want static code analysis so that the team can find code issues and track technical debt.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Muhammed Ali | EN-03 | None |

**Acceptance criteria**

- SonarQube analyzes the code.
- The team can produce a static analysis and technical-debt report.

**Tasks**

- Configure SonarQube for the repository.
- Generate the first report and review its issues.

### E1 Access and Security

#### US-01: Sign in

> As a user, I want to sign in so that the system knows who I am and what I can access.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | EN-02 | `POST /auth/login`* |

**Acceptance criteria**

- Valid credentials return an access token.
- Invalid credentials return `401` and do not reveal which value was wrong.
- A protected endpoint called with a missing or expired token returns `401`.
- Passwords are stored only as salted hashes, using an algorithm such as bcrypt or Argon2.

**Tasks**

- Implement the login endpoint and token validation.
- Create the sign-in screen.
- Add seed accounts for the employee and analyst roles.

#### US-02: Restrict access by role

> As a security analyst, I want access to reported emails restricted by role so that only authorized people can read them.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-01 | All `/reports` endpoints |

**Acceptance criteria**

- An employee can access only reports that are securely attributed to their authenticated account. Unverified forwarded reports are excluded from employee history.
- An employee who requests another person's report receives `403`.
- An employee who calls an analyst-only operation receives `403`.
- An authorized analyst can access reports and associated cases; administrator permissions are separately scoped and do not automatically grant case-resolution rights.

**Tasks**

- Add role checks to the API.
- Hide analyst screens from employees.
- Add tests for each role and endpoint combination.

#### US-03: Protect reported email data

> As an employee, I want the content of my reported email to be protected so that personal data is not exposed.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Burak | EN-01, US-04 | None |

This item is **Must regardless of AI hosting**. External AI use adds a separate privacy/security review, minimization and provider assessment; it does not make baseline protections optional.

**Acceptance criteria**

- Only the data listed in EN-01 is sent to the AI component.
- Email content does not appear in application logs.
- A documented retention period defines when report content is deleted.
- The application is reached only through HTTPS outside local development.

**Tasks**

- Define the data retention rule.
- Remove email content from logs.
- Limit the fields sent for AI analysis.

### E2 Email Reporting

#### US-04: Submit a suspicious email

> As an employee, I want to submit a suspicious email so that the security team can investigate it.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Nil | EN-02, US-01 | `POST /reports` |

**Acceptance criteria**

- A valid authenticated submission creates a Report with a unique ID and exactly one new linked Case, with `analysis_status=pending` and `case_status=New`, and returns `201`.
- A submission with missing required fields returns `400`, names the invalid fields, and creates no report.
- A saved report records the authenticated reporter, submission time, attribution status and linked Case ID.
- The saved report can be retrieved after the application restarts.
- The employee sees a confirmation containing the report ID.

**Tasks**

- Define required fields with Çağan.
- Implement the endpoint and validation.
- Create the submission form and confirmation screen.

#### US-05: Upload the original email file

> As an employee, I want to upload the original email file so that analysts receive its headers and links unchanged.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Ezgi | US-04 | `POST /reports` |

**Acceptance criteria**

- A valid `.eml` file is accepted and linked to the report.
- A file of another type returns `415` and creates no report.
- A file above the agreed size limit returns `413` and creates no report.
- The system only parses the file as text. It never opens the email's attachments or runs its content.

**Tasks**

- Agree the file size limit.
- Add file upload to the form and the API.
- Store uploaded files safely.

#### US-06: Track my reports

> As an employee, I want to see the status of my reports so that I know whether they were reviewed.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Ezgi | US-02, US-04 | `GET /reports`, `GET /reports/{id}` |

**Acceptance criteria**

- The employee sees only reports that can be authenticated as their own, newest first; unverified forwarded reports are never automatically linked by sender header alone.
- Each row shows Report ID, subject, submission date and the associated Case status, without internal analyst notes or unverified attribution.
- An employee with no reports sees an empty-state message.
- AI results, analyst notes, and action history are not shown to the employee.

**Tasks**

- Create the "My reports" screen.
- Filter API results by the signed-in user.

#### US-07: Prevent duplicate ingestion

> As a security analyst, I want the same forwarded mailbox message not to be processed twice so that the queue contains no accidental duplicates.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-22 | Inbox worker (not necessarily a public REST endpoint) |

**Acceptance criteria**

- Ingestion is idempotent by provider message identity where available; the same mailbox message never creates a second Report or Case.
- Reports of the same suspicious email from different employees remain separate Reports and Cases (1:1); cross-employee case merging is out of MVP scope.
- A message that cannot be matched as a duplicate is still saved, so no employee report is lost.

**Tasks**

- Define the idempotency rule.
- Store the provider message identifier with the report; enforce a uniqueness/idempotency key and test redelivery/retry behavior.

### E3 AI Analysis

#### US-08: Extract email indicators

> As a security analyst, I want the system to extract key indicators from an email so that I can see the suspicious elements quickly.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Burak | EN-01, US-04 | None (runs during intake) |

**Acceptance criteria**

- The system extracts the sender address and domain, reply-to address, subject, and URLs when they are present.
- A missing optional element does not stop the analysis.
- Malformed email content sets `analysis_status=failed` with a diagnostic visible to analysts; `case_status` does not change and the system does not crash.
- Links in the email are never opened during extraction.

**Tasks**

- Implement the extraction logic.
- Store the extracted indicators with the report.
- Test with valid, partial, and malformed emails.

#### US-09: Run AI risk analysis

> As a security analyst, I want the system to analyze a reported email with AI so that I can prioritize likely phishing attempts.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 8 SP | Burak | EN-01, EN-04, US-08, US-13 | `POST /reports/{id}/analysis` |

**Acceptance criteria**

- Analysis starts automatically after a Report is accepted and parsed; an analyst can retry it from the report detail screen if it failed.
- Successful analysis stores `Low`, `Medium`, or `Critical` risk, a score from 0 to 100, evidence-based indicators/reasons, and the model or ruleset version.
- Successful analysis changes `analysis_status` to `completed`; the Case remains in its human-managed lifecycle state.
- The success response follows the EN-01 decision: `200` for synchronous analysis or `202` for asynchronous analysis.
- Invalid model output is rejected and `analysis_status` becomes `failed`, without changing the Case lifecycle.
- On provider timeout or failure, the Report and Case remain saved, analysts can continue manual investigation, and retry is possible.
- Analysis is retried only from `failed`; a retry while analysis is `pending` or `completed` returns `409`. `analysis_status` stays `pending` until it becomes `completed` or `failed`.
- A request for a report that does not exist returns `404`.
- An analysis request from an employee returns `403`.

**Tasks**

- Integrate the AI component chosen in EN-01.
- Define the prompt or model input and the output format.
- Handle failures, timeouts, and retries.
- Add the retry action to the detail screen.
- Treat email content as untrusted input; never follow instructions embedded in reported emails; validate structured model outputs and test prompt-injection samples.
- Evaluate on a held-out labeled dataset using one declared positive-class definition and identical precision/recall/F1 rules; record sample size, uncertainty and baseline.
- Decide on local versus external model, synchronous versus asynchronous analysis, and precisely what data leaves the trust boundary in EN-01.

#### US-10: View risk level and reasons

> As a security analyst, I want to see the risk level and the reasons behind it so that I can judge whether the AI result is reliable.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Ezgi | US-09, US-13 | `GET /reports/{id}` |

**Acceptance criteria**

- The report detail shows the risk level, score, reasons, and extracted indicators.
- The result is labeled as an AI-generated advisory score, separated from verified indicators and the analyst verdict; it never automatically resolves a case.
- A report without a completed result shows "not analyzed", "analysis running" or "analysis failed" rather than an invented risk level.

**Tasks**

- Add analysis results to the report response and detail screen.
- Add the empty and failed states.

### E4 Triage and Case Management

#### US-11: List reported emails

> As a security analyst, I want to list reported emails so that I can see what needs attention.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-02, US-04 | `GET /reports` |

**Acceptance criteria**

- The analyst queue shows Case ID, Report ID, subject summary, latest submission, `case_status`, `analysis_status` and advisory risk level, where available.
- Results are paginated.
- Cases are ordered by risk priority (`Critical` before `Medium` before `Low`), with unscored items visibly identifiable and accessible; newest-first is available as an alternative. This is a minimal MVP priority rule, not the full filtering interface.
- An empty list returns `200` with no items and shows an empty-state message.

**Tasks**

- Implement the paginated endpoint.
- Create the analyst queue screen.

#### US-12: Filter and sort cases (MVP priority ordering)

> As a security analyst, I want to filter and sort reports so that I can handle the highest-risk ones first.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-09, US-11 | `GET /reports` |

**Acceptance criteria**

- Cases can be filtered by `case_status`, `analysis_status` and advisory risk level.
- Analysts can change sort order between priority and date. Priority sorting ranks `Critical` ahead of `Medium` and `Low`; unanalyzed cases remain discoverable and are never silently hidden.
- An invalid filter value returns `400`.
- Filters remain applied when the analyst moves between pages.

**Tasks**

- Add query parameters to the endpoint.
- Add filter and sort controls to the queue.

#### US-13: View report details

> As a security analyst, I want to open a report so that I can see all of its information.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Ezgi | US-02, US-04 | `GET /reports/{id}` |

**Acceptance criteria**

- The detail shows the individual Report, its linked Case, reporter attribution (including unverified status), submission time, case lifecycle and analysis state, without exposing another employee’s report.
- A report that does not exist returns `404`.
- Email content is displayed as plain text, so scripts do not run and links are not active.

**Tasks**

- Implement the endpoint.
- Create the detail screen with safe content display.

#### US-14: Start and manage analyst case review

> As a security analyst, I want to update a report's status so that the team can see how far each case has progressed.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-13 | `PATCH /cases/{caseId}`* |

**Acceptance criteria**

- An authorized analyst can move a Case from `New` to `Under Review` whether AI analysis is pending, completed or failed.
- Case resolution is performed only through the verdict-and-resolve action in US-15, with the verdict saved in the same atomic transaction.
- Invalid lifecycle transitions return `409` without changing the Case; unknown enum values return `400`.
- Employee attempts return `403`; nonexistent Case IDs return `404`.

**Tasks**

- Implement Case lifecycle rules independently of `analysis_status`.
- Add analyst-only Case status controls to the detail screen.
- Test every allowed and blocked status change.

#### US-15: Record human verdict and resolve Case

> As a security analyst, I want to record my final verdict so that a person, not the AI, makes the final decision.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-10, US-14 | `PATCH /cases/{caseId}`* |

**Acceptance criteria**

- Only an authorized analyst can atomically record a `phishing`, `legitimate`, or `inconclusive` verdict and resolve a Case from `Under Review`; missing/invalid verdict returns `400` and illegal transitions return `409`.
- The analyst verdict may differ from the AI risk level; model output never becomes the final verdict automatically.
- The AI result remains immutable after case resolution; changes are recorded in action history.

**Tasks**

- Add the verdict to the Case model and analyst-only resolution API; capture analyst identity/time in the audit trail.
- Add a resolve action that requires verdict selection.

#### US-16: Assign a report to an analyst

> As a security analyst, I want to assign a report to an analyst so that the team knows who is handling it.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | US-14 | `PATCH /cases/{caseId}`* |

**Acceptance criteria**

- A Case can be assigned to any authorized user with the Security Analyst role.
- Assigning a report to a non-analyst or an unknown user returns `400`.
- The list and detail screens show the assignee.
- A report can be reassigned.

**Tasks**

- Add the assignee field to the API and data model.
- Add the assignment control and show it in the queue.

#### US-17: Add analyst notes

> As a security analyst, I want to add notes to a report so that my findings are available to the team.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Sema | US-13 | `POST /reports/{id}/notes`* |

**Acceptance criteria**

- An analyst can add a note to a report.
- Each note shows its author and time.
- An empty note returns `400`.
- Employees cannot see notes.

**Tasks**

- Implement the notes endpoint and storage.
- Add a notes section to the detail screen.

### E5 Audit and Insight

#### US-18: Record action history

> As a security analyst, I want the system to record every important action so that decisions can be traced later.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | US-04, US-09, US-15 | None |

**Acceptance criteria**

- The system records report creation, analysis runs, and status changes. Once US-15 and US-16 are implemented, it also records verdicts and assignments.
- Each entry stores the action, actor, time, and old and new values where they apply.
- For database state changes, write the event and corresponding state change atomically. External AI calls are not enclosed in long-lived database transactions; record analysis attempt and completion/failure in separate reliable events.
- The API offers no way to edit or delete history entries.

**Tasks**

- Create the history record.
- Save history entries in the same transaction as each change.

#### US-19: View action history

> As a security analyst, I want to view a report's history so that I can see who did what and when.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Sema | US-18 | `GET /reports/{id}/history`* |

**Acceptance criteria**

- The detail screen shows the report's history in time order.
- A report that does not exist returns `404`.
- An employee who requests history receives `403`.

**Tasks**

- Implement the history endpoint.
- Add the history timeline to the detail screen.

#### US-20: Notify the employee of the outcome

> As an employee, I want to be told when my report is resolved so that I know what to do with the email.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Could | 3 SP | Muhammed Ali | US-15 | None |

**Acceptance criteria**

- When a report is resolved, the reporter receives one notification.
- The notification contains the report ID and outcome.
- A failed notification does not change the report's status, and the failure is logged.

**Tasks**

- Choose the notification method.
- Send the notification when a report is resolved.

#### US-21: View triage metrics

> As a security analyst, I want to see summary metrics so that I can understand the current workload.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Could | 3 SP | Sema | US-09, US-11 | `GET /metrics/summary`* |

**Acceptance criteria**

- The dashboard shows report counts by status and by risk level.
- The figures match the data in the report list.
- An employee who requests metrics receives `403`.

**Tasks**

- Implement the summary endpoint.
- Create the metrics view.

#### US-22: Ingest forwarded email reports safely

> As an employee, I want to forward suspicious emails to the designated inbox so that I can report them without accessing the web application.

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Muhammed Ali (proposed; confirm) | EN-02, US-04 | Inbox worker (not necessarily a public REST endpoint) |

**Acceptance criteria**

- The system ingests an email forwarded to an approved reporting mailbox and stores it as a Report linked to a Case; malformed input is quarantined/logged safely without executing active content.
- The forwarding envelope/header `From` value alone does **not** authenticate an employee. The Report stores a `submitter_attribution_status` (`Verified`, `Unverified`); a directory address match is not proof of identity.
- Unverified forwarded Reports remain available to authorized analysts but never appear as an employee's own reports until independently verified.
- The implementation defines mailbox polling/webhook authentication, rate limits and operational failure handling; US-07 enforces idempotent ingestion as a Must prerequisite for completing the forwarding workflow.
- Actual enterprise mailbox integration may use a controlled test inbox for MVP demonstration; external provider availability is an explicit deployment assumption.

**Tasks**

- Build a safe test-mailbox ingestion adapter; parse forwarded original message when present.
- Persist Report/Case association and attribution status; document authentication and failure states.
- Test forged sender headers and malformed MIME input; verify repeat delivery through US-07 acceptance tests.

## 7. Won't (This Release)

These items need confirmation against the final MVP scope in the Proposal and System Boundary documents.

| Item | Reason |
|---|---|
| Automatic email quarantine | The team plan suggests leaving it for a later release |
| Mail client button or add-in | The web form and mailbox forwarding cover reporting for the MVP |
| Single sign-on with a company directory | Seed accounts are enough for the MVP; directory address matching is not authentication |
| Training a custom AI model | The MVP uses the approach chosen in EN-01 |
| Administration screens for users | System Administrator responsibilities are defined; full management UI is deferred, and operational admin configuration uses controlled setup |

## 8. Definition of Ready

An item can enter a sprint only when all of these are true:

- [ ] Its user, capability, and value are explicit.
- [ ] It is linked to an epic and a feature.
- [ ] Its priority has been approved by the Product Owner.
- [ ] Its acceptance criteria are testable and include failure cases.
- [ ] It has been estimated by the team and is 8 SP or smaller, so the team can complete it within one sprint.
- [ ] Its dependencies are finished or planned earlier in the same sprint.
- [ ] Its unresolved questions have owners.
- [ ] It has one named owner.
- [ ] Its API changes are reflected in `openapi.yaml`.
- [ ] Its required test data is available, or a task exists to create it.

## 9. Traceability

### Planned product functions

| Function in the team plan | Items |
|---|---|
| Employee reports a suspicious email | US-04, US-22 (forwarded inbox) |
| Analyst prioritizes and lists cases | US-11, US-12 (priority default) |
| System analyzes the reported email | US-08, US-09 |
| Analyst views the risk level | US-10 |
| Analyst changes case status and records human verdict | US-14, US-15 |
| System stores action history | US-18 |

### Course specification requirements

| Requirement | Items |
|---|---|
| Starts with one Docker or docker-compose command | EN-05 |
| Runs in a live or demonstrable environment | EN-05 |
| GitHub Actions continuous integration | EN-03 |
| At least 50 percent unit-test coverage | EN-03 |
| Continuous delivery pipeline | EN-05 |
| Unit, integration, and end-to-end tests | EN-04, EN-06 |
| Automated integration and end-to-end test report | EN-06 |
| SonarQube analysis and technical-debt report | EN-07 |

### API coverage

| Endpoint | Items | Source |
|---|---|---|
| `POST /reports` | US-04, US-05 | Team plan |
| `GET /reports` | US-06, US-11, US-12 | Team plan |
| `GET /reports/{id}` | US-06, US-10, US-13 | Team plan; include linked Case ID/summary |
| `POST /reports/{id}/analysis` | US-09 | Team plan (retry of the automatic analysis) |
| `PATCH /cases/{caseId}`* | US-14, US-15, US-16 | Proposed distinct Case resource; align OpenAPI |
| `POST /auth/login`* | US-01 | Proposed |
| `POST /reports/{id}/notes`* | US-17 | Proposed |
| `GET /reports/{id}/history`* | US-19 | Proposed |
| `GET /metrics/summary`* | US-21 | Proposed |
| Forwarding inbox adapter | US-22 | Internal ingestion integration; not necessarily an API endpoint |

## 10. Assumptions and Open Decisions

| Topic | Current assumption | Decision owner |
|---|---|---|
| MVP scope | Web submission and forwarding intake, basic privacy, authenticated employee access, separate Report/Case models, AI advisory analysis, risk-prioritized queue, analyst verdict and case resolution, action history | Ezgi (product approval), Burak (architecture feasibility) |
| AI component | Undecided until EN-01; define model/provider, synchronous/asynchronous behavior, data transferred, timeout and degraded mode. Analysis state is separate from Case lifecycle | Burak |
| User accounts | Seeded accounts for authentication; Employee, Security Analyst and System Administrator permissions are distinct. Admin UI out of scope | Burak |
| Report and case | Per System Vision: a Report is one submission and each Report creates exactly one Case (1:1); cross-employee merging is out of MVP scope | Burak, Çağan |
| Status values | `analysis_status`: `pending`, `completed`, `failed`; `case_status`: `New`, `Under Review`, `Resolved` | Burak, Çağan |
| Risk levels | `Low`, `Medium`, `Critical` (exact serialization to be locked in OpenAPI) | Burak, Çağan |
| Item owners | Proposed owners in this backlog; no role implies solo implementation; confirm capacities and RACI | Sema, Nil |
| Human verdict | Analyst `phishing` / `legitimate` / `inconclusive` is mandatory for Case resolution; AI remains advisory | Ezgi, Burak |
| Forwarded reports | Sender headers alone cannot prove identity; unverified reports excluded from employee status portal | Burak, Çağan |
| Official project rules | Instructor announcements, rubric, GitHub permissions and submission methods need checking from original course material | Nil |

## 11. Revision and Acceptance Gate

This is a **revised proposal for team review**, not an approved architecture or instructor-verified requirement. Before considering the backlog baselined:

1. **Burak / Çağan:** Compare the proposed Case endpoints with the OpenAPI Draft; Report–Case cardinality, status fields and risk levels already follow the System Vision & Boundary. Record any deliberate deviation instead of silently changing those documents.
2. **Ezgi (Product Owner):** Approve Must/Should/Could priorities and confirm forwarding, risk prioritization, analyst verdict, and privacy are part of the intended MVP.
3. **Team / Nil:** Re-estimate any changed items (especially US-22 and the enlarged Case data model); approve ownership and check implementation capacity. The existing point estimates were preserved as provisional; 101 points do not equal a schedule commitment; US-07 is now Must and EN-06 requires US-22 and US-07.
4. **Sema:** Update the QA report and acceptance tests; link backlog IDs to real GitHub issues/board items, avoiding two competing sources of truth.
5. **Nil / Muhammed Ali:** Confirm instructor and assistants' actual repository, PR, branch protection, ZIP, release tag and submission requirements from original announcements.

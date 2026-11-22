# Product Backlog

- **Project:** AI-Assisted Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026[cite: 4]
- **Team:** Ezgi, Nil, Burak, Muhammed Ali, Sema, Çağan[cite: 4]
- **Prepared by:** Sema (Sprint 1 Draft) | **Updated & Prioritized by:** Ezgi Naz Aydugan (Product Owner, Sprint 2)[cite: 1, 4]
- **Version:** v2.0 (Sprint 2 Baselined)
- **Status:** Product Owner approved priorities and MVP scope; aligned with `SYSTEM_VISION_BOUNDARY.md` (v1.1), `openapi.yaml` (v1.1.0), and `SRS.md` (v1.0)[cite: 3, 5].
- **Revision basis:** Sprint 1 backlog (v1.3), QA findings Q-01–Q-29, and OpenAPI v1.1.0 contract verification[cite: 3, 4].

## 1. Purpose

This backlog defines the product work for our phishing triage system[cite: 4]. Employees report suspicious emails, the system analyzes them with AI, and security analysts use the results to manage each report[cite: 4]. It covers the product we will build, not the Sprint 1/Sprint 2 documents, which we track separately in GitHub Issues[cite: 1, 4].

## 2. How to Read This Backlog

**Hierarchy:** Epic > Feature > User Story > Task[cite: 4].

**User roles:** `Employee` submits reports and may see only reports tied to their authenticated identity[cite: 4]; `Security Analyst` (IT/SOC) investigates cases and makes final decisions[cite: 4]; `System Administrator` manages accounts, roles and operational configuration without automatically inheriting analyst case-decision authority[cite: 4]. Forwarded emails do not establish employee identity from the `From` field alone[cite: 4]. Admin UI and enterprise directory integration remain out of scope[cite: 4].

**Item types:** `US` is a user story[cite: 4]. `EN` is a technical enabler: work that users do not see but that the stories depend on[cite: 4].

**Priority:** We use MoSCoW[cite: 4]. An epic or feature takes the priority of its highest-priority item[cite: 4].

| Priority | Meaning |
|---|---|
| Must | Required for the MVP or the final submission[cite: 4] |
| Should | Important, but the product works without it[cite: 4] |
| Could | Useful if time remains[cite: 4] |
| Won't (this release) | Deliberately left for a later release[cite: 4] |

**Delivery dates:** The supplied QA report attributes a Sprint 3 (end-of-Week-8) MVP and Week-10 final deadline to the course specification; verify actual dates and requirements from the instructor’s current documents[cite: 4].

**Estimate:** Story points on the scale 1, 2, 3, 5, 8[cite: 4]. They compare relative size and are not hours[cite: 4]. An item above 8 points must be split[cite: 4].

**Rank:** The proposed build order[cite: 4]. Every item comes after the items it depends on[cite: 4].

**Owner:** Proposed implementation coordinator for the item, **not** its sole coder or RACI Accountable by default[cite: 4]. The Product Owner confirms priorities; the team confirms estimates and owners[cite: 4].

**Core vocabulary (aligned with `SYSTEM_VISION_BOUNDARY.md` v1.1 and `openapi.yaml` v1.1.0):** `Report` is an individual submitted email notification[cite: 4]; `Case` is the SOC investigation record[cite: 4]; each Report creates exactly one Case (1:1), and reports of the same email from different employees remain separate Cases[cite: 3, 4, 5]. `analysis_status` (`Pending`, `Completed`, `Failed`) is separate from `investigation_status` (`New`, `Under Review`, `Resolved`)[cite: 3, 4]. `risk_category` is `Low`, `Medium`, or `Critical`, and is **advisory**[cite: 3, 4]. `final_classification` (`Phishing`, `Legitimate`, `Inconclusive`) belongs to an authorized analyst[cite: 3, 4]. Only an analyst can resolve a Case, and only with a recorded `final_classification`[cite: 3, 4]. Reports from unverified forwarding sources remain `Unverified` and cannot appear in an employee’s personal history without independent attribution[cite: 3, 4].

## 3. Summary

| Priority | Items | Story points |
|---|---:|---:|
| Must | 27 | 95[cite: 4] |
| Should | 0 | 0[cite: 4] |
| Could | 2 | 6[cite: 4] |
| **Total** | **29** | **101**[cite: 4] |

| Owner | Must SP | All SP |
|---|---:|---:|
| Burak | 18 | 18[cite: 4] |
| Çağan | 17 | 17[cite: 4] |
| Muhammed Ali | 21 | 24[cite: 4] |
| Nil | 14 | 14[cite: 4] |
| Sema | 12 | 15[cite: 4] |
| Ezgi | 13 | 13[cite: 4] |

## 4. Epics and Features

Epics are listed in build order, using the rank of each epic's first item[cite: 4].

| Epic | Priority | Items | SP |
|---|---|---:|---:|
| E6 Delivery Foundation | Must | 7 | 25[cite: 4] |
| E1 Access and Security | Must | 3 | 9[cite: 4] |
| E2 Email Reporting | Must | 5 | 21[cite: 4] |
| E4 Triage and Case Management | Must | 7 | 19[cite: 4] |
| E3 AI Analysis | Must | 3 | 16[cite: 4] |
| E5 Audit and Insight | Must | 4 | 11[cite: 4] |

| Feature | Epic | Priority | Items | SP |
|---|---|---|---|---:|
| F6.1 Technical decisions | E6 | Must | EN-01 | 2[cite: 4] |
| F6.2 Environment and data | E6 | Must | EN-02, EN-05 | 8[cite: 4] |
| F6.3 Automation and testing | E6 | Must | EN-03, EN-04, EN-06, EN-07 | 15[cite: 4] |
| F1.1 Authentication | E1 | Must | US-01 | 3[cite: 4] |
| F1.2 Role-based access | E1 | Must | US-02 | 3[cite: 4] |
| F1.3 Data protection | E1 | Must | US-03 | 3[cite: 4] |
| F2.1 Report submission | E2 | Must | US-04, US-05, US-07, US-22 | 18[cite: 4] |
| F2.2 Report tracking | E2 | Must | US-06 | 3[cite: 4] |
| F4.1 Report queue | E4 | Must | US-11, US-12, US-13 | 8[cite: 4] |
| F4.2 Case workflow | E4 | Must | US-14, US-15, US-16, US-17 | 11[cite: 4] |
| F3.1 Indicator extraction | E3 | Must | US-08 | 5[cite: 4] |
| F3.2 Risk analysis | E3 | Must | US-09 | 8[cite: 4] |
| F3.3 Analysis results | E3 | Must | US-10 | 3[cite: 4] |
| F5.1 Action history | E5 | Must | US-18, US-19 | 5[cite: 4] |
| F5.2 Notifications and metrics | E5 | Could | US-20, US-21 | 6[cite: 4] |

## 5. Prioritized Backlog

Implementation starts with EN-01 and EN-02, and the first user story is US-01[cite: 4].

| Rank | ID | Item | Priority | SP | Owner | Depends on |
|---:|---|---|---|---:|---|---|
| 1 | EN-01 | Decide AI component approach | Must | 2 | Burak | None[cite: 4] |
| 2 | EN-02 | Set up environment and data model | Must | 5 | Çağan | None[cite: 4] |
| 3 | EN-03 | Set up CI pipeline | Must | 5 | Muhammed Ali | EN-02[cite: 4] |
| 4 | EN-05 | Containerize and deploy the product | Must | 3 | Muhammed Ali | EN-02, EN-03[cite: 4] |
| 5 | EN-04 | Set up test strategy and API contract tests | Must | 3 | Sema | EN-03[cite: 4] |
| 6 | EN-07 | Add static code analysis | Must | 2 | Muhammed Ali | EN-03[cite: 4] |
| 7 | US-01 | Sign in | Must | 3 | Çağan | EN-02[cite: 4] |
| 8 | US-02 | Restrict access by role | Must | 3 | Çağan | US-01[cite: 4] |
| 9 | US-04 | Submit a suspicious email | Must | 5 | Nil | EN-02, US-01[cite: 4] |
| 10 | US-11 | List reported emails | Must | 3 | Nil | US-02, US-04[cite: 4] |
| 11 | US-13 | View report details | Must | 2 | Ezgi | US-02, US-04[cite: 4] |
| 12 | US-08 | Extract email indicators | Must | 5 | Burak | EN-01, US-04[cite: 4] |
| 13 | US-09 | Run AI risk analysis | Must | 8 | Burak | EN-01, EN-04, US-08, US-13[cite: 4] |
| 14 | US-10 | View risk level and reasons | Must | 3 | Ezgi | US-09, US-13[cite: 4] |
| 15 | US-14 | Start and manage analyst case review | Must | 3 | Çağan | US-13[cite: 4] |
| 16 | US-03 | Protect reported email data | Must | 3 | Burak | EN-01, US-04[cite: 4] |
| 17 | US-15 | Record human verdict and resolve Case | Must | 3 | Çağan | US-10, US-14[cite: 4] |
| 18 | US-18 | Record action history | Must | 3 | Muhammed Ali | US-04, US-09, US-15[cite: 4] |
| 19 | US-12 | Filter and sort cases (MVP priority ordering) | Must | 3 | Nil | US-09, US-11[cite: 4] |
| 20 | US-22 | Ingest forwarded email reports safely | Must | 5 | Muhammed Ali (proposed) | EN-02, US-04[cite: 4] |
| 21 | US-07 | Prevent duplicate ingestion | Must | 3 | Nil | US-22[cite: 4] |
| 22 | US-05 | Upload the original email file | Must | 5 | Ezgi | US-04[cite: 4] |
| 23 | US-06 | Track my reports | Must | 3 | Ezgi | US-02, US-04[cite: 4] |
| 24 | US-16 | Assign a report to an analyst | Must | 3 | Muhammed Ali | US-14[cite: 4] |
| 25 | US-19 | View action history | Must | 2 | Sema | US-18[cite: 4] |
| 26 | US-17 | Add analyst notes | Must | 2 | Sema | US-13[cite: 4] |
| 27 | EN-06 | Add integration and end-to-end tests | Must | 5 | Sema | EN-04, EN-05, US-10, US-15, US-22, US-07[cite: 4] |
| 28 | US-20 | Notify the employee of the outcome | Could | 3 | Muhammed Ali | US-15[cite: 4] |
| 29 | US-21 | View triage metrics | Could | 3 | Sema | US-09, US-11[cite: 4] |

## 6. Item Details

All status names, risk categories, and API paths below are aligned with `openapi.yaml` (v1.1.0)[cite: 3].

### E6 Delivery Foundation

#### EN-01: Decide AI component approach

> As the development team, I want to decide how the AI analysis component will work so that the architecture and API can be finalized[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Burak | None | None[cite: 4] |

**Acceptance criteria**

- `SYSTEM_VISION_BOUNDARY.md` states how the AI component is realized: the System Vision keeps risk-assessment orchestration inside the boundary and treats any third-party model-inference provider as optional and external, so the decision records whether the approach is rule-based, ML-based or hybrid and whether an external provider is used[cite: 4].
- The chosen model or service is named, with the reason for choosing it[cite: 4].
- The decision states whether analysis is synchronous or asynchronous[cite: 4].
- The decision lists which email data is sent to the AI component[cite: 4].

**Tasks**

- Compare at least two AI options by cost, accuracy, and data privacy[cite: 4].
- Record the decision and review it with Çağan[cite: 4].

#### EN-02: Set up environment and data model

> As a developer, I want a shared environment and data model so that everyone builds against the same setup[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Çağan | None | None[cite: 4] |

**Acceptance criteria**

- A new team member can start the application locally by following the README[cite: 4].
- The data model covers users, reports, cases (one-to-one with reports), risk assessments, and append-only case history[cite: 4].
- Analysis and case lifecycle statuses are modeled separately; attribution status is recorded on incoming forwarded reports[cite: 4].
- Database changes are applied through versioned migrations[cite: 4].
- Secrets are not stored in the repository[cite: 4].

**Tasks**

- Choose and scaffold the application stack[cite: 4].
- Design the database schema and create the first migration[cite: 4].
- Document local setup in the README[cite: 4].

#### EN-03: Set up CI pipeline

> As a developer, I want automatic checks on every pull request so that problems are found before merging[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Muhammed Ali | EN-02 | None[cite: 4] |

**Acceptance criteria**

- GitHub Actions runs linting, tests, and OpenAPI validation on every pull request to `develop` and `main`[cite: 4].
- The pipeline reports unit-test coverage and fails when coverage is below 50 percent[cite: 4].
- A failed check blocks the merge[cite: 4].
- The check results are visible on the pull request[cite: 4].

**Tasks**

- Create the GitHub Actions workflow[cite: 4].
- Add the coverage report and the 50 percent threshold[cite: 4].
- Add OpenAPI validation to the workflow[cite: 4].
- Make the checks required in the branch protection rules[cite: 4].

#### EN-04: Set up test strategy and API contract tests

> As the QA owner, I want a test strategy and contract tests so that the implementation stays consistent with the API contract[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Sema | EN-03 | All[cite: 4] |

**Acceptance criteria**

- The test plan defines the unit, integration, and end-to-end test levels[cite: 4].
- A contract test job compares each implemented endpoint with `openapi.yaml`[cite: 4].
- Test data contains no real email content or personal data[cite: 4].
- The contract test job runs in the CI pipeline, and every new endpoint is added to it[cite: 4].

**Tasks**

- Write the test plan[cite: 4].
- Prepare safe sample phishing and legitimate emails[cite: 4].
- Add the contract test job to CI[cite: 4].

#### EN-05: Containerize and deploy the product

> As a developer, I want to start the whole product with one command so that it runs the same way in every environment[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | EN-02, EN-03 | None[cite: 4] |

**Acceptance criteria**

- One Docker or docker-compose command starts the whole product[cite: 4].
- Stored data remains available after the containers restart[cite: 4].
- A merge to `main` triggers the delivery pipeline[cite: 4].
- The product runs in a live or demonstrable environment[cite: 4].

**Tasks**

- Write the Docker and docker-compose configuration[cite: 4].
- Add the delivery step to GitHub Actions[cite: 4].
- Document the start command in the README[cite: 4].

#### EN-06: Add integration and end-to-end tests

> As the QA owner, I want integration and end-to-end tests so that the primary user journey is verified before the demonstration[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Sema | EN-04, EN-05, US-10, US-15, US-22, US-07 | All[cite: 4] |

**Acceptance criteria**

- Integration tests cover the API, the database, and the AI component boundary[cite: 4].
- End-to-end scenarios cover web-form submission and controlled test-mailbox forwarding, followed by analysis, analyst review and resolution; forwarded-message redelivery must not create a duplicate Report or Case[cite: 4].
- A repeatable load test checks the System Vision NFR-07 targets (10 concurrent users, 1,000 stored reports: 95% of manual report-creation requests within 3 seconds and 95% of dashboard list requests within 2 seconds)[cite: 4].
- The tests run automatically in the pipeline[cite: 4].
- The tests produce the report required for the final submission[cite: 4].

**Tasks**

- Write the integration tests[cite: 4].
- Write the end-to-end test for the primary user journey[cite: 4].
- Write the NFR-07 load test[cite: 4].
- Generate the test report[cite: 4].

#### EN-07: Add static code analysis

> As a developer, I want static code analysis so that the team can find code issues and track technical debt[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Muhammed Ali | EN-03 | None[cite: 4] |

**Acceptance criteria**

- SonarQube analyzes the code[cite: 4].
- The team can produce a static analysis and technical-debt report[cite: 4].

**Tasks**

- Configure SonarQube for the repository[cite: 4].
- Generate the first report and review its issues[cite: 4].

### E1 Access and Security

#### US-01: Sign in

> As a user, I want to sign in so that the system knows who I am and what I can access[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | EN-02 | `POST /auth/login`[cite: 3, 4] |

**Acceptance criteria**

- Valid credentials return an access token[cite: 4].
- Invalid credentials return `401` and do not reveal which value was wrong[cite: 4].
- A protected endpoint called with a missing or expired token returns `401`[cite: 4].
- Passwords are stored only as salted hashes, using an algorithm such as bcrypt or Argon2[cite: 4].

**Tasks**

- Implement the login endpoint and token validation[cite: 4].
- Create the sign-in screen[cite: 4].
- Add seed accounts for the employee and analyst roles[cite: 4].

#### US-02: Restrict access by role

> As a security analyst, I want access to reported emails restricted by role so that only authorized people can read them[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-01 | All `/reports` and `/cases` endpoints[cite: 3, 4] |

**Acceptance criteria**

- An employee can access only reports that are securely attributed to their authenticated account. Unverified forwarded reports are excluded from employee history[cite: 4].
- An employee who requests another person's report receives `403`[cite: 4].
- An employee who calls an analyst-only operation receives `403`[cite: 4].
- An authorized analyst can access reports and associated cases; administrator permissions are separately scoped and do not automatically grant case-resolution rights[cite: 4].

**Tasks**

- Add role checks to the API[cite: 4].
- Hide analyst screens from employees[cite: 4].
- Add tests for each role and endpoint combination[cite: 4].

#### US-03: Protect reported email data

> As an employee, I want the content of my reported email to be protected so that personal data is not exposed[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Burak | EN-01, US-04 | None[cite: 4] |

This item is **Must regardless of AI hosting**[cite: 4]. External AI use adds a separate privacy/security review, minimization and provider assessment; it does not make baseline protections optional[cite: 4].

**Acceptance criteria**

- Only the data listed in EN-01 is sent to the AI component[cite: 4].
- Email content does not appear in application logs[cite: 4].
- A documented retention period defines when report content is deleted[cite: 4].
- The application is reached only through HTTPS outside local development[cite: 4].

**Tasks**

- Define the data retention rule[cite: 4].
- Remove email content from logs[cite: 4].
- Limit the fields sent for AI analysis[cite: 4].

### E2 Email Reporting

#### US-04: Submit a suspicious email

> As an employee, I want to submit a suspicious email so that the security team can investigate it[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Nil | EN-02, US-01 | `POST /reports`[cite: 4] |

**Acceptance criteria**

- A valid authenticated submission creates a Report with a unique ID and exactly one new linked Case, with `analysis_status=Pending` and `investigation_status=New`, and returns `201`[cite: 3, 4].
- A submission with missing required fields returns `400`, names the invalid fields, and creates no report[cite: 4].
- A saved report records the authenticated reporter, submission time, attribution status and linked Case ID[cite: 4].
- The saved report can be retrieved after the application restarts[cite: 4].
- The employee sees a confirmation containing the report ID[cite: 4].

**Tasks**

- Define required fields with Çağan[cite: 4].
- Implement the endpoint and validation[cite: 4].
- Create the submission form and confirmation screen[cite: 4].

#### US-05: Upload the original email file

> As an employee, I want to upload the original email file so that analysts receive its headers and links unchanged[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Ezgi | US-04 | `POST /reports`[cite: 4] |

**Acceptance criteria**

- A valid `.eml` file is accepted and linked to the report[cite: 4].
- A file of another type returns `415` and creates no report[cite: 4].
- A file above the agreed size limit returns `413` and creates no report[cite: 4].
- The system only parses the file as text. It never opens the email's attachments or runs its content[cite: 4].

**Tasks**

- Agree the file size limit[cite: 4].
- Add file upload to the form and the API[cite: 4].
- Store uploaded files safely[cite: 4].

#### US-06: Track my reports

> As an employee, I want to see the status of my reports so that I know whether they were reviewed[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Ezgi | US-02, US-04 | `GET /reports`, `GET /reports/{id}`[cite: 4] |

**Acceptance criteria**

- The employee sees only reports that can be authenticated as their own, newest first; unverified forwarded reports are never automatically linked by sender header alone[cite: 4].
- Each row shows Report ID, subject, submission date and the associated Case `investigation_status`, without internal analyst notes or unverified attribution[cite: 3, 4].
- An employee with no reports sees an empty-state message[cite: 4].
- AI results, analyst notes, and action history are not shown to the employee[cite: 4].

**Tasks**

- Create the "My reports" screen[cite: 4].
- Filter API results by the signed-in user[cite: 4].

#### US-07: Prevent duplicate ingestion

> As a security analyst, I want the same forwarded mailbox message not to be processed twice so that the queue contains no accidental duplicates[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-22 | Inbox worker (not a public REST endpoint)[cite: 3, 4] |

**Acceptance criteria**

- Ingestion is idempotent by provider message identity where available; the same mailbox message never creates a second Report or Case[cite: 4].
- Reports of the same suspicious email from different employees remain separate Reports and Cases (1:1); cross-employee case merging is out of MVP scope[cite: 4].
- A message that cannot be matched as a duplicate is still saved, so no employee report is lost[cite: 4].

**Tasks**

- Define the idempotency rule[cite: 4].
- Store the provider message identifier with the report; enforce a uniqueness/idempotency key and test redelivery/retry behavior[cite: 4].

### E3 AI Analysis

#### US-08: Extract email indicators

> As a security analyst, I want the system to extract key indicators from an email so that I can see the suspicious elements quickly[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Burak | EN-01, US-04 | None (runs during intake)[cite: 4] |

**Acceptance criteria**

- The system extracts the sender address and domain, reply-to address, subject, and URLs when they are present[cite: 4].
- A missing optional element does not stop the analysis[cite: 4].
- Malformed email content sets `analysis_status=Failed` with a diagnostic visible to analysts; `investigation_status` does not change and the system does not crash[cite: 3, 4].
- Links in the email are never opened during extraction[cite: 4].

**Tasks**

- Implement the extraction logic[cite: 4].
- Store the extracted indicators with the report[cite: 4].
- Test with valid, partial, and malformed emails[cite: 4].

#### US-09: Run AI risk analysis

> As a security analyst, I want the system to analyze a reported email with AI so that I can prioritize likely phishing attempts[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 8 SP | Burak | EN-01, EN-04, US-08, US-13 | `POST /reports/{id}/analysis`[cite: 4] |

**Acceptance criteria**

- Analysis starts automatically after a Report is accepted and parsed; an analyst can retry it from the report/case detail screen if it failed[cite: 4].
- Successful analysis stores `Low`, `Medium`, or `Critical` `risk_category`, a score from 0 to 100, evidence-based indicators/reasons, and the model or ruleset version[cite: 3, 4].
- Successful analysis changes `analysis_status` to `Completed`; the Case remains in its human-managed lifecycle state[cite: 3, 4].
- The success response follows the EN-01 decision: `200` for synchronous analysis or `202` for asynchronous analysis[cite: 4].
- Invalid model output is rejected and `analysis_status` becomes `Failed`, without changing the Case lifecycle[cite: 3, 4].
- On provider timeout or failure, the Report and Case remain saved, analysts can continue manual investigation, and retry is possible[cite: 4].
- Analysis is retried only from `Failed`; a retry while analysis is `Pending` or `Completed` returns `409`. `analysis_status` stays `Pending` until it becomes `Completed` or `Failed`[cite: 3, 4].
- A request for a report that does not exist returns `404`[cite: 4].
- An analysis request from an employee returns `403`[cite: 4].

**Tasks**

- Integrate the AI component chosen in EN-01[cite: 4].
- Define the prompt or model input and the output format[cite: 4].
- Handle failures, timeouts, and retries[cite: 4].
- Add the retry action to the detail screen[cite: 4].
- Treat email content as untrusted input; never follow instructions embedded in reported emails; validate structured model outputs and test prompt-injection samples[cite: 4].
- Evaluate on a held-out labeled dataset using one declared positive-class definition and identical precision/recall/F1 rules; record sample size, uncertainty and baseline[cite: 4].
- Decide on local versus external model, synchronous versus asynchronous analysis, and precisely what data leaves the trust boundary in EN-01[cite: 4].

#### US-10: View risk level and reasons

> As a security analyst, I want to see the risk level and the reasons behind it so that I can judge whether the AI result is reliable[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Ezgi | US-09, US-13 | `GET /reports/{id}`, `GET /cases/{id}`[cite: 3, 4] |

**Acceptance criteria**

- The detail view shows the advisory `risk_category`, `score`, `evidence` reasons, and extracted indicators[cite: 3, 4].
- The result is labeled as an AI-generated advisory score, separated from verified indicators and the analyst `final_classification`; it never automatically resolves a case[cite: 3, 4].
- A report without a completed result shows `Pending` ("analysis running") or `Failed` ("analysis failed" with `failure_reason`) rather than an invented risk level[cite: 3, 4].

**Tasks**

- Add analysis results to the report/case response and detail screen[cite: 3, 4].
- Add the empty and failed states[cite: 4].

### E4 Triage and Case Management

#### US-11: List reported emails

> As a security analyst, I want to list reported emails so that I can see what needs attention[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-02, US-04 | `GET /cases`, `GET /reports`[cite: 3, 4] |

**Acceptance criteria**

- The analyst queue shows Case ID, Report ID, subject summary, submission timestamp, `investigation_status`, `analysis_status` and advisory `risk_category`, where available[cite: 3, 4].
- Results are paginated[cite: 4].
- Cases are ordered by risk priority (`Critical` before `Medium` before `Low`), with unscored items visibly identifiable and accessible; `newest` is available as an alternative[cite: 3, 4].
- An empty list returns `200` with no items and shows an empty-state message[cite: 4].

**Tasks**

- Implement the paginated endpoint[cite: 4].
- Create the analyst queue screen[cite: 4].

#### US-12: Filter and sort cases (MVP priority ordering)

> As a security analyst, I want to filter and sort reports so that I can handle the highest-risk ones first[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Nil | US-09, US-11 | `GET /cases`[cite: 3, 4] |

**Acceptance criteria**

- Cases can be filtered by `investigation_status`, `analysis_status` and advisory `risk_category`[cite: 3, 4].
- Analysts can change sort order between `priority` and `newest`. Priority sorting ranks `Critical` ahead of `Medium` and `Low`; unanalyzed cases remain discoverable and are never silently hidden[cite: 3, 4].
- An invalid filter value returns `400`[cite: 4].
- Filters remain applied when the analyst moves between pages[cite: 4].

**Tasks**

- Add query parameters to the endpoint[cite: 4].
- Add filter and sort controls to the queue[cite: 4].

#### US-13: View report details

> As a security analyst, I want to open a report so that I can see all of its information[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Ezgi | US-02, US-04 | `GET /reports/{id}`, `GET /cases/{id}`[cite: 3, 4] |

**Acceptance criteria**

- The detail shows the individual Report, its linked Case, reporter attribution (`Verified` or `Unverified`), submission time, `investigation_status` and `analysis_status`, without exposing another employee’s report[cite: 3, 4].
- A report/case that does not exist returns `404`[cite: 3, 4].
- Email content is displayed as plain text, so scripts do not run and links are not active[cite: 4].

**Tasks**

- Implement the endpoint[cite: 4].
- Create the detail screen with safe content display[cite: 4].

#### US-14: Start and manage analyst case review

> As a security analyst, I want to update a report's status so that the team can see how far each case has progressed[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-13 | `PATCH /cases/{id}`[cite: 3, 4] |

**Acceptance criteria**

- An authorized analyst can move a Case from `New` to `Under Review` whether AI analysis is `Pending`, `Completed` or `Failed`[cite: 3, 4].
- Case resolution is performed only through the verdict-and-resolve action in US-15, with `final_classification` saved in the same atomic transaction[cite: 3, 4].
- Invalid lifecycle transitions return `409` without changing the Case; unknown enum values return `400`[cite: 4].
- Employee attempts return `403`; nonexistent Case IDs return `404`[cite: 4].

**Tasks**

- Implement Case lifecycle rules independently of `analysis_status`[cite: 4].
- Add analyst-only Case status controls to the detail screen[cite: 4].
- Test every allowed and blocked status change[cite: 4].

#### US-15: Record human verdict and resolve Case

> As a security analyst, I want to record my final verdict so that a person, not the AI, makes the final decision[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Çağan | US-10, US-14 | `PATCH /cases/{id}`[cite: 3, 4] |

**Acceptance criteria**

- Only an authorized analyst can atomically record a `Phishing`, `Legitimate`, or `Inconclusive` `final_classification` and resolve a Case from `Under Review`; missing/invalid classification returns `400` and illegal transitions return `409`[cite: 3, 4].
- The analyst `final_classification` may differ from the AI `risk_category`; model output never becomes the final decision automatically[cite: 3, 4].
- The AI result remains immutable after case resolution; changes are recorded in `CaseHistory`[cite: 3, 4].

**Tasks**

- Add `final_classification` to the Case model and analyst-only resolution API; capture analyst identity/time in the audit trail[cite: 3, 4].
- Add a resolve action that requires classification selection[cite: 3, 4].

#### US-16: Assign a report to an analyst

> As a security analyst, I want to assign a report to an analyst so that the team knows who is handling it[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | US-14 | `PATCH /cases/{id}`[cite: 3, 4] |

**Acceptance criteria**

- A Case can be assigned to any authorized user with the `Analyst` role[cite: 3, 4].
- Assigning a case to a non-analyst or an unknown user returns `400`[cite: 3, 4].
- The list and detail screens show the `assignee_id`[cite: 3, 4].
- A case can be reassigned[cite: 3, 4].

**Tasks**

- Add the assignee field to the API and data model[cite: 4].
- Add the assignment control and show it in the queue[cite: 4].

#### US-17: Add analyst notes

> As a security analyst, I want to add notes to a report so that my findings are available to the team[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Sema | US-13 | `POST /cases/{id}/notes`[cite: 3, 4] |

**Acceptance criteria**

- An analyst can add a note to a case[cite: 3, 4].
- Each note shows its `actor_id` and `timestamp`[cite: 3, 4].
- An empty note returns `400`[cite: 4].
- Employees cannot see notes[cite: 4].

**Tasks**

- Implement the notes endpoint and storage[cite: 4].
- Add a notes section to the detail screen[cite: 4].

### E5 Audit and Insight

#### US-18: Record action history

> As a security analyst, I want the system to record every important action so that decisions can be traced later[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 3 SP | Muhammed Ali | US-04, US-09, US-15 | None[cite: 4] |

**Acceptance criteria**

- The system records report creation, analysis runs, and status changes. Once US-15 and US-16 are implemented, it also records classifications and assignments[cite: 3, 4].
- Each entry stores the `action_type`, `actor_id`, `timestamp`, and `old_value` / `new_value` where they apply[cite: 3, 4].
- For database state changes, write the event and corresponding state change atomically. External AI calls are not enclosed in long-lived database transactions; record analysis attempt and completion/failure in separate reliable events[cite: 4].
- The API offers no way to edit or delete history entries[cite: 4].

**Tasks**

- Create the history record[cite: 4].
- Save history entries in the same transaction as each change[cite: 4].

#### US-19: View action history

> As a security analyst, I want to view a report's history so that I can see who did what and when[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 2 SP | Sema | US-18 | `GET /cases/{id}/history`[cite: 3, 4] |

**Acceptance criteria**

- The detail screen shows the case's history in time order (oldest first)[cite: 3, 4].
- A case that does not exist returns `404`[cite: 3, 4].
- An employee who requests history receives `403`[cite: 4].

**Tasks**

- Implement the history endpoint[cite: 4].
- Add the history timeline to the detail screen[cite: 4].

#### US-20: Notify the employee of the outcome

> As an employee, I want to be told when my report is resolved so that I know what to do with the email[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Could | 3 SP | Muhammed Ali | US-15 | None[cite: 4] |

**Acceptance criteria**

- When a report is resolved, the reporter receives one notification[cite: 4].
- The notification contains the report ID and outcome[cite: 4].
- A failed notification does not change the report's status, and the failure is logged[cite: 4].

**Tasks**

- Choose the notification method[cite: 4].
- Send the notification when a report is resolved[cite: 4].

#### US-21: View triage metrics

> As a security analyst, I want to see summary metrics so that I can understand the current workload[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Could | 3 SP | Sema | US-09, US-11 | `GET /metrics/summary`[cite: 3, 4] |

**Acceptance criteria**

- The dashboard shows case counts by `investigation_status` and by `risk_category`[cite: 3, 4].
- The figures match the data in the case list[cite: 3, 4].
- An employee who requests metrics receives `403`[cite: 4].

**Tasks**

- Implement the summary endpoint[cite: 4].
- Create the metrics view[cite: 4].

#### US-22: Ingest forwarded email reports safely

> As an employee, I want to forward suspicious emails to the designated inbox so that I can report them without accessing the web application[cite: 4].

| Priority | Estimate | Owner | Depends on | API |
|---|---|---|---|---|
| Must | 5 SP | Muhammed Ali (proposed; confirm) | EN-02, US-04 | Inbox worker (not a public REST endpoint)[cite: 3, 4] |

**Acceptance criteria**

- The system ingests an email forwarded to an approved reporting mailbox and stores it as a Report linked to a Case; malformed input is quarantined/logged safely without executing active content[cite: 4].
- The forwarding envelope/header `From` value alone does **not** authenticate an employee. The Report stores a `submitter_attribution_status` (`Verified`, `Unverified`); a directory address match is not proof of identity[cite: 4].
- Unverified forwarded Reports remain available to authorized analysts but never appear as an employee's own reports until independently verified[cite: 4].
- The implementation defines mailbox polling/webhook authentication, rate limits and operational failure handling; US-07 enforces idempotent ingestion as a Must prerequisite for completing the forwarding workflow[cite: 4].
- Actual enterprise mailbox integration may use a controlled test inbox for MVP demonstration; external provider availability is an explicit deployment assumption[cite: 4].

**Tasks**

- Build a safe test-mailbox ingestion adapter; parse forwarded original message when present[cite: 4].
- Persist Report/Case association and attribution status; document authentication and failure states[cite: 4].
- Test forged sender headers and malformed MIME input; verify repeat delivery through US-07 acceptance tests[cite: 4].

## 7. Won't (This Release)

Confirmed by Product Owner for Sprint 2 (`SRS.md` Section 2.2): the following capabilities remain explicitly out of scope for the MVP release[cite: 4].

| Item | Reason |
|---|---|
| Automatic email quarantine | Deferred to a later release; MVP focuses on decision support[cite: 4, 8] |
| Mail client button or add-in | The web form and mailbox forwarding cover reporting for the MVP[cite: 4] |
| Single sign-on with a company directory | Seed accounts are enough for the MVP; directory address matching is not authentication[cite: 4] |
| Training a custom AI model | The MVP uses the approach chosen in EN-01[cite: 4] |
| Administration screens for users | System Administrator responsibilities are defined; full management UI is deferred, and operational admin configuration uses controlled setup[cite: 4] |

## 8. Definition of Ready

An item can enter a sprint only when all of these are true[cite: 4]:

- [x] Its user, capability, and value are explicit[cite: 4].
- [x] It is linked to an epic and a feature[cite: 4].
- [x] Its priority has been approved by the Product Owner[cite: 4].
- [x] Its acceptance criteria are testable and include failure cases[cite: 4].
- [x] It has been estimated by the team and is 8 SP or smaller, so the team can complete it within one sprint[cite: 4].
- [x] Its dependencies are finished or planned earlier in the same sprint[cite: 4].
- [ ] Its unresolved questions have owners[cite: 4].
- [x] It has one named owner[cite: 4].
- [x] Its API changes are reflected in `openapi.yaml`[cite: 3, 4].
- [ ] Its required test data is available, or a task exists to create it[cite: 4].

## 9. Traceability

### Planned product functions

| Function in the team plan | Items |
|---|---|
| Employee reports a suspicious email | US-04, US-05, US-22 (forwarded inbox)[cite: 4] |
| Analyst prioritizes and lists cases | US-11, US-12 (priority default)[cite: 4] |
| System analyzes the reported email | US-08, US-09[cite: 4] |
| Analyst views the risk level | US-10[cite: 4] |
| Analyst changes case status and records human verdict | US-14, US-15[cite: 4] |
| System stores action history | US-18, US-19[cite: 4] |

### Course specification requirements

| Requirement | Items |
|---|---|
| Starts with one Docker or docker-compose command | EN-05[cite: 4] |
| Runs in a live or demonstrable environment | EN-05[cite: 4] |
| GitHub Actions continuous integration | EN-03[cite: 4] |
| At least 50 percent unit-test coverage | EN-03[cite: 4] |
| Continuous delivery pipeline | EN-05[cite: 4] |
| Unit, integration, and end-to-end tests | EN-04, EN-06[cite: 4] |
| Automated integration and end-to-end test report | EN-06[cite: 4] |
| SonarQube analysis and technical-debt report | EN-07[cite: 4] |

### API coverage

| Endpoint | Items | Source |
|---|---|---|
| `POST /auth/login` | US-01 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `POST /reports` | US-04, US-05 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /reports` | US-06, US-11 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /reports/{id}` | US-06, US-10, US-13 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `POST /reports/{id}/analysis` | US-09 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /cases` | US-11, US-12 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /cases/{id}` | US-10, US-13 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `PATCH /cases/{id}` | US-14, US-15, US-16 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `POST /cases/{id}/notes` | US-17 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /cases/{id}/history` | US-19 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| `GET /metrics/summary` | US-21 | `openapi.yaml` v1.1.0[cite: 3, 4] |
| Forwarding inbox adapter | US-07, US-22 | Internal ingestion worker[cite: 3, 4] |

## 10. Assumptions and Open Decisions

| Topic | Current assumption | Decision owner |
|---|---|---|
| MVP scope | Web submission and forwarding intake, basic privacy, authenticated employee access, separate Report/Case models, AI advisory analysis, risk-prioritized queue, analyst verdict and case resolution, action history[cite: 4] | Ezgi (approved), Burak (architecture feasibility)[cite: 4] |
| AI component | Undecided until EN-01; define model/provider, synchronous/asynchronous behavior, data transferred, timeout and degraded mode. Analysis state is separate from Case lifecycle[cite: 4] | Burak[cite: 4] |
| User accounts | Seeded accounts for authentication; Employee, Security Analyst and System Administrator permissions are distinct. Admin UI out of scope[cite: 4] | Burak[cite: 4] |
| Report and case | Per System Vision: a Report is one submission and each Report creates exactly one Case (1:1); cross-employee merging is out of MVP scope[cite: 4] | Burak, Çağan[cite: 4] |
| Status values | `analysis_status`: `Pending`, `Completed`, `Failed`; `investigation_status`: `New`, `Under Review`, `Resolved`[cite: 3, 4] | Burak, Çağan[cite: 4] |
| Risk levels & classification | `risk_category`: `Low`, `Medium`, `Critical`; `final_classification`: `Phishing`, `Legitimate`, `Inconclusive` (aligned with `openapi.yaml` v1.1.0)[cite: 3, 4] | Burak, Çağan[cite: 4] |
| Item owners | Proposed owners in this backlog; no role implies solo implementation; confirm capacities and RACI[cite: 4] | Sema, Nil[cite: 4] |
| Human verdict | Analyst `Phishing` / `Legitimate` / `Inconclusive` is mandatory for Case resolution; AI remains advisory[cite: 3, 4] | Ezgi (approved), Burak[cite: 4] |
| Forwarded reports | Sender headers alone cannot prove identity; unverified reports excluded from employee status portal[cite: 4] | Burak, Çağan[cite: 4] |
| Official project rules | Instructor announcements, rubric, GitHub permissions and submission methods need checking from original course material[cite: 4] | Nil[cite: 4] |

## 11. Sprint 2 Product Owner Sign-Off & Remaining Team Gates

1. **Ezgi (Product Owner — Completed):** Approved MoSCoW priorities (27 `Must` items / 95 SP, 2 `Could` items / 6 SP), confirmed MVP scope (dual-channel reporting, safe parsing, advisory AI risk scoring, prioritized analyst queue, mandatory human `final_classification`, and append-only `CaseHistory`), and aligned backlog terminology and endpoints with `openapi.yaml` (v1.1.0) and `SRS.md` (v1.0)[cite: 3, 4].
2. **Burak / Çağan:** Confirm technical design alignment (C4/UML diagrams and UI flows) with the baselined backlog and `openapi.yaml`[cite: 3].
3. **Team / Nil:** Track Sprint 2/3 implementation capacity and link backlog item IDs (`EN-01`–`EN-07`, `US-01`–`US-22`) to active GitHub Issues[cite: 4].
4. **Sema:** Verify NFRs and test scenarios against the updated acceptance criteria and `SRS.md`[cite: 4].
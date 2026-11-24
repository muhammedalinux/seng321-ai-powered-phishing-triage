# Non-Functional Requirements Specification (NFR)

## 1. Document Information

| Field | Value |
| --- | --- |
| Project | AI-Assisted Phishing Triage System |
| Course | SENG 321 – Software Engineering Project |
| Document | Non-Functional Requirements Specification |
| Version | 1.0 draft |
| Status | Sprint 2 working document |
| Related issue | GitHub Issue #19 |

This document defines measurable quality requirements for the AI-Assisted Phishing Triage System. It complements `docs/sprint2/SRS.md`, `docs/PROJECT_PROPOSAL.md`, and `docs/SYSTEM_VISION_BOUNDARY.md`.

The targets below are academic MVP targets and must be measured under the stated conditions. They are not claims that the system already satisfies the targets.

## 2. Quality Requirement Conventions

Each requirement has a unique identifier in the form `NFR-XXX`. A requirement is considered testable only when its target, conditions, and verification method are defined.

Unless otherwise stated:

- Measurements exclude third-party provider downtime and uncontrolled network latency.
- Tests use controlled, non-production data.
- Security and privacy tests must not expose real employee or organizational email content.
- Results, test conditions, sample sizes, and known limitations must be recorded with the verification evidence.

## 3. Security Requirements

### NFR-001 – Authentication protection

All protected API and dashboard operations shall require a valid authenticated session or access token.

**Target:** 100% of tested protected operations reject missing, invalid, and expired credentials with an unauthorized response.

**Verification:** Automated integration tests covering every protected route and an expired-token scenario.

### NFR-002 – Password protection

If the MVP stores passwords, it shall store only salted, one-way password hashes using an approved password-hashing algorithm such as Argon2id or bcrypt.

**Target:** No plaintext or reversibly encrypted password is present in the database, logs, API responses, or repository fixtures.

**Verification:** Database inspection, source-code review, secret-scanning checks, and authentication integration tests.

### NFR-003 – Role-based authorization

The backend shall enforce role permissions independently of frontend controls and client-supplied identifiers.

**Target:** 100% of authorization test cases prevent employees from accessing analyst/admin operations and prevent users from accessing another employee's report.

**Verification:** Negative integration tests for each role and direct API requests that bypass the UI.

### NFR-004 – Input validation

The system shall validate request fields, uploaded file types, sizes, and structured external-service responses before processing or persisting them.

**Target:** Malformed, oversized, unsupported, and schema-invalid inputs are rejected or isolated without an unhandled server error in all defined test cases.

**Verification:** Unit and integration tests using malformed MIME data, invalid JSON, unsupported files, oversized inputs, and unexpected enum values.

### NFR-005 – Untrusted email content

Email bodies, headers, URLs, and attachment metadata shall be treated as untrusted data and shall not control authorization, system instructions, tool calls, or case closure.

**Target:** Prompt-injection and content-manipulation test fixtures produce no privilege change, tool authorization, autonomous remediation, or unauthorized case transition.

**Verification:** Security test suite with adversarial email fixtures and review of analysis-to-action boundaries.

### NFR-006 – Secure transport and configuration

Credentials, tokens, and external-service secrets shall not be hard-coded or committed to the repository. Protected deployment traffic shall use an approved encrypted transport configuration.

**Target:** Repository and build scans report zero committed secrets; production-like deployment configuration uses HTTPS/TLS for protected traffic.

**Verification:** Secret scanner, configuration review, CI checks, and deployment smoke test.

## 4. Privacy and Data Protection Requirements

### NFR-007 – Role-limited sensitive data access

Sensitive email content, extracted indicators, analyst notes, and risk evidence shall be accessible only to authorized users who need them for their role.

**Target:** 100% of tested cross-role and cross-user access attempts are denied, while authorized analyst access remains available.

**Verification:** Authorization matrix tests and manual review of dashboard/API responses.

### NFR-008 – Reporter attribution privacy

The system shall distinguish verified reporter attribution from an email's displayed `From` header or forwarded-message metadata.

**Target:** A header address or untrusted forwarded value alone cannot make an attribution `Verified` or expose a report in an employee's history.

**Verification:** Ingestion tests using mismatched sender, forwarder, and authenticated account values.

### NFR-009 – Data minimization

External model or threat-intelligence services shall receive only the minimum data approved for that integration.

**Target:** Each external integration has a documented request schema and sends no unrelated employee identity, analyst note, or internal authorization data.

**Verification:** Mock-provider request capture, payload inspection, and code/configuration review.

### NFR-010 – Sensitive data in logs

Application logs shall avoid raw email bodies, credentials, access tokens, and unnecessary personal information.

**Target:** The defined sensitive-data log test suite finds zero prohibited raw-content or secret values in application logs.

**Verification:** Log capture during representative submissions, failure cases, authentication, and analyst review operations.

### NFR-011 – Retention and deletion policy

The project shall document the retention behavior for submitted messages, extracted evidence, case history, and operational logs.

**Target:** Every persisted sensitive-data category has an identified owner and a documented retention-policy entry. Until the team confirms the final durations, the MVP policy document shall explicitly mark the retention period as pending and define the interim handling/deletion decision.

**Verification:** Documentation review and database/schema inspection. The current MVP verification checks that the retention-policy document exists, covers submitted messages, extracted evidence, CaseHistory, and operational logs, and clearly marks unconfirmed periods. The time periods will be added to this requirement after team confirmation.

## 5. Performance Requirements

### NFR-012 – Manual report creation latency

Under the controlled MVP workload, the manual report-creation API shall respond within the agreed latency target, excluding asynchronous risk assessment work.

**Target:** At least 95% of report-creation requests complete within 3 seconds with 10 concurrent users and 1,000 stored reports.

**Verification:** Repeatable load test with documented hardware, dataset, request count, concurrency, and dependency conditions.

### NFR-013 – Analyst queue latency

The analyst dashboard list operation shall return a usable page of Cases within the agreed latency target.

**Target:** At least 95% of dashboard list API requests complete within 2 seconds with 10 concurrent users and 1,000 stored reports.

**Verification:** Repeatable API/load test using representative filters, pagination, and sorting.

### NFR-014 – Automated triage latency

The system shall expose a completed or partial risk-assessment result to the dashboard within the target when required dependencies are available.

**Target:** p95 assessment availability is at most 15 seconds under defined demonstration conditions; the report must state sample size and whether external services were mocked or live.

**Verification:** End-to-end timing test from accepted submission to assessment status/result availability.

### NFR-015 – Resource behavior

The system shall not consume unbounded memory or storage when processing malformed, large, or repeated messages.

**Target:** Defined maximum upload/message limits are enforced, and the worker process remains below 512 MB resident memory during the agreed stress test. The stress test must complete without process termination, disk exhaustion, or an exceeded memory limit.

**Verification:** Resource-monitored tests using boundary-size messages, repeated delivery, malformed MIME structures, and large attachment metadata. Record peak resident memory, disk usage, test duration, concurrency, and input sizes; pass only when peak worker memory is below 512 MB and the other defined limits are respected.

## 6. Reliability and Fault Handling Requirements

### NFR-016 – Accepted-report durability

Once a Report is acknowledged as accepted, its essential metadata and linked Case shall be persisted durably.

**Target:** 100% of accepted reports in the durability test remain recoverable after a controlled application restart.

**Verification:** Integration test that submits reports, restarts the application, and verifies Report–Case recovery.

### NFR-017 – Failure isolation

Failure of parsing, model inference, or threat-intelligence lookup shall not delete an accepted Report or prevent human review.

**Target:** Every defined downstream failure mode produces a visible `Failed` or partial-analysis state and preserves the Case.

**Verification:** Fault-injection tests for parser, model, timeout, malformed provider response, and database retry scenarios.

### NFR-018 – Idempotent mailbox processing

Repeated delivery of the same provider mailbox message shall produce one processing result.

**Target:** 100 repeated-delivery test cases produce no duplicate Report–Case pair for the same idempotency key.

**Verification:** Concurrent and sequential duplicate-ingestion integration tests.

### NFR-019 – Error response consistency

The API shall return documented, structured error responses without exposing stack traces, secrets, or internal infrastructure details.

**Target:** All documented client and server error classes conform to the API error schema in automated contract tests.

**Verification:** API contract tests and manual inspection of representative error responses.

## 7. Usability and Accessibility Requirements

### NFR-020 – Clear employee submission flow

An employee shall be able to understand how to submit a suspicious email and whether the submission was accepted.

**Target:** At least 80% of 5 representative test participants (at least 4 of 5) complete the submission task without facilitator intervention, and all validation errors identify the corrective action.

**Verification:** Task-based usability test with 5 documented participants, a task script, completion count, facilitator-intervention count, and issue log.

### NFR-021 – Clear analyst review flow

The analyst dashboard shall present Case status, risk recommendation, evidence availability, and final-decision controls distinctly.

**Target:** In usability testing, at least 80% of 5 representative analyst participants (at least 4 of 5) correctly distinguish automated risk from final classification and complete the review workflow.

**Verification:** Task-based usability test with 5 documented analyst participants and review of completion counts, incorrect-task outcomes, and facilitator interventions.

### NFR-022 – Accessibility baseline

The web interface shall follow WCAG 2.1 AA practices applicable to the MVP interface.

**Target:** No critical automated accessibility violations; keyboard navigation, form labels, focus visibility, semantic headings, and sufficient color contrast are verified for all primary flows.

**Verification:** Automated accessibility scan plus manual keyboard and screen-reader-oriented checks for report submission, queue, and Case detail views.

### NFR-023 – Understandable status and errors

The interface shall use consistent labels for `New`, `Under Review`, `Resolved`, `Pending`, `Completed`, and `Failed` states.

**Target:** All primary pages use the canonical status names and explain actionable errors in plain language.

**Verification:** UI inspection, content review, and acceptance tests for each status and error state.

## 8. Maintainability and Testability Requirements

### NFR-024 – Modular separation

The implementation shall keep report intake, parsing, risk assessment, authorization, and Case management separable through documented interfaces.

**Target:** Each listed module has a clear responsibility and can be tested with at least one isolated test double where an external dependency exists.

**Verification:** Architecture/code review and unit-test inspection.

### NFR-025 – API and data contract documentation

The API-facing behavior, enumerations, error responses, and key data relationships shall be documented consistently with the OpenAPI draft and SRS.

**Target:** No conflicting definitions exist for roles, risk categories, Case statuses, final classifications, or Report–Case cardinality across the project documents.

**Verification:** Documentation consistency review and OpenAPI validation.

### NFR-026 – Automated test coverage of critical paths

The project shall maintain automated tests for authentication/authorization, parsing failures, scoring boundaries, idempotency, and Case workflows.

**Target:** Every critical path has at least one passing automated test, and all tests run reproducibly using documented commands.

**Verification:** CI/test run and test-to-requirement traceability review.

### NFR-027 – Static analysis and code quality

The project shall run the agreed formatter, linter, and static-analysis checks before merging changes.

**Target:** The default branch has no newly introduced high-severity SonarQube findings attributable to the change under review. The agreed formatter, linter, and SonarQube analysis must run before merging changes.

**Verification:** SonarQube analysis report, CI output, and Pull Request review checklist. The report must identify the analyzed commit and the resulting findings by severity.

### NFR-028 – Configuration and dependency transparency

Runtime configuration and external dependencies shall be documented with safe defaults and version information.

**Target:** A new team member can start the application and run the critical test suite using the documented setup steps, without access to private credentials.

**Verification:** Clean-environment setup rehearsal and README/developer-documentation review.

## 9. Interoperability Requirements

### NFR-029 – Common intake representation

Web submissions and mailbox-ingested reports shall produce a common internal Report representation.

**Target:** Both intake channels populate the required Report fields and enter the same Case creation and risk-assessment workflow.

**Verification:** End-to-end tests comparing equivalent web and mailbox fixtures.

### NFR-030 – External-service substitution

Optional model and threat-intelligence providers shall be replaceable by deterministic test doubles or local fixtures.

**Target:** Critical tests and demonstrations can run without live third-party credentials or network access.

**Verification:** Offline test run using mocks/fixtures and dependency-injection review.

## 10. Traceability and Verification Summary

| Requirement group | IDs | Primary verification evidence |
| --- | --- | --- |
| Security | NFR-001–NFR-006 | Authentication, authorization, input-validation, secret-scanning, and adversarial-content tests |
| Privacy | NFR-007–NFR-011 | Access matrix, payload/log inspection, attribution tests, and policy review |
| Performance | NFR-012–NFR-015 | Repeatable load, latency, and resource-boundary tests |
| Reliability | NFR-016–NFR-019 | Restart, fault-injection, duplicate-delivery, and contract tests |
| Usability/accessibility | NFR-020–NFR-023 | Task-based usability tests, automated scans, and manual keyboard checks |
| Maintainability/testability | NFR-024–NFR-028 | Architecture review, CI, static analysis, setup rehearsal, and test inspection |
| Interoperability | NFR-029–NFR-030 | Equivalent-channel integration tests and offline fixture execution |

### 10.1 Mapping to System Vision quality requirements

The System Vision document uses the shorter identifiers `NFR-01` through `NFR-10`. The following mapping connects those baseline requirements to the detailed requirements in this document.

| System Vision requirement | Detailed requirements | Coverage |
| --- | --- | --- |
| NFR-01 Security | NFR-001–NFR-006 | Authentication, authorization, validation, untrusted-content handling, and secure configuration |
| NFR-02 Privacy | NFR-007–NFR-011 | Role-limited access, attribution privacy, data minimization, logging, and retention policy |
| NFR-03 Reliability | NFR-016–NFR-019 | Durability, failure isolation, idempotency, and consistent errors |
| NFR-04 Usability | NFR-020–NFR-021, NFR-023 | Employee submission, analyst review, status labels, and actionable errors |
| NFR-05 Maintainability | NFR-024–NFR-028 | Modular separation, contracts, tests, SonarQube analysis, and configuration transparency |
| NFR-06 Interoperability | NFR-029–NFR-030 | Common intake representation and replaceable external integrations |
| NFR-07 Performance | NFR-012–NFR-015 | API latency, triage latency, and resource behavior |
| NFR-08 Traceability | NFR-017, NFR-019, NFR-026 | Case-history persistence, structured errors, and test-to-requirement traceability |
| NFR-09 Fault Handling | NFR-004, NFR-008, NFR-010, NFR-017–NFR-020 | Invalid input, unavailable evidence, safe logs, downstream failures, and error handling |
| NFR-10 Testability | NFR-004, NFR-018, NFR-024–NFR-026, NFR-030 | Input, idempotency, modularity, automated tests, and offline test doubles |

## 11. Assumptions and Open Decisions

- The exact deployment environment and hardware used for performance measurement must be recorded before the final benchmark.
- Authentication is specified by the current `docs/OPENAPI.yaml` contract as JWT bearer authentication. Implementation and tests shall align with that contract; if the API security scheme changes, `docs/OPENAPI.yaml` and this document must be updated together.
- The team must define maximum accepted message and attachment metadata sizes for the MVP.
- Retention duration and deletion/anonymization behavior require team and course-policy confirmation.
- The QA and DevOps team members should review the verification methods and confirm the CI/static-analysis commands before this document is marked final.

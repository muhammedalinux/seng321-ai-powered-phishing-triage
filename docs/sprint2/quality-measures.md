# Sprint 2 — Quality Measures and Verification Plan

**Project:** AI-Assisted Phishing Triage System  
**Course:** BM321 / SENG 321 — Software Engineering  
**Date:** 2026-11-24  
**Responsible role:** QA / Tester (Sema)  
**Status:** Proposed verification baseline; pending team review and PR approval. No execution results are claimed.  
**Related issues:** [#20](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/issues/20), #17, #19, #27, #34.

## 1. Purpose and sources

This document consolidates measurable quality criteria for the planned MVP. It complements, rather than replaces, [NFR](NFR.md), [SRS](SRS.md), [QA test plan](QA_TEST_PLAN.md), [Product Backlog](product-backlog.md), and [RTM](traceability-matrix.md). Current SRS identifiers are FR-001–FR-022 and current NFR identifiers are NFR-001–NFR-030. Existing QM and TC identifiers retain their original meaning. NFR targets govern where the earlier QA plan is incomplete; disagreements require an explicit team decision.

## 2. Existing quality metrics

All results are **NOT RUN / not evidenced by this review**. QA coordinates verification; the roles below are proposed technical collaborators, not new issue assignments.

| Metric | Attribute | Acceptance target | Requirements / source | Verification and evidence | Collaborator |
|---|---|---|---|---|---|
| QM-01 | Testability | ≥50% unit-test line coverage at MVP, plus passing critical-path tests | NFR-026; QA plan | CI coverage report, commit SHA, test-to-requirement mapping | DevOps |
| QM-02 | Security | 100% protected endpoints pass authentication and role checks | FR-001, FR-002; NFR-001, NFR-003 | Endpoint × role matrix; missing, invalid, expired tokens and direct unauthorized requests | Architect / Implementation |
| QM-03 | Performance | p95 report creation ≤3 s | FR-003, FR-012; NFR-012 | 10 concurrent users, 1,000 stored reports; latency report excluding asynchronous assessment | Implementation |
| QM-04 | Performance | p95 queue response ≤2 s | FR-013; NFR-013 | Same workload; representative filters, sorting and pagination | Implementation |
| QM-05 | Performance | p95 assessment availability ≤15 s | FR-009, FR-010, FR-020; NFR-014 | Time from acceptance to usable completed/partial result; record terminal failures separately; reconcile QA timing definition before execution | Architect |
| QM-06 | Reliability | 0 accepted reports lost after controlled restart | FR-012; NFR-016 | Restart and verify Report–Case pairs | DevOps |
| QM-07 | Reliability | 0 duplicate pairs across 100 repeated-delivery test cases | FR-005; NFR-018 | Sequential and concurrent replay of identical mailbox keys | Implementation |
| QM-08 | Robustness | 0 crashes on malformed email fixtures | FR-006, FR-020; NFR-004, NFR-017 | Malformed MIME and fault injection; verify accepted cases remain visible | Architect |
| QM-09 | Privacy/security | 0 prohibited secrets or raw sensitive content in relevant logs/repository scans | NFR-006, NFR-010 | Secret scan and log capture on representative workflows | DevOps |
| QM-10 | AI effectiveness | Recall ≥90% | FR-009; QA plan / proposal §8 | Labeled evaluation set and confusion matrix | Architect |
| QM-11 | AI effectiveness | Precision ≥85% | FR-009; QA plan / proposal §8 | Same evaluation protocol | Architect |
| QM-12 | AI effectiveness | False positive rate ≤10% | FR-009; QA plan / proposal §8 | Same evaluation protocol | Architect |
| QM-13 | Usability | ≥4 of 5 employees submit without assistance; all validation errors actionable | FR-003; NFR-020 | Scripted usability sessions and error review | UI/UX |
| QM-14 | Accessibility | 0 critical automated violations plus manual WCAG 2.1 AA baseline checks | NFR-022 | Scan primary flows; keyboard, focus, labels, headings and contrast checks | UI/UX |
| QM-15 | Maintainability | 0 new high-severity SonarQube findings | NFR-027 | SonarQube report tied to commit; formatter/linter/CI evidence | DevOps |

## 3. Additional NFR verification coverage

These are direct NFR checks, not newly assigned QM identifiers or replacement thresholds. All are planned.

| Requirement | Acceptance criterion | Verification / evidence | Collaborator |
|---|---|---|---|
| NFR-002 | If passwords stored, no plaintext or reversible passwords | DB, code, fixture and log inspection; hashing review | Implementation |
| NFR-004 | All defined malformed/oversized/schema-invalid inputs rejected or isolated without unhandled errors | Unit/integration fixtures; TC-08–11, TC-17 | Implementation |
| NFR-005 | Zero privilege changes, unauthorized tool actions, remediation or case transitions from malicious content | Adversarial fixtures; TC-19, TC-33; boundary review | Architect |
| NFR-006 | HTTPS/TLS for protected deployment traffic, zero committed secrets | Configuration review, smoke test, scan | DevOps |
| NFR-007 | All defined unauthorized cross-role/user access denied; authorized analyst access retained | Negative and positive access matrix | Implementation |
| NFR-008 | Untrusted header alone never verifies attribution or exposes employee history | Mismatched sender/forwarder/account fixtures; TC-16 | Implementation |
| NFR-009 | Approved minimal provider payloads; no unrelated identity, notes or authorization data | Mock-provider payload capture and schema inspection | Architect |
| NFR-011 | Each sensitive persisted category has owner and policy entry; pending periods and interim handling explicit | Retention policy and schema review | Architect / Product Owner |
| NFR-015 | Defined limits enforced; peak worker RSS <512 MB, no termination or disk exhaustion | Stress test; record input sizes, concurrency, duration, RSS and disk use | DevOps |
| NFR-017 | Every defined downstream failure visibly recorded; accepted Case preserved | Parser/provider/timeout/retry fault injection | Architect |
| NFR-019 | All documented errors conform to schema; no secrets or stack traces | Contract tests and response review | Implementation |
| NFR-021 | ≥4 of 5 representative analysts distinguish risk from verdict and complete review | Scripted analyst usability test | UI/UX |
| NFR-023 | Canonical statuses and actionable errors throughout primary pages | UI review and status/error acceptance scenarios | UI/UX |
| NFR-024 | Clear module responsibilities; isolated test double where external dependency exists | Design/code review and unit-test inspection | Architect |
| NFR-025 | Zero conflicting canonical definitions across project documents | Cross-document comparison and OpenAPI validation | Architect |
| NFR-026 | Every critical path has at least one passing automated test; reproducible commands | CI and RTM review, in addition to QM-01 | DevOps |
| NFR-028 | Clean setup and critical suite run without private credentials | Setup rehearsal and documented commands | DevOps |
| NFR-029 | Both intake channels populate common fields and enter same workflow | Compare web/mailbox E2E fixtures | Implementation |
| NFR-030 | Critical tests and demonstrations run without live external credentials/network | Offline mock-provider suite | Architect |

## 4. Functional verification scenarios

Existing TC references point to [QA_TEST_PLAN.md](QA_TEST_PLAN.md). Additional checks below are proposals until added to that plan; none is an executed test.

| Workflow | Requirements | Existing tests / planned additions | Expected result |
|---|---|---|---|
| Authentication and roles | FR-001–002 | TC-01–06; verify 3,600 s token lifetime | Authorized access only; invalid/expired tokens rejected |
| Web intake and persistence | FR-003, FR-012 | TC-07–12 | Valid submission produces one Report and Case; >5 MB .eml rejected with 413 |
| Mailbox and provenance | FR-004–007 | TC-13–17, TC-22, TC-33 | Idempotent intake; safe parsing; unverified sender cannot claim history |
| Evidence and risk | FR-008–011, FR-020 | TC-18–23, TC-33 | Missing evidence explicit; score advisory; timeout at 30 s handled without deleting Case; retry rules respected |
| Queue and investigation | FR-013–017 | TC-24–29, TC-31–32 | Canonical ordering, valid state transitions, analyst-only final classification, attributable history |
| Employee status | FR-018 | TC-04, TC-16, TC-34 | Only own verified reports and permitted status data |
| Deferred administration | FR-019 | TC-06; proposed contract/scope inspection | No MVP public admin endpoints; admin role does not imply analyst privileges |
| Case assignment | FR-021 | TC-30; proposed valid/null assignment and history tests | Analyst assignment only; history recorded, risk/verdict unchanged |
| Metrics summary | FR-022 | TC-05, TC-37; proposed risk-bucket and total reconciliation | Analyst-only counts match queue, including Unscored |

## 5. Risk scoring and triage evaluation protocol

- Use a labeled synthetic/public email evaluation set with documented provenance, class distribution, dataset version and separation from tuning data. No real confidential employee mail.
- Preserve the QA plan convention: Medium or Critical is a positive prediction; Low is negative. Failed/unscored outcomes are reported separately with coverage and failure rate. Do not silently discard these outcomes or treat them as safe; report a conservative sensitivity analysis counting abstentions as errors where applicable.
- Recall = TP/(TP+FN); precision = TP/(TP+FP); false positive rate = FP/(FP+TN). Report raw counts and denominators, dataset size, configuration, model version and limitations. Undefined metrics are not passes.
- Check score bounds (0–100), canonical risk labels, evidence availability and reason traceability. A score is not a calibrated probability unless separately validated.
- Human verdicts remain independent of automated risk. No automated case closure, privilege change or mailbox remediation is permitted. Evaluate workflow correctness independently of predictive accuracy.

## 6. Test environment and evidence

Use controlled test data, documented hardware/software versions, commit SHA, dataset/fixture identifiers, dependency mode (mocked/live), request count, concurrency, and test command. Record raw timing and p95 calculation method; distinguish service failures from usable results. Proposed tools follow the QA plan: pytest, Playwright, repeatable load script, usability sessions and SonarQube. Store evidence with the corresponding PR or release; avoid secrets or real email bodies.

For every check record: metric/requirement ID, date, commit, environment, expected target, observed result, evidence link, status (PASS/FAIL/NOT RUN/BLOCKED), defect reference and reviewer. No observed values are available in this document.

## 7. Verification gates and unresolved decisions

- Sprint 2 documentation gate: traceable requirements, measurable criteria, consistent terminology, accessible evidence and reviewed PR. This is not a runtime quality certification.
- MVP gate: follow QA plan entry/exit criteria; all Must stories pass, no open Critical/Major defects, and QM-01–09 recorded. Track all remaining metrics and applicable NFR checks with explicit results or approved scope decisions. No approval is implied.
- Reconcile QA plan's QM-05 terminal-failure timing with NFR-014 usable assessment availability; 30 s failure timeout is distinct from the 15 s p95 goal.
- Confirm retention durations and interim policy, stress-test workload and resource limits, risk dataset/evaluation protocol, and course rubric/coverage interpretation with responsible owners.
- Update QA and RTM references to reflect FR-019 revised scope, FR-021 and FR-022. Resolve Must/Could conflict for metrics summary with Product Owner.
- QA and Architect must review this plan through PR before marking #20 complete.

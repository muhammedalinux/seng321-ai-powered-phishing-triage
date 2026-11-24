# Sprint 2 — Quality and Deliverables Review

**Project:** AI-Assisted Phishing Triage System  
**Course:** BM321 / SENG 321 — Software Engineering  
**Review date:** 2026-11-24  
**Responsible role:** QA / Tester (Sema)  
**Issue:** [#28](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/issues/28)  
**Overall status:** CONDITIONAL / final acceptance pending corrective actions and evidence review.

## 1. Scope and method

This is a documentation review of the repository's `develop` branch and available submission feature branch, against the four Sprint 2 deliverable groups and submission instructions supplied by the team. Repository files, issue metadata and cross-document consistency were inspected. No runtime tests, Figma interaction/access tests, full link crawl, official rubric audit, or actual team sign-off were performed. The separate official assessment rubric was not available for this review. Findings must be rechecked after integration.

PASS means the specified documentary check was supported; FAIL means a concrete omission or inconsistency; PENDING means evidence or review is outstanding. File presence alone is not final acceptance.

[Repository](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage) · [Sprint 2 directory](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/tree/develop/docs/sprint2) · [Milestone #1](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/milestone/1)

## 2. Required deliverable groups

| Group | Artifact / issue | Documentary check | Status | Finding / corrective owner |
|---|---|---|---|---|
| 1. SRS and backlog | [SRS](SRS.md), [#17](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/issues/17) | Detailed roles, workflows, numbered FRs and acceptance criteria present | PASS | 22 FRs found; cross-document alignment remains open (Architect) |
| 1 | [Product Backlog](product-backlog.md), #18 | Prioritized IDs, ownership, dependencies and estimates present | PASS | 29 items documented; metrics summary Could conflicts with unqualified FR-022; PO to resolve |
| 1 | SRS ↔ backlog ↔ RTM | Consistent baseline IDs and scope | FAIL | RTM/QA still use old 20-FR baseline; add FR-021/022 and revise FR-019 mapping (Nil, Sema, Burak, Ezgi) |
| 2. NFR and quality | [NFR](NFR.md), #19 | 30 numbered NFRs with targets and verification methods | PASS | Retention policy/durations remain a declared pending decision (Architect/PO) |
| 2 | [QA plan](QA_TEST_PLAN.md), #20/#34 | Metrics and test cases exist | PASS | 15 QM and 37 TC entries; stale testability notes and timing semantics require updates (QA) |
| 2 | [Quality measures](quality-measures.md), #20 | Separate required deliverable | PENDING | New document prepared for review; verify merged path and PR evidence (QA) |
| 2 | NFR ↔ QA ↔ SRS | Current requirement and metric definitions | FAIL | Update FR references and resolve QM-05 vs NFR-014 (QA/Architect) |
| 3. UI/UX and flows | [UI/UX](UI-UX.md), #21 | Figma link and four screen specifications documented | PASS | Link documented; actual permissions, interactive prototype and exported images not verified (UI/UX) |
| 3 | Figma prototype, #21 | Accessible clickable prototype consistent with requirements | PENDING | Open link using reviewer access and verify all primary screens (UI/UX/QA) |
| 3 | [User flows](user-flows.md), #22 | Three editable Mermaid flows present | PASS | Reporting, analyst review and auth flows present |
| 3 | UI/UX ↔ SRS | Canonical labels, role boundaries and safe workflow | FAIL | High/Suspicious labels, remediation controls and reporter-to-queue path conflict with baseline (UI/UX/Architect) |
| 4. Architecture | [C4](architecture/C4.md), #23 | Context/container models and trust boundaries present | PASS | Mermaid source present; final rendered diagram review pending (Architect) |
| 4 | [UML](architecture/UML.md), #24 | Structural/behavioral models and editable sources present | PASS | Reconcile latest FR-019/021/022 scope and coverage; render/review diagrams (Architect) |
| 4 | [Deployment](deployment-plan.md), #25 | Proposed topology and operational controls documented | PASS | Explicitly proposed infrastructure; ensure mailbox intake and API path alignment (DevOps/Architect) |
| 4 | Architecture consistency | C4/UML/deployment vs current SRS and API | PENDING | Full contract and diagram-level recheck required (Architect/DevOps) |

## 3. Supporting artifacts and submission evidence

| Artifact / issue | Check | Status | Action / owner |
|---|---|---|---|
| [Sprint backlog](sprint-backlog.md), #26 | Assigned work and milestone tracking | FAIL | Snapshot lists 13 issues, omitting #34; refresh against milestone (Nil) |
| [Progress report](progress-report.md), #26 | Actual status and current merge evidence | FAIL | Report says 13 issues and old PR/merge states; refresh snapshot and evidence (Nil) |
| [RTM](traceability-matrix.md), #27 | Every current FR/NFR represented | FAIL | 20 FR + 30 NFR baseline, while SRS now has 22 FR; update mappings and stale feature-branch references (Nil/QA/Architect) |
| #20 and #28 | Closed issue evidence | FAIL | Both were closed while named files/evidence were missing in develop; reopen or explicitly document corrective work and link PRs (QA) |
| #34 | QA plan evidence | PASS | QA plan present in develop; execution and review approval not independently verified |
| #29 | Submission document | PENDING | Submission file found on `issue29-submission-package` in prior inspection; verify merge and latest content (DevOps) |
| #29 | Release/tag | PENDING | Verify tag resolves to reviewed final commit; release link alone is not proof (DevOps) |
| All | Issue/PR links and contribution history | PENDING | Populate Evidence fields, verify approvals, merge status and contributors; preserve history (all owners/Nil) |
| Teams | Submission confirmation and official rubric | PENDING | Verify course rubric, final package accessibility, and Teams receipt (PO/SM) |

Known PR references documented in repository materials include #30 (SRS), #31 (NFR), #32 (C4), #33 (UML), #35 (QA plan), #36 (backlog), #37 (planning), and #38 (deployment). These are leads for evidence reconciliation, not independent confirmation of approvals or merge status. Use the [PR list](https://github.com/muhammedalinux/seng321-ai-powered-phishing-triage/pulls) and individual issue history.

## 4. Corrective action register

Severity describes documentation/release risk, not proven runtime defects. Owners are proposed responsible roles based on existing assignments.

| ID | Severity | Verified finding / concern | Required correction and recheck | Owner | State |
|---|---|---|---|---|---|
| REV-01 | Major | UI specifies High; SRS uses Critical | Replace High labels and filters with canonical Low/Medium/Critical; show Unscored where applicable | Çağan | Open |
| REV-02 | Major | Verdict UI/flows use Suspicious and include remediation controls | Use Phishing/Legitimate/Inconclusive, remove out-of-scope remediation; preserve analyst decision authority | Çağan / Burak | Open |
| REV-03 | Major | Prototype sends reporter to analyst Case List; flow waits for AI before confirmation | Separate employee confirmation/history and analyst queue; confirm accepted Report before async result; display failure/pending states | Çağan | Open |
| REV-04 | Major | RTM and QA plan reference old 20-FR SRS; FR-019 changed; FR-021/022 added | Map FR-019 to deferred admin scope, FR-021 to US-16 and TC-30 plus history/positive tests, FR-022 to US-21 and TC-05/37 plus bucket tests; review scope priority | Nil / Sema / Ezgi / Burak | Open |
| REV-05 | Major | QA QM-05 counts Completed or Failed; NFR-014 targets completed/partial result availability | Define usable-result latency, report failures separately, preserve 15 s p95 target and 30 s timeout distinction | Sema / Burak | Open |
| REV-06 | Minor | Earlier QA testability notes describe missing token lifetime, upload limit and timeout | Current SRS explicitly states 3,600 s, 5 MB and 30 s. Update QA notes after contract confirmation; recheck remaining notes rather than assume resolved | Sema / Burak | Open (SRS-side values documented) |
| REV-07 | Major | FR-022 mandatory wording conflicts with US-21 Could priority | PO/Architect agree whether metrics is MVP-required or explicitly deferred; align SRS/backlog/API/QA | Ezgi / Burak | Open |
| REV-08 | Major | Sprint backlog/progress omit #34; RTM includes outdated merge assumptions | Refresh issue count/status, links and current integrated baseline | Nil | Open |
| REV-09 | Major | #20/#28 named artifacts and PR evidence were missing despite closure | Merge reviewed documents, update Evidence fields and verify acceptance criteria | Sema | Pending PR |
| REV-10 | Major | Submission branch inspected earlier uses docs/SRS.md instead of docs/sprint2/SRS.md and assigns SRS/NFR work differently from actual issue assignments | Fix paths and responsibilities (#17/#19 Burak), include all final artifacts and verify tag/Teams evidence | Muhammed Ali / Nil | Pending recheck |
| REV-11 | Major | Retention policy periods not finalized; evidence of interim policy not verified | Document categories, owners and interim handling; confirm durations or explicit pending decision per NFR-011 | Burak / Ezgi | Pending |
| REV-12 | Major | External Figma access, diagram rendering and complete rubric compliance not verified | Review clickable design and rendered models; check official rubric and record actual reviewer evidence | Sema / Çağan / Burak | Pending |
| REV-13 | Major | User flows lack explicit forwarded intake, employee history and failure alternatives | Add or reference dedicated flows including unverified attribution and manual review after failed analysis | Çağan / Burak | Open |

## 5. Final recheck checklist

- [x] Inspect existence of core SRS, backlog, NFR, QA, UI/UX, user-flow, C4 and UML documents in develop.
- [x] Record observed consistency findings and proposed corrective owners.
- [ ] Verify revised quality-measures and review-checklist files in develop with reviewed PRs.
- [ ] Confirm latest SRS/QA/RTM IDs and scope alignment.
- [ ] Recheck corrected UI/UX flows and Figma permissions.
- [ ] Render and review architecture diagrams; validate contract alignment.
- [ ] Verify official rubric and complete all required deliverable checks.
- [ ] Reconcile issue statuses, PR evidence and individual contributions.
- [ ] Confirm release/tag points to reviewed commit and final links resolve.
- [ ] Record QA, Architect and PO review outcome; confirm Teams submission receipt.

## 6. Review decision

All four required deliverable groups have core documentation in the repository. This does not establish full consistency, final submission readiness, or successful runtime verification. Confirmed inconsistencies and pending external checks prevent an unconditional PASS. Corrective PRs and follow-up review are required. No approval, review meeting, test result or submission receipt is claimed.

**Review evidence:** PR URL and actual reviewers to be recorded after PR creation and review. Recheck findings against the final release commit; retain this dated snapshot as historical evidence.

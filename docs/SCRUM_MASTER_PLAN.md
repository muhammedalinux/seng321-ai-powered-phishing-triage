# Sprint 1 — Scrum Master Execution Plan

- **Project:** AI-Assisted Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026
- **Prepared by:** Nil (Scrum Master)
- **Version:** v1.2
- **Status:** Revised team working plan, aligned with System Vision & Boundary v1.1 (reference document). Instructor-specific requirements still require verification.
- **Language:** English (Turkish team-message templates in Part 5)

> **Source-of-truth rule:** The six Sprint 1 deliverables (per the QA report's announcement coverage) are: Project Proposal, System Vision & Boundary, OpenAPI Draft, Team Roles & RACI Matrix, GitHub Repository & Git-Flow, and Initial Product Backlog. The deadline, official submission mechanism and exact rubric details remain unverified here. **Do not invent them.** The current working ownership known from team work is **Ezgi → Project Proposal**, **Burak → System Vision & Boundary** and **Sema → Initial Product Backlog**; confirm final accountability in the team RACI. All other deliverable owners remain TBD.
>
> **Architecture baseline:** Burak's current **System Vision & Boundary** is the architectural reference for the Proposal and OpenAPI Draft. Its v1.1 text was used to align this plan; the OpenAPI Draft has not been checked yet.

## 0. Scrum Master's Responsibilities and Boundaries

Nil coordinates Sprint 1, maintains visibility of progress, facilitates lightweight planning and follow-ups, identifies blockers, and helps the team use an agreed GitHub workflow **if GitHub is part of the official or team-approved process**. Nil does **not** automatically own, technically approve, review, or merge every deliverable or Pull Request (PR).

| Role | Person | Primary contribution (not an automatic deliverable assignment) |
|---|---|---|
| Product Owner | Ezgi | Clarify product value, scope, and priorities |
| Scrum Master | Nil | Facilitate coordination, track progress, surface blockers |
| Software Architect | Burak | Advise on system boundaries, interfaces, and architecture |
| DevOps Engineer | Muhammed Ali | Support repository workflow, automation, and permissions where authorized |
| QA Engineer | Sema | Define and check quality criteria; review evidence where agreed |
| Implementation Engineer | Çağan | Contribute implementation feasibility and technical details |

**Already known working assignments:** Ezgi is preparing the Project Proposal; Burak is preparing System Vision & Boundary; Sema is preparing the Initial Product Backlog. These are working assignments, not a substitute for the agreed RACI. OpenAPI, RACI, and GitHub Repository & Git-Flow need confirmed owners.

**Decision boundaries:** The team confirms issue owners and reviewers using its agreed RACI matrix. QA checking is distinct from architectural approval; neither is automatically a Scrum Master responsibility. Repository administration, branch protection, and merge permissions depend on actual GitHub access and team decisions. The Product Owner clarifies scope, while the instructor's instructions override local process preferences.

## 1. Sprint 1 Planning — Chronological Execution

### Step 1 — Before Sprint Planning: Verify the source of truth

**Nil's actions**

- [ ] Obtain the instructor's official Sprint 1 announcement or assignment document.
- [ ] Read the instructor announcement and record the **exact acceptance requirements** of the GitHub Repository & Git-Flow and Initial Product Backlog deliverables before finalizing their Issues.
- [ ] Record the official deadline, submission location, required file formats, grading criteria, and any required approval or naming conventions, if specified.
- [ ] Confirm the known working owners (**Ezgi: Proposal; Burak: System Vision**) and have the team approve other owners and reviewers in the RACI.
- [ ] Distinguish **instructor-required** GitHub steps from **team-recommended** practices; check repository access, branch/PR permissions, and optional GitHub Projects availability.
- [ ] Create a short list of unresolved questions; do not convert assumptions into requirements.

**Next actor:** Ezgi helps clarify product scope; the relevant repository administrator confirms access; the team/instructor resolves requirement questions.

**Exit condition:** A shared, clearly marked list of confirmed requirements and outstanding questions exists.

### Step 2 — Sprint Planning: Agree on ownership and order

**Nil's actions**

- [ ] Hold one short planning discussion with the six team members.
- [ ] Walk through all six deliverables once.
- [ ] Confirm Ezgi as the working Proposal owner, Burak as the working System Vision owner and Sema as the Initial Product Backlog author; agree on one accountable owner and appropriate contributors/reviewers for each remaining deliverable in the RACI.
- [ ] Agree on a realistic internal review buffer before the official deadline (duration decided by the team).
- [ ] Record owner, reviewer, target date, and dependencies in each Issue after confirmation.
- [ ] Agree on lightweight progress updates and how to escalate blockers.

**Next actor:** Each confirmed owner accepts the Issue and clarifies open questions; designated reviewers agree on review scope.

**Exit condition:** Confirmed Issues have accepted owners, review expectations, and visible next steps. Unconfirmed fields remain marked **TBD**.

### Step 3 — Set up GitHub and begin work

**Nil's actions**

- [ ] If the team is using GitHub Issues, create Issues for all six deliverables using Part 2 (including the known working assignees).
- [ ] Adjust Issues 5 and 6 if the official announcement differs from the templates in Part 2.
- [ ] Add Issues to a Sprint 1 GitHub Project board **only if one is available and the team chooses to use it**; the repository Issues list alone is sufficient for tracking if permitted.
- [ ] Apply consistent labels and link related Issues.
- [ ] Ask owners to move accepted Issues to **To Do**, then **In Progress** when work begins.
- [ ] Confirm that each owner knows the repository location for the output; if no location is agreed, ask the team to decide rather than invent one.

**Next actor:** Issue owners prepare their deliverables and raise blockers promptly.

**Exit condition:** Each active task is visible, owned, and traceable in GitHub.

### Step 4 — During the Sprint: Monitor without taking over

**Nil's actions**

- [ ] Check the board at the team's agreed update cadence.
- [ ] Ask owners for concise status: completed, next, blocked.
- [ ] Identify dependency conflicts, missing decisions, and review bottlenecks.
- [ ] Help arrange decisions with the right role rather than making technical decisions on their behalf.
- [ ] If PRs are part of the agreed workflow, remind owners to open them early enough for review; otherwise request a traceable document review.
- [ ] Record material changes to scope or assignment in the relevant Issue.

**Next actor:** Owners complete work; reviewers provide actionable feedback; authorized maintainers handle protected-repository operations.

**Exit condition:** Blockers are assigned a next action and do not silently remain unresolved.

### Step 5 — Review and close

**Nil's actions**

- [ ] Verify that each deliverable has an accessible artifact and a linked PR or another team-approved traceable review record.
- [ ] Confirm the designated reviewers have completed their checks.
- [ ] Check that acceptance criteria are evidenced and any requested changes are addressed.
- [ ] Confirm approved changes are merged by an authorized person **if the team uses PRs**; otherwise verify the approved document version is stored at the agreed location.
- [ ] Check whether the official submission was completed by the designated submitter, according to the instructor's requirements.
- [ ] Confirm `CONTRIBUTIONS.md` is complete, the Sprint 1 release tag points to the approved commit, `mhmttemin` and `goksuyldrm` have accepted repository access, the ZIP contains all six deliverables, and the repository link is in the Teams description.
- [ ] Move qualifying Issues to **Done** and prepare a concise completion summary with remaining risks.

**Next actor:** Designated submitter handles course submission; the team confirms final status.

**Exit condition:** The team can point to evidence for each completed requirement; uncompleted work is explicitly documented, not silently marked Done.

### Dependency and completion overview

| Deliverable | Primary dependency | Suggested review focus | Done when |
|---|---|---|---|
| Project Proposal (working owner: Ezgi) | Confirmed project scope and instructor criteria | Product fit, clarity, feasibility, alignment with System Vision | Reviewed final version recorded; official submission tracked separately |
| System Vision & Boundary (working owner: Burak) | Agreed scope; proposal alignment | Actors, trust boundaries, Report–Case distinction, permissions, human authority | Reviewed version forms the baseline for Proposal/OpenAPI |
| OpenAPI Draft (YAML) | Initial system boundary and workflow assumptions | Endpoint consistency, schema validity, security and analyst decision flow | YAML is syntactically valid and matches agreed draft interfaces |
| Team Roles & RACI Matrix | Team role list and agreed ownership decisions | One accountable decision path per activity, no invented assignments | Matrix is agreed by the team and available in the repository |
| GitHub Repository & Git-Flow | Agreed branch/PR workflow; repository access | Repository structure, branch protection, traceable contributions, `CONTRIBUTIONS.md` | Rules are documented and enforced; each member's work is traceable |
| Initial Product Backlog (working owner: Sema) | System Vision & Boundary; agreed scope | Priorities, acceptance criteria, estimates, dependencies, owners | Reviewed by someone other than the author; backlog current in GitHub or Jira |

**Recommended working order (not a mandatory sequence):** Establish scope and responsibilities first; develop the proposal and vision iteratively; draft the OpenAPI contract once the boundary and major workflows are sufficiently clear. RACI can be prepared in parallel. Reconcile inconsistencies before final review.

## 2. GitHub Issues — Ready-to-Copy Templates

> **Assignment rule:** Working owners already identified: **Ezgi (Project Proposal)**, **Burak (System Vision & Boundary)** and **Sema (Initial Product Backlog)**. Nil should confirm these in the RACI. Other owners stay `TBD` until agreed. Review involvement below is suggested rather than automatically assigned. Issues, branches, PRs, and branch protection are needed for traceability (see QA report Q-08, Q-15, Q-20); labels and the project board are team workflow choices.

### Issue 1 — Project Proposal

**Title:** `[Sprint 1] Prepare Project Proposal`  
**Assignee:** Ezgi (working assignment; confirm in RACI)  
**Suggested labels:** `sprint-1`, `documentation`, `deliverable`, `needs-review`

**Description:** Prepare a concise project proposal for the AI-Assisted Phishing Triage System. Explain the problem of suspicious employee-reported emails, the intended IT/SOC audience, the proposed AI-assisted triage concept, and the role of human analysts. Follow the instructor's official proposal template and criteria when available.

**Tasks**
- [ ] Obtain and check the official proposal requirements.
- [ ] Describe the problem, intended users, and project objective.
- [ ] Summarize the proposed high-level solution and expected value.
- [ ] Clarify that AI scores are advisory and analysts make final decisions.
- [ ] Check consistency with the latest approved System Vision & Boundary: employee permissions, administrator role, report and case distinction, case lifecycle, trust boundaries, risk tier terminology, and AI decision limits.
- [ ] Request the agreed review, address comments, and finalize the artifact.

**Acceptance criteria**
- [ ] The proposal addresses all confirmed instructor requirements.
- [ ] Project purpose, audience, and scope are understandable.
- [ ] No claim implies automatic final phishing classification by AI.
- [ ] Reviewer feedback is resolved or explicitly documented.
- [ ] The final artifact is accessible and submitted if officially required.

**Dependencies:** Official proposal criteria; agreed scope; alignment with Issue 2.  
**Review involvement to confirm:** Product scope (Ezgi); technical feasibility (Burak/Çağan as agreed); quality checks (Sema as agreed).

### Issue 2 — System Vision & Boundary

**Title:** `[Sprint 1] Define System Vision and Boundary`  
**Assignee:** Burak (working assignment; confirm in RACI)  
**Suggested labels:** `sprint-1`, `documentation`, `scope`, `deliverable`, `needs-review`

**Description:** Define what the system aims to achieve, who interacts with it, and which responsibilities belong inside versus outside the proposed solution. Clearly distinguish AI-generated recommendations from analyst-controlled case decisions.

**Tasks**
- [ ] Confirm any official vision/boundary template or rubric.
- [ ] Describe primary users: reporting employees and authorized IT/SOC analysts.
- [ ] Outline report intake, email data extraction, risk scoring, dashboard display, analyst review, case management, and feedback; distinguish **Report**, **Case**, and **analysis job/status**.
- [ ] Define in-scope and out-of-scope functions without inventing unsupported channels or integrations.
- [ ] Describe trust boundaries and human oversight; include the administrator role if present in the current System Vision; distinguish **verified identity** from an email header address match.
- [ ] Specify canonical case states and risk tiers from the current System Vision, then align them with the Proposal and API draft (do not treat GitHub board states as case states).
- [ ] Obtain agreed reviews and resolve comments.

**Acceptance criteria**
- [ ] System purpose, actors, boundaries, and major flows are clear.
- [ ] Any unresolved implementation choices are marked as open questions; employee report visibility and identity attribution are unambiguous.
- [ ] AI scores are advisory; final case decisions remain with authorized analysts.
- [ ] The document is consistent with confirmed requirements and reviewed.

**Dependencies:** Scope agreement; instructor criteria; alignment with Issues 1 and 3.  
**Review involvement to confirm:** Ezgi for product scope, Burak for architectural boundary, Sema for completeness.

### Issue 3 — OpenAPI Draft (YAML)

**Title:** `[Sprint 1] Draft OpenAPI Specification (YAML)`  
**Assignee:** TBD — confirm via RACI  
**Suggested labels:** `sprint-1`, `api`, `architecture`, `deliverable`, `needs-review`

**Description:** Produce a preliminary OpenAPI YAML contract for the system's agreed API-facing workflows. It is a **draft contract**, not a claim that endpoints are implemented. Select paths and schemas collaboratively based on the confirmed system boundary; do not assume a particular framework, authentication provider, or submission channel.

**Tasks**
- [ ] Confirm the required OpenAPI version, file location, and instructor expectations, if specified.
- [ ] Agree on which workflows require API endpoints in Sprint 1.
- [ ] Draft relevant paths, operations, request/response schemas, and status responses.
- [ ] Use the canonical risk values from the System Vision (`Low / Medium / Critical`) in schema enums and examples; earlier working documents that used `High` have been aligned.
- [ ] Explicitly separate AI risk recommendations from analyst decisions, and distinguish **case status** from **background analysis status**.
- [ ] Document role-based access (employee, analyst, administrator as applicable) and distinguish verified employee submissions from unauthenticated forwarded email metadata.
- [ ] Validate YAML syntax and OpenAPI structure with an agreed validator.
- [ ] Obtain architecture and quality reviews; revise the draft.

**Acceptance criteria**
- [ ] The file is valid YAML and conforms to the agreed OpenAPI version.
- [ ] Documented interfaces match the confirmed System Vision, including Report–Case mapping, access permissions, canonical risk tiers, and case states.
- [ ] Schemas and response descriptions are internally consistent.
- [ ] Analyst-controlled decisions are not conflated with model-generated scores.
- [ ] Open questions and intentionally deferred endpoints are clearly marked.
- [ ] Review feedback is resolved or recorded.

**Dependencies:** Issue 2's initial boundary and workflows; instructor's API requirements.  
**Review involvement to confirm:** Burak for architectural consistency, Çağan for implementability, Sema for validation/quality.

### Issue 4 — Team Roles & RACI Matrix

**Title:** `[Sprint 1] Document Team Roles and RACI Matrix`  
**Assignee:** TBD — confirm via RACI/team agreement  
**Suggested labels:** `sprint-1`, `team-process`, `documentation`, `deliverable`, `needs-review`

**Description:** Record the six confirmed team roles and document agreed responsibility allocations for Sprint 1 activities. Do not infer deliverable ownership solely from job titles. In the matrix, **R** means Responsible (does the work), **A** Accountable (owns the outcome), **C** Consulted, and **I** Informed.

**Tasks**
- [ ] Record the six team members and roles accurately.
- [ ] Identify Sprint 1 activities/deliverables to include in the matrix.
- [ ] Ask the team to agree on R/A/C/I entries; leave undecided cells TBD.
- [ ] Check that each activity has a clear accountable decision path, where applicable.
- [ ] Clarify review, approval, submission, and repository administration responsibilities separately.
- [ ] Request team confirmation and publish the agreed matrix.

**Acceptance criteria**
- [ ] All six names and roles match the agreed team roster.
- [ ] No RACI assignments are presented as confirmed without team agreement.
- [ ] Responsibilities for coordination, QA review, architectural approval, and GitHub administration are distinguishable.
- [ ] The team has confirmed the final matrix and it is accessible.

**Dependencies:** Team roster; agreed responsibility decisions; any instructor-specific RACI format.  
**Review involvement to confirm:** All six team members for responsibility agreement.

### Issue 5 — GitHub Repository & Git-Flow

**Title:** `[Sprint 1] Set Up GitHub Repository and Git-Flow`  
**Assignee:** TBD — proposed: Muhammed Ali (confirm via RACI)  
**Suggested labels:** `sprint-1`, `devops`, `team-process`, `deliverable`, `needs-review`

**Description:** Set up an organized repository with `main`, `develop`, and feature branches, document the Git-Flow rules (branch, commit, Issue, Pull Request, and review conventions), and make every member's work traceable. Follow the course assistants' announcement; verify exact requirements against the original.

**Tasks**
- [ ] Create the repository (under a GitHub organization if the team confirms this requirement) with the agreed folder structure and a README linking the project documents.
- [ ] Create `main` and `develop`; add rulesets requiring Pull Requests and status checks on both.
- [ ] Document branch, commit, Issue, Pull Request, and review rules.
- [ ] Add the `LICENSE` file matching the Proposal.
- [ ] Add `CONTRIBUTIONS.md` with each member's section and links to their GitHub records.
- [ ] Give `mhmttemin` and `goksuyldrm` at least Read access and confirm the invitations are accepted.

**Acceptance criteria**
- [ ] `main` and `develop` exist and the enforced rules match the written Git-Flow policy.
- [ ] Every Pull Request is reviewed by someone other than its author.
- [ ] Each member's work is traceable through their own commits, Issues, and Pull Requests.
- [ ] `CONTRIBUTIONS.md` and `LICENSE` are in the repository.
- [ ] Both course assistants can read the repository.

**Dependencies:** Team roster; agreed workflow; OpenAPI validation job for the required status check.  
**Review involvement to confirm:** Nil for process fit; Çağan for technical setup; Sema for quality checks.

### Issue 6 — Initial Product Backlog

**Title:** `[Sprint 1] Prepare Initial Product Backlog`  
**Assignee:** Sema (working assignment; confirm in RACI)  
**Suggested labels:** `sprint-1`, `documentation`, `backlog`, `deliverable`, `needs-review`

**Description:** Prepare the prioritized product backlog (epics, features, user stories) with acceptance criteria, effort estimates, dependencies, and owners, aligned with the System Vision & Boundary. Keep the backlog current in GitHub Projects/Issues or Jira using the same item IDs.

**Tasks**
- [ ] Define epics, features, and user stories with MoSCoW priorities.
- [ ] Write testable acceptance criteria that include failure cases.
- [ ] Add estimates, dependencies, and proposed owners.
- [ ] Check consistency with System Vision & Boundary: Report–Case 1:1, status fields, risk tiers, human verdict.
- [ ] Synchronize the items with the GitHub backlog.
- [ ] Request review and address comments.

**Acceptance criteria**
- [ ] Priorities and Must items match the approved MVP.
- [ ] Every item has acceptance criteria, an estimate, dependencies, and one named owner.
- [ ] Terms, enums, and endpoints match the System Vision and OpenAPI Draft.
- [ ] The Product Owner has approved priorities; reviewer feedback is resolved.

**Dependencies:** Issue 2 (System Vision & Boundary); Issue 3 (OpenAPI Draft) for endpoint alignment.  
**Review involvement to confirm:** Ezgi for priorities; Burak or Çağan for technical accuracy (not the author).

## 3. Lightweight GitHub Workflow

These practices follow the course assistants' announcement as reported in the QA report (traceable commits, Issues, Pull Requests, peer review, and branch protection on `main` and `develop`). If the official instructions differ, follow those instructions. A document may be ready/reviewed in GitHub **without** having been officially submitted to the instructor.

1. **Find assigned work (when GitHub Issues are used).** Open the repository's **Issues** tab, filter by `assignee:@me` (or use the GitHub Project board), read the description and acceptance criteria, and ask questions in the Issue before starting.
2. **Create a branch when appropriate.** For a repository-based deliverable, update the default branch locally and create a focused branch, e.g., `docs/issue-12-project-proposal` or `api/issue-14-openapi-draft`. A branch is optional if the team's agreed workflow uses another traceable method.
3. **Prepare and commit.** Edit the agreed file(s), check formatting/validity, and make clear commits such as `docs: draft system vision and boundary`. Do not include secrets, real suspicious emails, or sensitive organizational data in the repository.
4. **Open a Pull Request (if the agreed workflow uses PRs).** Push the branch, open a PR to the agreed target branch, summarize changes, link the Issue (`Closes #12` only when merge should close it), and note any unresolved questions.
5. **Request review.** Request the designated reviewer(s) according to the RACI/team agreement. Architectural approval belongs to the agreed technical approver; QA checking belongs to the agreed quality reviewer. A Scrum Master may coordinate review but is not automatically an approver.
6. **Address feedback.** Respond to comments, update the branch, and re-request review if needed. Record decisions or scope changes in the Issue/PR.
7. **Merge approved changes (if PRs are used).** A team member with merge permission merges after the required reviews/checks pass under the repository's actual rules. Do not assume Nil is the merge owner.
8. **Close the Issue (if Issues are used).** Confirm the accepted artifact is merged or otherwise finalized, and that course submission is completed if required. If the PR used a closing keyword, GitHub may close the Issue automatically on merge; otherwise close it manually with an evidence link.

**Simple PR description template**

```md
## Summary
- 

## Related Issue
- Relates to #ISSUE_NUMBER

## Validation / Review Evidence
- [ ] Content checked against confirmed requirements
- [ ] Relevant formatting or YAML/OpenAPI validation completed
- [ ] Advisory AI scoring vs. analyst decision preserved (if applicable)

## Open Questions
- None / list here
```

**Role separation:** Nil coordinates visibility and reminders; Sema performs QA checks when designated; Burak gives architectural feedback/approval when designated; Muhammed Ali assists with repository setup or automation if authorized; Ezgi clarifies product priorities; the actual repository maintainers administer access and merging. No person is assigned an approval power merely because of their role title.


## 3A. Cross-Document Architecture Consistency Gate

Nil does **not** make these architectural decisions, but checks that the responsible reviewers have reconciled the documents before declaring them ready. **Burak's current System Vision & Boundary is the reference; request its latest version rather than relying on an earlier draft.**

| Topic | Required agreement across Proposal, System Vision and OpenAPI | Suggested checker |
|---|---|---|
| Actor and permissions | Employee, SOC analyst and (where defined) system administrator; employee visibility must follow verified attribution rules | Burak + Sema |
| Report versus Case | Each Report creates exactly one Case (1:1) per System Vision; cross-employee merging is out of MVP scope | Burak + Çağan |
| Lifecycle | Case states separate from analysis processing states and from GitHub task states | Burak + Çağan |
| Risk tiers | One enum across prose, JSON/YAML schemas, UI labels and tests; `Low / Medium / Critical` per System Vision | Burak + Ezgi |
| AI authority | Risk scores and explanatory signals are advisory; analyst owns final classification/closure | Ezgi + Burak + Sema |
| Forwarded-email identity | `From` address or directory match alone is not proof of authenticated employee identity; uncertain attribution remains unverified | Burak + Sema |
| API boundaries | `POST /reports` and any analyst/admin endpoints reflect actual allowed access; endpoints are drafts, not implemented features | Burak + Çağan |
| Security/privacy | Prompt-injection safeguards, safe parsing, PII handling and failures are not contradicted | Burak + Sema |

**Release gate:** A difference must be either resolved in all relevant documents or listed as an explicit open design decision; no undocumented contradictory claims in the final set.

## 3B. Separate Review, Repository Completion and Official Submission

Track these three milestones for each deliverable independently:

- [ ] **Content approved:** The designated reviewers check the deliverable against its acceptance criteria.
- [ ] **Repository finalized (if applicable):** The accepted version is saved/merged in the agreed location; record a path or PR link.
- [ ] **Course submission confirmed:** The designated submitter provides evidence the deliverable was submitted via the **official** mechanism, if required.

A GitHub Issue can be closed after agreed work is finished while course submission remains a separately tracked pending action. Do **not** claim a GitHub merge alone equals instructor submission.

## 4. Sprint Tracking — GitHub Issues / Optional Project Board

| Column | Move an Issue here when... | Expected action |
|---|---|---|
| **Backlog** | Work is identified but not yet accepted or scheduled; requirements/owner may be unresolved | Clarify scope and confirm ownership |
| **To Do** | The deliverable is confirmed, assigned, and ready to start | Owner plans the first step |
| **In Progress** | The owner has begun producing the deliverable | Owner updates progress and flags blockers |
| **In Review** | An artifact or PR is ready for agreed review | Reviewer checks criteria and provides feedback |
| **Done** | Agreed work and reviews are complete, with a link to the final artifact; any course submission is tracked separately | Nil verifies tracking accuracy; owner confirms closure |

If substantial revisions are requested, return the Issue from **In Review** to **In Progress**. A blocked Issue can remain in its current column with a `blocked` label and a comment naming the blocker, next action, and person responsible for resolving it. Avoid extra columns unless the team needs them.

**Suggested labels:** `sprint-1`, `deliverable`, `documentation`, `api`, `architecture`, `scope`, `team-process`, `needs-review`, `blocked`. Keep labels minimal and consistent.

**Important:** `Backlog / To Do / In Progress / In Review / Done` are **team task states**, not the product's case lifecycle (`New / Under Review / Resolved` per the System Vision). GitHub Projects is optional if it has not been required or adopted.

**Minimal Issue update format:** `Status: ... | Completed: ... | Next: ... | Blocker: none / ... | Help needed from: ...`.

## 5. Communication — Short Turkish Message Templates

The planning document is in English; the following ready-to-send messages are in Turkish for team coordination.

### Sprint kickoff

> Merhaba ekip, Sprint 1 çalışmalarına başlıyoruz. Öncelikle hocanın resmi duyurusundaki teslimleri ve gereklilikleri doğrulayacağız; ardından görev sahiplerini RACI'ye göre netleştirip GitHub Issues üzerinden ilerleyeceğiz. Lütfen planlama öncesinde kapsamla ilgili sorularınızı paylaşın.

### Task assignment (after confirmation)

> Merhaba [isim], Sprint 1 kapsamında **[Issue başlığı]** görevi ekip kararı/RACI doğrultusunda sana atandı. GitHub Issue: [link]. Lütfen kapsamı ve kabul kriterlerini kontrol edip görevi üstlendiğini teyit eder misin? Bir belirsizlik varsa Issue üzerinden belirtelim.

### Progress update request

> Merhaba ekip, kısa Sprint 1 durum güncellemesi rica ediyorum. Herkes kendi Issue'sunda tamamladıklarını, sıradaki adımı ve varsa engelleri paylaşabilir mi? Böylece review ve bağımlılıkları zamanında koordine edebiliriz.

### Blocker reporting

> Merhaba, **[Issue/link]** için bir engel tespit edildi: **[engel]**. Etkisi: **[etki]**. İhtiyaç duyulan karar/destek: **[ihtiyaç]**. Konuyla ilgili doğru kişiyi dahil edip sonraki adımı netleştirelim.

### Review reminder

> Merhaba [isim], **[PR/link]** inceleme için hazır. Uygun olduğunda, üzerinde anlaştığımız kabul kriterleri açısından değerlendirebilir misin? Özellikle **[kontrol noktası]** için geri bildirimin önemli. Teşekkürler.

### Sprint completion

> Merhaba ekip, Sprint 1 kapanış durumunu paylaşıyorum. Tamamlanan ve doğrulanan teslimler: **[liste]**. Devam eden/açık konular: **[liste veya yok]**. Resmi teslim durumu: **[doğrulandı/bekleniyor]**. Emeği geçen herkese teşekkürler; son bağlantıları ve kanıtları GitHub Issues üzerinde güncel tutalım.

## 6. Mandatory vs. Recommended — Decision Register

| Topic | Status | Next verification |
|---|---|---|
| Six Sprint 1 deliverables | Per QA report announcement coverage | Verify the full official list against the original announcement |
| Project Proposal | Named deliverable | Check instructor's exact rubric/format |
| System Vision & Boundary | Named deliverable | Check instructor's exact rubric/format |
| OpenAPI Draft (YAML) | Named deliverable | Check required version, scope, and validation expectations |
| Team Roles & RACI Matrix | Named deliverable | Check prescribed format and approval expectations |
| Deliverable owners/reviewers | **Working owners: Ezgi → Proposal; Burak → System Vision; Sema → Backlog. Others TBD** | Team confirms all accountability/review roles against RACI |
| Official deadline and submission mechanism | **Not provided** | Check instructor announcement |
| GitHub Issues, branches, PRs, Project board, labels | **Needed for traceability per QA report** (assistants review commits, Issues, PRs, branch protection, `CONTRIBUTIONS.md`); Project board optional | Verify against the original announcement |
| Review/merge permissions | **Not confirmed** | Check repository settings and team decisions |
| AI scores are advisory | **Project constraint** | Preserve in all relevant documents and interfaces |

## 7. Scrum Master Quick Checklist

- [ ] **Before work:** Obtain official announcement; confirm all six deliverables and the deadline.
- [ ] **At planning:** Confirm Ezgi/Proposal, Burak/System Vision and Sema/Backlog working ownership; assign remaining owners, reviewers, and dependencies in RACI.
- [ ] **After planning:** If GitHub Issues are used, create the six Issues; configure a board only if desired/required.
- [ ] **During work:** Monitor Issues, surface blockers, coordinate reviews without replacing owners.
- [ ] **Before closure:** Check review evidence, optional PR/merge status, and **separately** the official submission requirements.
- [ ] **At closure:** Report completed items, remaining gaps, and links to the final artifacts.

**Core rule:** Coordinate the process, make responsibility visible, and keep the team aligned—without assuming technical approval or repository-administration authority.

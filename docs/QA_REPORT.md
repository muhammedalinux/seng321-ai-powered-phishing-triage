# Sprint 1 Quality Assurance Report

- **Project:** AI-Assisted Phishing Triage System
- **Course:** SENG 321 – Software Engineering Project, Fall 2026
- **Prepared by:** Sema
- **Version:** v1.3
- **Status:** Revised preliminary review. Submission approval **not granted**: implementation, OpenAPI, repository and actual instructor requirements still need verification.
- **Evidence note:** Course announcements/rubric are cited by the original QA author but were not independently available for this revision. Treat instructor-specific rules as reported requirements until checked against originals.

## 1. Scope

**Reviewed:**

- The instructor's Sprint 1 announcement
- The course assistants' Sprint 1 announcement on deliverables, submission, and GitHub access
- The BM 321 Semester Team Project Specification and Assessment Rubric (course specification)
- Our Sprint 1 work plan
- Our updated task list, which we treat as the current version where it differs from the work plan. The course assistants' announcement and the Sprint 1 assignment files override both.
- SENG 321 Revised Assignment Sprint 1 Project Foundation (PDF)
- SENG 321 Revised Assignment Sprint 1 Examples (Markdown)
- `backlog(2).md` (original) and `INITIAL_PRODUCT_BACKLOG.md` (revised draft)
- `PROJECT_PROPOSAL.md` and `SYSTEM_VISION_BOUNDARY.md` (the System Vision & Boundary is the reference document)

**Not reviewed yet:**

- The OpenAPI YAML file
- The Team Roles and RACI document
- The GitHub repository, including its Issues, Pull Requests, and commits
- `CONTRIBUTIONS.md`

These findings include historical defects in earlier drafts as well as open checks against actual implementation and OpenAPI. Consistency between the documents can only be confirmed once they are complete, using the checklist in Section 6. In v1.2 the Proposal, Backlog, Scrum Master plan and this report were aligned with the System Vision & Boundary v1.1; OpenAPI consistency is still unverified.

## 2. Result

There are **29 findings: 20 high, 7 medium, and 2 low priority** (Q-01–Q-22 retained; Q-23–Q-29 additional findings). Findings may be addressed in the revised draft but are not closed until owner review and cross-document evidence are available.

We should resolve Q-01 to Q-05, Q-09, and Q-23 to Q-29 first, because these affect the System Boundary, OpenAPI, and Backlog together. Q-08 should be resolved early too, because it changes how every team member delivers their work.

## 3. Findings

### High priority

| ID | Finding | Why it matters | Recommendation | Owner |
|---|---|---|---|---|
| Q-01 | The work plan leaves open whether the AI component runs inside the system or as a separate service. | Feasibility, the boundary diagram, and the behavior of `POST /reports/{id}/analysis` all depend on this decision. | Record the decision in the System Boundary document, including the model or service, synchronous or asynchronous execution, and the data that is shared. | Burak, Çağan |
| Q-02 | The work plan and example API do not mention sign-in or role checks, although employees and analysts perform different operations. | Without authentication, the system cannot stop an employee from reading another person's report. | Add authentication to the MVP scope. Add a security scheme and `401` and `403` responses to the OpenAPI file. | Burak, Çağan |
| Q-03 | Action history is in the work plan's backlog examples, but not in its API examples. | Viewing history requires an endpoint, so the Backlog and OpenAPI documents could conflict. | US-18 records action history internally and US-19 shows it to analysts; both are Must because the System Vision includes report history in the MVP. Include `GET /reports/{id}/history` in OpenAPI. | Çağan, Sema |
| Q-04 | Terms are not fixed. The sources use both "report" and "case", and both "security analyst" and "IT/SOC teams". | The data model, user roles, and documents could diverge. | Choose one definition for each term and use it in every document. Use distinct Report (submission) and Case (investigation) records, linked one-to-one as defined in the System Vision & Boundary. | Burak, Çağan |
| Q-05 | Status values and risk levels are not defined. | `PATCH /cases/{caseId}` (proposed), tests and screens depend on these values. Case lifecycle and AI analysis status must not share one enum. | Define the values and allowed transitions the same way in System Boundary and OpenAPI. Section 10 of `INITIAL_PRODUCT_BACKLOG.md` contains the proposed split-status and risk vocabulary; confirm exact API serialization and transitions. | Burak, Çağan |
| Q-06 | The updated task list does not use named roles. The Sprint 1 assignment requires named owners for product, process, architecture, quality, DevOps, and implementation work, and its RACI example has these six roles. The course specification, however, keeps the implementation role for the seven-person team. | Every student must hold a named role, and role ownership must be visible in the RACI matrix and repository. | The team already uses the following named mapping; reflect it in the RACI and verify any rubric-specific exception with the assistants: Product Owner: Ezgi, Scrum Master: Nil, Architect: Burak, QA Engineer: Sema, DevOps Engineer: Muhammed Ali, and Implementation Engineer: Çağan. In the RACI example, the Product Owner is Accountable for acceptance criteria and the QA Engineer is Responsible. | Nil |
| Q-07 | The Sprint 1 assignment requires a stakeholder analysis of users, operators, decision makers, external providers, and affected groups, but the updated task list only names target users. | A list of users does not replace a stakeholder analysis. | Analyze employees, IT/SOC teams, company management, AI or email service providers, and other affected groups, with their interests and influence. | Ezgi |
| Q-08 | The updated task list says Muhammed Ali does not have to upload the deliverable files, and it names no one who does. The course assistants say the project documents must be kept in the repository. | The assistants will review each student's commits from their own GitHub account, commit dates, Issue assignees, branches, Pull Requests, reviews, and merges. If one person uploads everything, the other members' work will not be traceable. | Ensure meaningful personal contributions are attributable to each member using their own GitHub account, commits, issues, PRs and reviews as required by the actual course rules; shared deliverables may have multiple contributors, not necessarily one file per person. | Muhammed Ali, Nil |
| Q-09 | MVP scope has two owners. Ezgi prepares the initial scope, and Burak finalizes the MVP scope. | The Proposal, Boundary, and Backlog documents could describe different MVPs. The Must items in the backlog depend on this scope. | Ezgi as Product Owner owns product scope and priority decisions; Burak defines technical boundaries/feasibility. They jointly reconcile the Proposal, System Vision, OpenAPI and backlog; architecture does not automatically override product acceptance. | Ezgi, Burak |
| Q-10 | The updated task list does not clearly assign GitHub Issues, the Sprint 1 release tag, the ZIP file, or the Teams submission. It only says that Muhammed Ali prepares the GitHub evidence and repository URL. The work plan gave this work to Nil and Muhammed Ali, but its initial six Issues do not cover final QA, the release, or the submission. | The submission needs a complete ZIP file and a working repository link in the Teams description. One member submits for the group, so the work could be missed if no one owns it. | Name the submitting member and the owners of this work, and open Issues for final QA, the release, and the submission. | Nil, Muhammed Ali |
| Q-11 | The backlog currently exists as `INITIAL_PRODUCT_BACKLOG.md`, but the rubric's top level requires a prioritized backlog that is current in Jira or GitHub. | Requirements and backlog are worth 20 points in the rubric, and a static file is harder to keep current or trace to the product. | Add the backlog items to GitHub Projects, GitHub Issues, or Jira, and keep the item IDs from `INITIAL_PRODUCT_BACKLOG.md`. | Sema, Nil |
| Q-12 | No one is assigned `CONTRIBUTIONS.md`, which the course assistants require in the repository. | It must list each member's name, surname, student number, GitHub username, role, responsibilities, completed work, and related Issue, Pull Request, and commit links. | Name one coordinator. Each member adds their own section, with links to their GitHub records. | Nil |
| Q-13 | The sources assign no one to give the course assistants repository access. | The assistants must be able to read the repository. If it is private, sharing the link is not enough. | Add `mhmttemin` and `goksuyldrm` with at least Read access, and confirm that both invitations have been accepted. | Muhammed Ali |

| Q-23 | **Final analyst verdict was Should while case closure was Must.** The original US-14 could resolve a case before optional US-15 was delivered. | Human decision authority is a core product guarantee; an MVP cannot resolve without a human verdict. | In the revised backlog, US-15 is Must and resolution requires an authorized analyst verdict; align API transaction, tests and System Vision. **Status: draft corrected, evidence pending.** | Ezgi, Burak, Çağan, Sema |
| Q-24 | **Risk-based prioritization was Should**, although triage prioritization is the central product value. | An unsorted report list fails to deliver the core triage capability. | Revised US-11 makes a minimum risk-prioritized queue Must; US-12 is Must for explicit sorting/filtering, subject to PO scope and re-estimation. **Status: draft corrected, approval pending.** | Ezgi, Nil, Sema |
| Q-25 | **Baseline email-data protection was Should** unless external AI was used. | Sensitive mail may be mishandled even with a local model. | Make US-03 Must regardless of model hosting, including access controls, minimization, logging, storage/retention and safe rendering. **Status: draft corrected, validation pending.** | Burak, Çağan, Sema |
| Q-26 | **Forwarding ingestion and employee attribution** are inconsistent across the Proposal, the initial backlog and access rules. | `From` headers and directory address matches do not authenticate an employee, and forwarded reports can leak into "My reports". | Add controlled mailbox ingestion (US-22), store attribution status and restrict employee viewing to independently verified ownership. **Status: draft added, architecture/API confirmation pending.** | Burak, Muhammed Ali, Çağan |
| Q-27 | **The original backlog conflated Report with Case and analysis state with Case state; risk vocabulary also diverged.** | Duplicate reports, case resolution, status codes and OpenAPI schemas can contradict one another. | Use distinct Report/Case resources, separate analysis/case status fields, and `Low/Medium/Critical` advisory risk vocabulary; cardinality is 1:1 per System Vision; reconcile endpoints with OpenAPI. **Status: draft corrected against System Vision; OpenAPI check pending.** | Burak, Çağan, Sema |

| Q-28 | **Idempotent mailbox ingestion was optional (US-07 Could), while forwarding was mandatory.** | The System Vision explicitly requires idempotent processing of provider mailbox messages. | US-07 is now Must, depends on US-22, and must prevent a redelivered provider message from creating an additional Report or Case. Distinct employees' reports remain separate. **Status: draft corrected; implementation and review pending.** | Nil, Muhammed Ali, Çağan, Sema |
| Q-29 | **E2E tests did not depend on forwarding or idempotency work, and the Proposal treated fixture-only forwarding and asynchronous execution as settled.** | The stated MVP demonstration of both intake channels could be marked complete without testing them; EN-01 has not chosen sync vs. async. | EN-06 now depends on US-22 and US-07; Proposal states execution is undecided and requires a controlled test-mailbox flow, with fixtures only as supporting tests. **Status: draft corrected; demo, architecture decision, and OpenAPI review pending.** | Burak, Ezgi, Sema, Muhammed Ali |

### Documentation resolution vs. verification

Q-04 and Q-27 have their Report–Case and split-status **definitions resolved in the System Vision and revised backlog**, but remain open for OpenAPI/schema verification. Q-05 has canonical statuses and risk labels defined in System Vision; its exact API serialization and transition checks remain open. Q-01 is partly addressed by defining the boundary in System Vision, but model selection and sync/async execution remain open under EN-01. Q-23–Q-26 and Q-28–Q-29 have proposed document fixes; no finding is marked fully closed until the designated owner and independent reviewer record evidence.

### Medium priority

| ID | Finding | Why it matters | Recommendation | Owner |
|---|---|---|---|---|
| Q-14 | Some required items are missing from the updated task list: out-of-scope features, assumptions, and the license rationale in the Proposal, constraints and assumptions in the Boundary document, and protection rules for `main` and `develop`. | These items are required even though the shorter list does not name them. | Use the Sprint 1 assignment files and announcements as the checklist for each deliverable, not the shorter task list. | Ezgi, Burak, Muhammed Ali |
| Q-15 | The sources do not name a file for the RACI document or the Git-Flow rules, and they do not define a folder structure or Issue conventions. The team also plans one repository, while the course specification and Sprint 1 assignment both say "GitHub organization". | The written workflow must match the actual repository, and the documents must be organized in both the repository and the ZIP file. | Create the repository under a GitHub organization. Agree on the folder structure, file names, branch names, and Issue format, and link the documents from the README. | Nil, Muhammed Ali |
| Q-16 | Some deliverables have no named reviewer, and Sema is both the backlog author and the QA coordinator. Every Pull Request needs review by at least one teammate. | Authors would check their own work, which weakens the peer review evidence. | Name a reviewer for each deliverable. Burak or Çağan should review the backlog for technical accuracy, and someone other than Sema should review the QA report. | Nil |
| Q-17 | The risk items do not specifically cover personal data in reported emails. | Reported emails can contain personal data, and an external AI service would increase this risk. | Add this risk to the Proposal, and assess the requirements of Turkey's data protection law (KVKK, Law No. 6698). | Ezgi, Burak |
| Q-18 | The course specification sets Sprint 1 for the end of Week 2, but the team has no internal deadlines. | Proposal, Boundary, OpenAPI, and final QA depend on each other, so one delay reduces the time for final QA. | Set dates backward from the submission deadline, and keep a few days for final QA. | Nil |
| Q-19 | No one is assigned the process records listed under the Scrum Master role: stand-up and retrospective records, impediment tracking, and board administration. | The rubric's individual contribution criterion also looks at reviews, meetings, and evidence requirements. | Assign this work when Q-06 is closed, and start the records now. | Nil |
| Q-20 | The Sprint 1 assignment requires pull request and status-check rules on `main` and `develop`, but the Sprint 1 plan has no automated check. | Rulesets cannot require a status check that does not exist, and the written policy must match the enforced rules. | Add a GitHub Actions job that validates the OpenAPI file, and require it on both branches. | Muhammed Ali, Çağan |

### Low priority

| ID | Finding | Why it matters | Recommendation | Owner |
|---|---|---|---|---|
| Q-21 | License selection is assigned, but no task adds the `LICENSE` file or reviews dependency licenses. | The assignment requires the license in the repository and a justification in the Proposal. | Add the `LICENSE` file, justify the choice, and record dependency license checks as a follow-up. | Ezgi, Muhammed Ali |
| Q-22 | The sources do not name the tool for validating the OpenAPI file. | Without a named tool, it is harder to show evidence that the file is valid. | Choose a tool such as Redocly CLI or Spectral, and keep the result as evidence. | Çağan |

## 4. Backlog Review — After Proposed Revision

| Check | Result | Evidence / remaining work |
|---|---|---|
| Product work separated from Sprint 1 documentation | Pass (draft) | Sprint 1 deliverables tracked separately from product stories |
| IDs, feature grouping and estimates | Pass (draft) | Revised backlog contains 29 items / 101 SP: 27 Must (95 SP), 0 Should, 2 Could (6 SP) |
| Priorities match core triage behavior | Revised; PO sign-off pending | US-03, US-07, US-12 and US-15 promoted to Must; US-05, US-06, US-16, US-17 and US-19 promoted to Must because the System Vision includes them in the MVP; US-11 default priority queue |
| Forwarded email intake | Revised; feasibility pending | New US-22; mailbox/test-inbox adapter and ownership proof need approval |
| Report and Case are distinct | Aligned with System Vision v1.1; OpenAPI pending | Each Report creates exactly one Case (1:1) |
| Human verdict cannot be bypassed | Revised; OpenAPI/tests pending | US-15 Must; `Resolved` requires human verdict |
| Analysis and case lifecycle are separate | Proposed; OpenAPI pending | `analysis_status` separated from `case_status` |
| Risk vocabulary consistent | Aligned with System Vision v1.1; OpenAPI pending | `Low`, `Medium`, `Critical` used in Proposal, Backlog and this report |
| Dependency order and item arithmetic | Pass (draft) | Reordered ranked table with dependencies preceding dependent items; provisional estimates only |
| Per-owner delivery capacity | Pending | Workload still uneven; team to re-estimate and rebalance |
| Backlog available and up to date in GitHub/Jira | Pending | Synchronize item IDs and decide one authoritative system |
| Product Owner priorities approved | Pending | Ezgi sign-off required |
| All requirements matched to current Proposal/Boundary/OpenAPI | Partially verified | Proposal, Backlog, QA report and Scrum Master plan aligned with System Vision v1.1; OpenAPI still to be compared |

**Important:** A corrected Markdown draft is not evidence that implementation, branch rules, CI, or an OpenAPI contract has been validated.

## 5. Announcement Coverage

"Covered" means the work plan or updated task list includes the item. It does not mean the finished document meets the requirement.

### Required deliverables

| Deliverable | Required content | Result | Finding |
|---|---|---|---|
| Project Proposal | Problem statement and supporting evidence | Covered | |
| | Stakeholder identification and analysis | Partial | Q-07 |
| | Proposed solution and expected outcomes | Covered | |
| | Feasibility assessment and initial project scope | Covered | Q-01, Q-09 |
| | Assumptions, potential risks, and open-source license | Partial | Q-14, Q-17, Q-21 |
| System Vision and Boundary | Project purpose and target users | Covered | Q-04 |
| | System responsibilities and core functionalities | Covered | |
| | System context and external interactions | Partial | Q-01 |
| | In-scope and out-of-scope features | Covered | Q-09 |
| | Constraints and assumptions | Partial | Q-14 |
| OpenAPI Draft | Valid OpenAPI specification in YAML format | Partial | Q-22 |
| | API resources, paths, and HTTP operations | Partial | Q-02, Q-03 |
| | Request and response schemas | Covered | Q-05 |
| | Representative success and error responses | Covered | |
| | Initial API contract reflecting the proposed system | Partial | Q-01, Q-04 |
| Team Roles and RACI | Responsibilities for each team member | Covered | |
| | Product, process, architecture, quality, and DevOps | Covered | Q-06 |
| | Implementation | Covered | Q-06 |
| | RACI matrix | Covered | Q-16 |
| GitHub Repository and Git-Flow | Organized GitHub repository | Partial | Q-08, Q-15 |
| | `main`, `develop`, and feature branches | Covered | |
| | GitHub Issues and Pull Requests | Partial | Q-10 |
| | Peer review and branch protection rules | Partial | Q-14, Q-16 |
| | Traceable commit history and documented contributions | Partial | Q-08 |
| Initial Product Backlog | Prioritized epics, features, and user stories | Draft ready | Product Owner approval needed |
| | Acceptance criteria | Draft ready | |
| | Initial effort estimates | Draft ready | Team review needed |
| | Dependencies and assigned owners | Draft ready | Team review needed |

### Submission requirements

| Requirement | Result | Note |
|---|---|---|
| Team number, project name, and member information | Partial | The project name and member first names are known. The team number, full names, student numbers, and GitHub usernames are missing. |
| Responsible role owners | Pending | Q-06, Q-10 |
| ZIP file on Teams with all project documents | Pending | Q-10 |
| GitHub repository link in the Teams description | Pending | Q-10 |
| Repository access for both course assistants | Pending | Q-13 |
| `CONTRIBUTIONS.md` in the repository | Pending | Q-12 |
| Sprint 1 release tag | Pending | Q-10 |
| Links to Issues, Pull Requests, and commits | Pending | Q-12 |

## 6. Final QA Checklist

### General

- [ ] The documents follow the Revised Assignment Sprint 1 Project Foundation PDF.
- [ ] Proposal, vision, boundary, API, and backlog describe the same first release.
- [ ] There are no technology-first proposals, unlabeled diagrams, giant stories, or direct pushes to `main`.
- [ ] Another team can explain the release and build a mock client from the API contract. Uncertain answers are recorded as revision requests.
- [ ] The Sprint 1 Examples file has been reviewed, and our documents are written for our own project.
- [ ] The project name, user roles, and terms are the same in all documents.
- [ ] Findings Q-01 to Q-29 are closed with owner sign-off and evidence; draft-only corrections do not count as closure.

### Proposal

- [ ] The problem statement names the affected actor and context, and it gives evidence with sources.
- [ ] It has a measurable baseline where possible, and it does not prescribe an implementation.
- [ ] The stakeholder analysis covers users, operators, decision makers, external providers, and affected groups.
- [ ] The success criteria are measurable.
- [ ] Feasibility covers data, integration, skills, schedule, legal, ethical, and operational constraints.
- [ ] Out-of-scope features, assumptions, major risks, validation actions, the license, and the reason for choosing it are included.

### Proposal and System Boundary

- [ ] The initial scope in the Proposal matches the final MVP scope in the Boundary document.
- [ ] Features left for later releases are listed as out of scope.
- [ ] The target users in the Proposal match the user roles in the Boundary document.
- [ ] Assumptions and constraints do not conflict.
- [ ] The actors and external systems in the boundary diagram match the text.
- [ ] Each diagram relationship shows its direction, the information exchanged, and its purpose.
- [ ] The AI component decision matches.

### System Boundary and OpenAPI

- [ ] Each core responsibility maps to an API operation, or the document explains why none is needed.
- [ ] The OpenAPI file contains no out-of-scope features.
- [ ] The user roles match the security rules in the OpenAPI file.
- [ ] The data schemas preserve distinct Report and Case identities, their relationship, and forwarded-report attribution state.
- [ ] `case_status`, `analysis_status`, `verdict` and `risk_level` use distinct fields/enums; transitions and serializations match.

### OpenAPI

- [ ] The YAML file passes validation without errors.
- [ ] The file has metadata, and server information when it is known.
- [ ] Each operation has an `operationId`.
- [ ] Reusable schemas, responses, and security schemes are in `components`.
- [ ] Each operation has a success example and an error example.
- [ ] The primary journey has success, input-error, and conflict behavior.
- [ ] `400`, `401`, `403`, `404`, and `409` responses are defined where they apply.
- [ ] Protected operations have a security definition.
- [ ] The list operation has pagination parameters.

### OpenAPI and Backlog

- [ ] Every API endpoint proposed for the MVP in `INITIAL_PRODUCT_BACKLOG.md` appears in OpenAPI with an agreed path; internal mailbox workers are not required to be public endpoints.
- [ ] Every MVP operation in the OpenAPI file maps to at least one backlog item.
- [ ] The status codes in the acceptance criteria match the OpenAPI file.
- [ ] Each provisional API path and design assumption marked with `*` in `INITIAL_PRODUCT_BACKLOG.md` has been accepted or revised.

### Proposal, Boundary, and Backlog

- [ ] Must items match the approved MVP: safe reporting (including forwarding if approved), minimum privacy, priority queue, advisory AI, human verdict and auditable case resolution.
- [ ] Won't items match the out-of-scope features.
- [ ] Each success criterion in the Proposal can be traced to backlog items.
- [ ] The user roles in the backlog match the Boundary document.
- [ ] The Product Owner has approved the priorities and the revised 29-item / 101-point draft has been re-estimated by the team.
- [ ] The team has reviewed the estimates, owner assignments and implementation capacity.

### Team Roles and RACI

- [ ] All six team members hold a named role from the course specification.
- [ ] Each deliverable has exactly one Accountable person.
- [ ] Each deliverable has at least one Responsible person.
- [ ] Each deliverable names its Consulted and Informed members.
- [ ] Product, process, architecture, quality, DevOps, and implementation responsibilities are covered.
- [ ] The matrix matches the actual task owners.
- [ ] The backlog owners do not conflict with the roles.

### GitHub

- [ ] The `main` and `develop` branches exist.
- [ ] Rulesets on `main` and `develop` require pull requests and status checks.
- [ ] The enforced rules match the written Git-Flow policy.
- [ ] Branch, commit, Issue, Pull Request, and review rules are documented.
- [ ] Each Pull Request is reviewed by at least one teammate.
- [ ] Each Issue has a purpose, an assignee, and acceptance evidence.
- [ ] Every change links to an Issue, and each Pull Request includes test or review evidence.
- [ ] The backlog is current in GitHub or Jira, with the same item IDs as `INITIAL_PRODUCT_BACKLOG.md`.
- [ ] Every member's work is traceable through commits from their own GitHub account, Issues, and Pull Requests.
- [ ] Each Pull Request was reviewed by someone other than its author.
- [ ] The project documents are in the repository, and the README links to them.
- [ ] The `LICENSE` file matches the license in the Proposal.
- [ ] The repository is under a GitHub organization, if the team confirms this requirement.

### Submission

- [ ] Approved repository work is on the `main` branch.
- [ ] The Sprint 1 release tag points to the correct commit.
- [ ] `mhmttemin` and `goksuyldrm` have accepted their invitations and can read the repository.
- [ ] `CONTRIBUTIONS.md` has every member's name, surname, student number, GitHub username, role, responsibilities, work, and links.
- [ ] The repository link works, and it is in the Teams submission description.
- [ ] The links to Issues, Pull Requests, commits, and the release tag all work.
- [ ] The ZIP file contains all six deliverables, and it opens correctly.
- [ ] The team number, member information, and responsible role owners are complete.
- [ ] One member has submitted for the group on Teams.

## 7. Decision Log and Ownership (Revision v1.1)

| Decision / verification | Proposed owner | State | Evidence required |
|---|---|---|---|
| Final product MVP priorities and forwarding commitment | Ezgi | Pending | Product Owner approval in Issue/PR |
| Report–Case relationship and status transitions | Burak + Çağan | Pending | Current System Vision, ERD and OpenAPI diff |
| Privacy/authentication of forwarded reports | Burak + Muhammed Ali | Pending | Threat model, attribution cases and tests |
| Human verdict/resolution and triage priority | Çağan + Sema | Pending | OpenAPI operations and acceptance tests |
| Revised estimates and ownership | Whole team, coordinated by Nil | Pending | Planning record and GitHub backlog |
| Course mandatory GitHub/submission rules | Nil + Muhammed Ali | Not independently checked in this revision | Original instructor/assistant announcement |
| QA sign-off by a reviewer other than author | Assigned reviewer (TBD) | Pending | Linked review/approval evidence |

### QA finding closure rule

A finding is closed **only** after its owner supplies a corrected artifact or decision, a reviewer verifies it against the course requirement and current architecture, and a link to the approved PR/Issue or equivalent evidence is recorded. The revised backlog is a proposal, not a substitute for those checks.

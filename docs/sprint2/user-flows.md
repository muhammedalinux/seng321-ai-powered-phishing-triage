# User Flow Diagrams — AI-Powered Phishing Triage System

## 1. Overview
This document defines the primary end-to-end user journeys for the **AI-Powered Phishing Triage System**. The flows cover two core roles:
- **End User (Reporter):** Submits suspicious emails for automated triage.
- **Security Analyst:** Reviews triaged cases, inspects AI-generated risk explanations, and records final verdicts.

---

## 2. Flow 1: Suspicious Email Reporting (End User)

```mermaid
flowchart TD
    A[User Receives Suspicious Email] --> B[Open Report Email Screen]
    B --> C[Enter Sender, Subject, Body or Upload .eml]
    C --> D{Client-Side Validation Passed?}
    D -- No --> E[Show Inline Validation Errors]
    E --> C
    D -- Yes --> F[Submit to Backend API]
    F --> G[AI Triage Engine Analyzes Email]
    G --> H{Submission Successful?}
    H -- No --> I[Display Error Message & Retry Option]
    I --> C
    H -- Yes --> J[Generate Case ID & Initial Risk Score]
    J --> K[Show Confirmation Screen with Case Reference]
```

### Key Steps
1. The user navigates to the **Report Email** screen and inputs email metadata/content or uploads a raw `.eml` file.
2. Required fields (sender address, subject, content) are validated before submission.
3. Upon submission, the system creates a triage case, runs automated AI risk scoring, and returns a confirmation with a tracking ID.

---

## 3. Flow 2: Case Triage & Analyst Review (Security Analyst)

```mermaid
flowchart TD
    A[Analyst Logs In] --> B[Open Case List / Triage Queue]
    B --> C[Filter / Sort by Risk Score, Status, or Date]
    C --> D[Select a Case]
    D --> E[View Case Detail Screen]
    E --> F[Inspect Email Headers, Content & AI Risk Indicators]
    F --> G[Open Analyst Review Panel]
    G --> H[Select Verdict: Phishing / Suspicious / Legitimate]
    H --> I[Add Review Notes]
    I --> J{Confirm Verdict Submission?}
    J -- Cancel --> E
    J -- Confirm --> K[Update Case Status to Resolved]
    K --> L[Log Audit Trail & Return to Updated Case List]
```

### Key Steps
1. The analyst accesses the **Case List**, prioritized by AI risk level (`High`, `Medium`, `Low`).
2. Selecting a case opens **Case Details**, displaying original email artifacts alongside AI-extracted phishing indicators (suspicious URLs, domain mismatches, urgency cues).
3. The analyst assigns a final verdict, attaches justification notes, and submits the review to close the case.

---

## 4. Flow 3: Authentication & Role-Based Access Handling

```mermaid
flowchart TD
    A[User Accesses Protected Route] --> B{Authenticated?}
    B -- No --> C[Redirect to Login Screen]
    C --> D[Authenticate Credentials]
    D --> E{Valid Role?}
    E -- No --> F[Show 403 Access Denied]
    E -- Yes --> G[Redirect to Intended Screen]
    B -- Yes --> G
```
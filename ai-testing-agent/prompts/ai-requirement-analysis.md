# Prompt: AI-Powered Requirement Analysis & Test Plan Generation

> Copy this entire prompt and paste it into ChatGPT / Copilot.
> Then paste your requirement document, user story, or design specs below.

---

## PROMPT — START

```
ROLE
You are a Senior QA Analyst and Test Strategist with 15+ years of experience in requirement analysis, risk-based test planning, and test design.

OBJECTIVE
Analyze the provided requirement document, user story, or feature specification and produce:
1. A structured feature summary
2. A risk-based test plan
3. Suggested test case outlines (not full test cases — outlines for a tester to expand)
4. Ambiguities and questions for clarification
5. Recommended test types

INPUT I WILL PROVIDE
- Requirement document, SRS, user story, or feature description
- Application context (optional — what kind of app, domain, etc.)
- Known constraints or risks (optional)

OUTPUT FORMAT (STRICT)

### 1. Feature Summary

| Field           | Value |
|-----------------|-------|
| Feature Name    |       |
| Module          |       |
| User Roles      |       |
| Business Goal   |       |
| Key Flows       | Numbered list of main flows |
| Integrations    | External systems, APIs involved |

### 2. Risk Assessment

| Risk Area        | Risk Level | Justification | Testing Impact |
|------------------|------------|---------------|----------------|
| <area>           | High/Med/Low | Why this is risky | What to test more heavily |

### 3. Suggested Test Areas

| # | Test Area | Type | Priority | Flow Reference | Notes |
|---|-----------|------|----------|----------------|-------|
| 1 | Valid login with correct creds | Positive | P1 | FLOW-001 | Happy path |
| 2 | Login with expired password | Negative | P1 | FLOW-001-E1 | Security |
| 3 | Max-length username | Boundary | P3 | FLOW-001 | Data validation |

### 4. Ambiguities & Questions

| # | Question | Why It Matters | Suggested Default |
|---|----------|----------------|-------------------|
| 1 | What happens on 5th failed login? | Security — lockout policy unclear | Assume 30-min lockout |

### 5. Recommended Test Types

| Test Type | Applicable? | Reason |
|-----------|-------------|--------|
| Smoke | Yes/No | |
| Regression | Yes/No | |
| Security | Yes/No | |
| Performance | Yes/No | |
| Accessibility | Yes/No | |
| Visual | Yes/No | |

ANALYSIS RULES (STRICT)

1. Read the entire requirement before generating any output.
2. Identify ALL user roles mentioned or implied.
3. Map every flow (happy path AND exception paths) — assign Flow IDs (FLOW-<MODULE>-<SEQ>).
4. For risks: consider data sensitivity, complexity, integration points, user impact, and regulatory needs.
5. For test areas: cover positive, negative, boundary, security, and usability — at minimum.
6. For ambiguities: flag anything unclear, missing, or contradictory — do not assume silently.
7. Suggest which test types (smoke, regression, security, perf, accessibility, visual) are relevant and why.
8. Assign priorities following test-id-rules: P1 (critical), P2 (high), P3 (medium), P4 (low).

DO NOT:
- Generate full detailed test cases (use prompts/manual-testing.md for that)
- Explain QA methodology or testing theory
- Add introductions or conclusions
- Skip the ambiguities section — there are ALWAYS questions to surface
- Use vague language ("test various scenarios") — be specific
```

## PROMPT — END

---

## How to Use

1. Copy the prompt above.
2. Paste into your AI tool.
3. Below the prompt, paste your requirement:

```
APPLICATION CONTEXT: Hospital Management System (Web App)

REQUIREMENT:
The Patient Registration module allows new patients to register via a web form.
Fields: First Name, Last Name, Email (unique), Phone (10 digits), DOB (must be past date), Gender (dropdown).
On success: confirmation message + email sent.
On duplicate email: "Email already registered" error.
All fields are mandatory.
Admin users can also register patients on behalf of walk-ins.
```

4. Review the AI output:
   - Validate the feature summary and risk assessment.
   - Check that all flows and edge cases are captured.
   - Use the ambiguities list to clarify with your team.
   - Feed the "Suggested Test Areas" into `prompts/manual-testing.md` to generate full test cases.

---

## Workflow Integration

This prompt is **Step 1** in the AI-augmented testing workflow:

```
Requirement Doc
    ↓
[ai-requirement-analysis.md]  →  Feature Summary + Risk Assessment + Test Areas
    ↓
[manual-testing.md]  →  Full Detailed Test Cases
    ↓
[automation-playwright.md / automation-cypress.md]  →  Automation Scripts
    ↓
[execution-triage.md]  →  Failure Classification + Next Actions
    ↓
[defect-reporting.md]  →  Structured Bug Reports
```

---

## Tips for Better Results

- **More context = better analysis.** Include business rules, regulatory requirements, and user personas.
- **Mention the domain.** "Healthcare app" triggers different risk analysis than "e-commerce app" (HIPAA vs PCI-DSS).
- **Include non-functional requirements.** If performance or accessibility matter, state them explicitly.
- **Attach UI wireframes or screenshots** (describe them textually if you can't upload images) — they help AI identify additional test areas.
- **State known constraints.** "Must support IE11" or "Must work offline" significantly changes the test plan.

---

## Follow-Up Prompts

After getting the analysis, use these follow-ups:

### Generate full test cases from the analysis:
```
"Using the test areas above, generate full detailed test cases following
the format in templates/testcases-template.md. Use project prefix HMS
and module prefix REG. Start numbering at 001."
```

### Expand a specific risk area:
```
"Expand Risk Area #2 (Data Privacy) into detailed test scenarios.
Include GDPR compliance checks, data masking validation, and
consent flow testing."
```

### Generate Gherkin from test areas:
```
"Convert Test Areas 1-5 into Gherkin scenarios following
rules/gherkin-rules.md. Use Scenario Outline for data-driven cases."
```

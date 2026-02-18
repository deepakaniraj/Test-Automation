# Test Cases Template

> Use this template for all manual test cases. Each test case must fill every field below.
> Follow `rules/test-id-rules.md` for ID conventions and `rules/output-format-rules.md` for formatting.

---

## Test Case Entry

| Field                    | Value |
|--------------------------|-------|
| **Test Case ID**         | `<PROJECT>-<MODULE>-<SEQ>` (e.g., HMS-LOGIN-001) |
| **Requirement ID**       | Linked requirement or user story ID |
| **Module / Feature**     | Module or feature name |
| **Flow ID**              | Workflow identifier (e.g., FLOW-LOGIN-001) |
| **Title**                | Short, descriptive title of the test case |
| **Priority**             | P1 (Critical) / P2 (High) / P3 (Medium) / P4 (Low) |
| **Type**                 | Smoke / Regression / System / Integration |
| **Preconditions**        | What must be true before test execution |
| **Automation Status**    | Not Automated / Automated / Partial |
| **Automation Script Ref**| File path or link to automation script (if automated) |

### Test Steps

| Step # | Action | Test Data | Expected Result |
|--------|--------|-----------|-----------------|
| 1      |        |           |                 |
| 2      |        |           |                 |
| 3      |        |           |                 |

### Post-Conditions

- Describe the expected system state after test execution.

---

## Bulk Test Case Format (for multiple test cases in one document)

```markdown
### TC-001: <Title>

| Field           | Value          |
|-----------------|----------------|
| Test Case ID    | HMS-LOGIN-001  |
| Requirement ID  | REQ-AUTH-001   |
| Module          | Authentication |
| Priority        | P1             |
| Type            | Smoke          |
| Preconditions   | User exists in system, browser open |
| Automation      | Not Automated  |

**Steps:**

| # | Action                        | Test Data         | Expected Result                |
|---|-------------------------------|-------------------|--------------------------------|
| 1 | Navigate to login page        | URL: /login       | Login page is displayed        |
| 2 | Enter valid username          | user: john.doe    | Username field is populated    |
| 3 | Enter valid password          | pass: ********    | Password field is populated    |
| 4 | Click Login button            | —                 | User is redirected to dashboard|

**Post-Conditions:** User is logged in and session is active.

---
```

## Field Definitions

| Field               | Description |
|----------------------|-------------|
| Test Case ID         | Unique ID following `<PROJECT>-<MODULE>-<SEQ>` convention |
| Requirement ID       | Maps to a requirement, user story, or acceptance criterion |
| Module / Feature     | Functional area of the application |
| Flow ID              | Business workflow this test belongs to |
| Title                | Brief summary — should be understandable without reading steps |
| Priority             | P1 = must test every run, P4 = test when time permits |
| Type                 | Smoke = critical path, Regression = core features, System = end-to-end |
| Preconditions        | Environment, data, and state requirements |
| Automation Status    | Current automation state |
| Automation Script Ref| Path to the `.spec.ts` or `.cy.ts` file that automates this case |
| Test Steps           | Ordered actions with specific test data and verifiable expected results |
| Post-Conditions      | System state after successful test execution |

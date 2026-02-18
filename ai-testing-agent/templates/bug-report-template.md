# Bug Report Template

> Use this template for all defect reports. Every field must be filled.
> Follow `rules/test-id-rules.md` for ID conventions.

---

## Bug Report Entry

| Field                  | Value |
|------------------------|-------|
| **Bug ID**             | `BUG-<MODULE>-<SEQ>` (e.g., BUG-LOGIN-001) |
| **Title**              | `[<Module>] Short descriptive summary of the defect` |
| **Reported By**        | Tester name |
| **Reported Date**      | YYYY-MM-DD |
| **Environment**        | QA / UAT / Prod |
| **Browser / Device**   | Chrome 120 / Edge / Safari / Android 14 / iOS 17 |
| **OS**                 | Windows 11 / macOS / Linux / Android / iOS |
| **Build / Version**    | Application build number or version |
| **Severity**           | Critical / Major / Minor / Trivial |
| **Priority**           | P1 (Immediate) / P2 (High) / P3 (Medium) / P4 (Low) |
| **Status**             | New / Open / In Progress / Fixed / Retest / Closed / Reopened |
| **Linked Test Case ID**| Test Case ID(s) that discovered this bug |
| **Linked Requirement** | Requirement or user story ID |
| **Assigned To**        | Developer name (if known) |

---

### Preconditions

- Describe the setup, data, or state required to reproduce the bug.

---

### Steps to Reproduce

| Step # | Action | Test Data |
|--------|--------|-----------|
| 1      |        |           |
| 2      |        |           |
| 3      |        |           |

---

### Expected Result

- What should happen according to the requirements.

---

### Actual Result

- What actually happened (the defect behavior).

---

### Evidence

| Type        | Link / Path |
|-------------|-------------|
| Screenshot  |             |
| Video       |             |
| Error Log   |             |
| Console Log |             |
| Network Log |             |

---

### Root Cause / Notes

- Any additional analysis, observations, or suspected root cause.
- Note if reproducible consistently or intermittently.
- Mention any workaround if available.

---

### Retest Results (filled after fix)

| Field           | Value |
|-----------------|-------|
| Fix Build       |       |
| Retest Date     |       |
| Retest Status   | Pass / Fail |
| Retested By     |       |
| Notes           |       |

---

## Severity Definitions

| Severity  | Description |
|-----------|-------------|
| Critical  | Application crash, data loss, security breach, complete feature block |
| Major     | Major functionality broken, no workaround available |
| Minor     | Feature works but with issues, workaround exists |
| Trivial   | Cosmetic issue, typo, minor UI misalignment |

## Priority Definitions

| Priority | Description |
|----------|-------------|
| P1       | Fix immediately — blocks testing or release |
| P2       | Fix before next release |
| P3       | Fix when time permits |
| P4       | Nice to have — low impact |

# Daily Execution Update Template

> Use this template to report daily test execution status.
> Fill every section after each day's test run.

---

## Daily Execution Report

| Field              | Value |
|--------------------|-------|
| **Date**           | YYYY-MM-DD |
| **Environment**    | QA / UAT / Prod |
| **Build / Version**| Application build number or version |
| **Reported By**    | Tester name |
| **Sprint / Cycle** | Sprint or test cycle identifier |

---

### Execution Summary

| Metric              | Count |
|----------------------|-------|
| Total Test Cases     |       |
| Executed             |       |
| Passed               |       |
| Failed               |       |
| Skipped              |       |
| Blocked              |       |
| Not Executed         |       |
| **Pass %**           |       |

---

### Suites Executed

| Suite         | Total | Passed | Failed | Skipped | Pass % |
|---------------|-------|--------|--------|---------|--------|
| Smoke         |       |        |        |         |        |
| Regression    |       |        |        |         |        |
| System / E2E  |       |        |        |         |        |

---

### New Bugs Logged Today

| Bug ID           | Title                        | Severity | Linked TC |
|------------------|------------------------------|----------|-----------|
| BUG-LOGIN-005    | Login fails with valid SSO   | Major    | TC-LOGIN-012 |

---

### Bugs Retested Today

| Bug ID           | Title                        | Retest Status | Notes |
|------------------|------------------------------|---------------|-------|
| BUG-REG-003      | Registration date validation | Pass          | Fixed in build 2.3.1 |

---

### Risks / Blockers / Issues

| # | Description | Impact | Status |
|---|-------------|--------|--------|
| 1 |             |        |        |

---

### Plan for Next Day

- [ ] Execute remaining regression test cases for Module X
- [ ] Retest BUG-XXX-XXX after fix deployment
- [ ] Add new test cases for Feature Y
- [ ] Run smoke suite on UAT environment

---

### Notes / Comments

- Any additional observations, environment instability, data refresh needs, or stakeholder communication.

---

## Master Execution Tracker Row (append to master-execution.xlsx)

| Date | Total | Passed | Failed | Skipped | Blocked | Pass % | Environment | Build | Notes |
|------|-------|--------|--------|---------|---------|--------|-------------|-------|-------|
|      |       |        |        |         |         |        |             |       |       |

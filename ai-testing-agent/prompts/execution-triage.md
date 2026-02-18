# Prompt: Execution Triage

> Copy this entire prompt and paste it into ChatGPT / Copilot.
> Then paste your test runner output below the prompt.

---

## PROMPT — START

```
ROLE
You are a test execution triage assistant with deep experience analyzing test runner outputs from Cypress, Playwright, Jest, and other frameworks.

OBJECTIVE
Read the provided test runner output logs, summarize all failures, classify each failure's root cause, and suggest concrete next actions.

INPUT I WILL PROVIDE
- Test runner output (console log, JSON report, or summary text)
- Test ID → Module mapping (optional)
- Previous run results for flaky detection (optional)

TRIAGE RULES (STRICT)

1. Identify every failed test from the output.

2. For each failed test, extract:
   - Test Case ID or test name
   - Error message (short — max 1 sentence)
   - Relevant stack trace snippet (max 2 lines — not full dump)

3. Classify root cause into exactly ONE of these categories:

| Category        | When to Use |
|-----------------|-------------|
| bug             | Application returned wrong result, unexpected error, HTTP 4xx/5xx, broken business logic |
| locator issue   | Element not found, selector changed, stale element, timeout waiting for element |
| data issue      | Test data missing, expired, or incorrect — database state, fixture problem |
| flaky/timing    | Intermittent failure, race condition, animation delay, network timing |
| environment     | Server down, URL unreachable, certificate error, deployment issue, config mismatch |

4. Suggest a concrete next action for each failure:

| Category        | Suggested Action |
|-----------------|------------------|
| bug             | Raise defect using defect-reporting prompt |
| locator issue   | Update selector in page object; verify UI changes |
| data issue      | Refresh test data; check fixture; verify DB state |
| flaky/timing    | Add explicit wait or retry; check for race condition |
| environment     | Check server status; verify deployment; contact DevOps |

5. If previous run context is provided:
   - Flag tests that passed before but fail now → "New Failure"
   - Flag tests that fail intermittently → "Flaky"
   - Flag tests that have failed consistently → "Persistent Failure"

OUTPUT FORMAT (STRICT)

### Triage Summary

| # | Test Case ID | Status | Root Cause | Failure Summary | Suggested Action |
|---|-------------|--------|------------|-----------------|------------------|
| 1 | HMS-LOGIN-001 | Failed | bug | Login returns 500 on valid creds | Raise defect |
| 2 | HMS-REG-003 | Failed | locator issue | #submit-btn not found | Update selector |

### High-Level Analysis

- Total Passed: X
- Total Failed: X
- Total Skipped: X
- Pass %: X%

### Top Issues
1. <Most impactful category>: X failures — <pattern description>
2. <Second category>: X failures — <pattern description>

### Recommendations
- <1-3 actionable recommendations for the team>

DO NOT:
- Dump raw logs
- Explain triage methodology
- Add introductions or conclusions
- Speculate without evidence from the logs
- Classify as "unknown" — always pick the most likely category
```

## PROMPT — END

---

## How to Use

1. Copy the prompt above.
2. Paste into your AI tool.
3. Below the prompt, paste your test runner output:

```
TEST RUNNER OUTPUT:

  Authentication
    ✓ HMS-LOGIN-001 - Valid login (2.3s)
    ✗ HMS-LOGIN-002 - Invalid password shows error (4.1s)
      Error: Timed out waiting for selector '[data-testid="error-message"]'
    ✗ HMS-LOGIN-003 - SSO login redirects correctly (8.2s)
      Error: POST /api/sso/auth returned 500 Internal Server Error

  Patient Registration
    ✓ HMS-REG-001 - Register with valid data (3.5s)
    ✗ HMS-REG-002 - Duplicate email shows error (1.2s)
      Error: Expected "Email already registered" but received "Registration successful"
    ✓ HMS-REG-003 - Phone validation (1.8s)

  6 tests: 3 passed, 3 failed
```

4. Get a classified triage table with next actions.
5. Use the "Raise defect" action items with `prompts/defect-reporting.md`.

---

## Automation Integration

For automated triage, you can also feed:

- **Playwright JSON report** (`playwright-report/results.json`)
- **Cypress Mochawesome JSON** (`cypress/results/output.json`)
- **Allure results** (`allure-results/*.json`)

Just paste the relevant JSON and the AI will parse it the same way.

---

## Flaky Test Detection (Optional)

If you provide previous run results, add them like this:

```
PREVIOUS RUN (2026-02-17):
  HMS-LOGIN-002: Passed
  HMS-LOGIN-003: Failed (same error)
  HMS-REG-002: Passed
```

The AI will then flag:
- HMS-LOGIN-002 → "New Failure" (was passing, now fails)
- HMS-LOGIN-003 → "Persistent Failure" (failing across runs)
- HMS-REG-002 → "New Failure" or "Flaky" depending on history depth

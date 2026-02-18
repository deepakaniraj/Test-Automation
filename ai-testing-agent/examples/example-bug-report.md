# Example: Bug Report

> This file shows a gold-standard bug report that follows `templates/bug-report-template.md`.
> Use this as a reference when reviewing AI-generated bug reports.

---

## Bug Report

| Field                  | Value |
|------------------------|-------|
| **Bug ID**             | BUG-LOGIN-001 |
| **Title**              | [Login] HTTP 500 error when submitting valid SSO credentials |
| **Reported By**        | QA Team |
| **Reported Date**      | 2026-02-18 |
| **Environment**        | QA |
| **Browser / Device**   | Chrome 120.0.6099.130 |
| **OS**                 | Windows 11 Pro |
| **Build / Version**    | v2.3.1 (Build #1847) |
| **Severity**           | Critical |
| **Priority**           | P1 |
| **Status**             | New |
| **Linked Test Case**   | HMS-LOGIN-005 |
| **Linked Requirement** | REQ-AUTH-003 |
| **Assigned To**        | — |

---

### Preconditions

- User "john.doe@company.com" exists in the SSO provider (Azure AD).
- SSO integration is enabled for the QA environment.
- User is not currently logged in.
- Browser cache and cookies are cleared.

---

### Steps to Reproduce

| Step # | Action | Test Data |
|--------|--------|-----------|
| 1 | Navigate to the login page | URL: https://qa.hospital-app.com/login |
| 2 | Click the "Sign in with SSO" button | — |
| 3 | SSO provider login page is displayed | — |
| 4 | Enter valid SSO email | Email: john.doe@company.com |
| 5 | Click "Continue" on SSO provider page | — |
| 6 | SSO provider authenticates successfully and redirects back to application | — |
| 7 | Application displays HTTP 500 error | — |

---

### Expected Result

- After SSO authentication, the user should be redirected to the Dashboard page.
- Welcome message "Welcome, John" should be displayed.
- User session should be created.

---

### Actual Result

- After SSO redirect, the application displays a white screen with the message: **"Internal Server Error"**.
- The browser console shows: `POST /api/sso/auth returned 500 — NullReferenceException at AuthService.ValidateSSOToken()`
- No user session is created.
- The URL changes to `/error?code=500`.

---

### Evidence

| Type        | Link / Path |
|-------------|-------------|
| Screenshot  | `artifacts/screenshots/BUG-LOGIN-001-white-screen.png` |
| Screenshot  | `artifacts/screenshots/BUG-LOGIN-001-console-error.png` |
| Video       | `artifacts/videos/BUG-LOGIN-001-sso-flow.webm` |
| Error Log   | `POST /api/sso/auth 500 — System.NullReferenceException: Object reference not set to an instance of an object. at AuthService.ValidateSSOToken(String token) in /src/Services/AuthService.cs:line 142` |
| Network Log | `artifacts/logs/BUG-LOGIN-001-network-har.json` |

---

### Root Cause / Notes

- **Suspected root cause:** The SSO token validation method (`AuthService.ValidateSSOToken`) appears to receive a null token parameter, causing a `NullReferenceException`. This may indicate the SSO callback URL is not passing the token correctly to the API endpoint.
- **Reproducibility:** Consistent — 100% reproducible on QA environment.
- **Workaround:** None — SSO login is completely blocked.
- **Impact:** All SSO users (estimated 80% of user base) cannot log in.
- **Note:** Standard username/password login (non-SSO) works correctly.

---

### Retest Results (filled after fix)

| Field           | Value |
|-----------------|-------|
| Fix Build       | — |
| Retest Date     | — |
| Retest Status   | — |
| Retested By     | — |
| Notes           | — |

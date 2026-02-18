# Example: Test Cases

> This file shows gold-standard test cases that follow `templates/testcases-template.md` and `rules/test-id-rules.md`.
> Use this as a reference when reviewing AI-generated test cases.

---

### HMS-LOGIN-001: Valid Login with Correct Credentials

| Field           | Value |
|-----------------|-------|
| Test Case ID    | HMS-LOGIN-001 |
| Requirement ID  | REQ-AUTH-001 |
| Module          | Authentication |
| Flow ID         | FLOW-LOGIN-001 |
| Priority        | P1 |
| Type            | Smoke |
| Preconditions   | User "john.doe@example.com" exists in the system with password "P@ssw0rd!"; browser is open |
| Automation      | Not Automated |

**Steps:**

| # | Action | Test Data | Expected Result |
|---|--------|-----------|-----------------|
| 1 | Navigate to the login page | URL: /login | Login page is displayed with username and password fields |
| 2 | Enter valid username | Username: john.doe@example.com | Username field is populated |
| 3 | Enter valid password | Password: P@ssw0rd! | Password field is populated (masked) |
| 4 | Click the Login button | — | User is redirected to the Dashboard page |
| 5 | Verify welcome message | — | Welcome message "Welcome, John" is displayed in the header |

**Post-Conditions:** User is logged in, session is active, and Dashboard page is displayed.

---

### HMS-LOGIN-002: Login with Invalid Password

| Field           | Value |
|-----------------|-------|
| Test Case ID    | HMS-LOGIN-002 |
| Requirement ID  | REQ-AUTH-001 |
| Module          | Authentication |
| Flow ID         | FLOW-LOGIN-001 |
| Priority        | P1 |
| Type            | Regression |
| Preconditions   | User "john.doe@example.com" exists in the system; browser is open |
| Automation      | Not Automated |

**Steps:**

| # | Action | Test Data | Expected Result |
|---|--------|-----------|-----------------|
| 1 | Navigate to the login page | URL: /login | Login page is displayed |
| 2 | Enter valid username | Username: john.doe@example.com | Username field is populated |
| 3 | Enter incorrect password | Password: WrongPass123 | Password field is populated (masked) |
| 4 | Click the Login button | — | Error message "Invalid credentials" is displayed below the form |
| 5 | Verify user remains on login page | — | URL is still /login; username field retains the entered value |

**Post-Conditions:** User is not logged in; no session is created.

---

### HMS-LOGIN-003: Login with Empty Username and Password

| Field           | Value |
|-----------------|-------|
| Test Case ID    | HMS-LOGIN-003 |
| Requirement ID  | REQ-AUTH-001 |
| Module          | Authentication |
| Flow ID         | FLOW-LOGIN-001 |
| Priority        | P2 |
| Type            | Regression |
| Preconditions   | Browser is open |
| Automation      | Not Automated |

**Steps:**

| # | Action | Test Data | Expected Result |
|---|--------|-----------|-----------------|
| 1 | Navigate to the login page | URL: /login | Login page is displayed |
| 2 | Leave username field empty | — | Username field is empty |
| 3 | Leave password field empty | — | Password field is empty |
| 4 | Click the Login button | — | Validation error "Username is required" is displayed under the username field |
| 5 | Verify password validation | — | Validation error "Password is required" is displayed under the password field |

**Post-Conditions:** User remains on login page; no API call is made.

---

### HMS-LOGIN-004: Login with Username at Maximum Length (255 characters)

| Field           | Value |
|-----------------|-------|
| Test Case ID    | HMS-LOGIN-004 |
| Requirement ID  | REQ-AUTH-002 |
| Module          | Authentication |
| Flow ID         | FLOW-LOGIN-001 |
| Priority        | P3 |
| Type            | Regression |
| Preconditions   | Browser is open |
| Automation      | Not Automated |

**Steps:**

| # | Action | Test Data | Expected Result |
|---|--------|-----------|-----------------|
| 1 | Navigate to the login page | URL: /login | Login page is displayed |
| 2 | Enter a 255-character email in the username field | Username: aaa...aaa@example.com (255 chars total) | Username field accepts all 255 characters |
| 3 | Enter any password | Password: Test1234 | Password field is populated |
| 4 | Click the Login button | — | System processes the request without truncation or error (invalid credentials message is acceptable since user does not exist) |

**Post-Conditions:** System correctly handled boundary-length input without crash or unexpected behavior.

---

### HMS-LOGIN-005: Login Button is Disabled When Fields Are Empty

| Field           | Value |
|-----------------|-------|
| Test Case ID    | HMS-LOGIN-005 |
| Requirement ID  | REQ-AUTH-003 |
| Module          | Authentication |
| Flow ID         | FLOW-LOGIN-001 |
| Priority        | P2 |
| Type            | Smoke |
| Preconditions   | Browser is open |
| Automation      | Not Automated |

**Steps:**

| # | Action | Test Data | Expected Result |
|---|--------|-----------|-----------------|
| 1 | Navigate to the login page | URL: /login | Login page is displayed |
| 2 | Observe the Login button without entering any data | — | Login button is disabled (grayed out, not clickable) |
| 3 | Enter username only | Username: john.doe@example.com | Login button remains disabled |
| 4 | Enter password | Password: P@ssw0rd! | Login button becomes enabled (active, clickable) |

**Post-Conditions:** Login button state correctly reflects form completeness.

---

## Coverage Summary

| Category   | Count | Test Case IDs |
|------------|-------|---------------|
| Positive   | 1     | HMS-LOGIN-001 |
| Negative   | 2     | HMS-LOGIN-002, HMS-LOGIN-003 |
| Boundary   | 1     | HMS-LOGIN-004 |
| UI / State | 1     | HMS-LOGIN-005 |
| **Total**  | **5** | |

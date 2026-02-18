# Test ID Rules

> These rules define how Test Case IDs, Bug IDs, Flow IDs, and Requirement IDs are structured.
> All prompts and templates must follow these conventions.

---

## Test Case ID Format

```
<PROJECT>-<MODULE>-<SEQ>
```

| Component  | Description | Example |
|------------|-------------|---------|
| PROJECT    | Project abbreviation (2–5 uppercase letters) | HMS, ERP, CRM |
| MODULE     | Module or feature abbreviation (2–10 uppercase letters) | LOGIN, REG, APPT, DASH |
| SEQ        | 3-digit sequential number, zero-padded | 001, 002, 050, 100 |

### Examples

| Test Case ID     | Meaning |
|------------------|---------|
| HMS-LOGIN-001    | Hospital Management System, Login module, test #1 |
| HMS-LOGIN-002    | Hospital Management System, Login module, test #2 |
| HMS-REG-001      | Hospital Management System, Registration module, test #1 |
| ERP-INV-015      | ERP System, Invoice module, test #15 |

### Rules

1. Always use uppercase letters.
2. Separate components with hyphens (`-`).
3. Sequence numbers start at 001 and increment by 1.
4. Never skip or reuse sequence numbers within a module.
5. If the project prefix is not provided by the user, default to `TC` (e.g., `TC-LOGIN-001`).

---

## Bug ID Format

```
BUG-<MODULE>-<SEQ>
```

| Component  | Description | Example |
|------------|-------------|---------|
| BUG        | Fixed prefix | BUG |
| MODULE     | Same module abbreviation as test case IDs | LOGIN, REG |
| SEQ        | 3-digit sequential number | 001, 002 |

### Examples

| Bug ID           | Meaning |
|------------------|---------|
| BUG-LOGIN-001    | First bug found in Login module |
| BUG-REG-003      | Third bug found in Registration module |

---

## Flow ID Format

```
FLOW-<MODULE>-<SEQ>
```

| Component  | Description | Example |
|------------|-------------|---------|
| FLOW       | Fixed prefix | FLOW |
| MODULE     | Module abbreviation | LOGIN, REG, APPT |
| SEQ        | 3-digit sequential number | 001, 002 |

### Examples

| Flow ID          | Meaning |
|------------------|---------|
| FLOW-LOGIN-001   | Primary login flow |
| FLOW-REG-001     | Standard registration flow |
| FLOW-REG-002     | SSO registration flow |

---

## Requirement ID Format

```
REQ-<MODULE>-<SEQ>
```

| Component  | Description | Example |
|------------|-------------|---------|
| REQ        | Fixed prefix | REQ |
| MODULE     | Module abbreviation | AUTH, REG, APPT |
| SEQ        | 3-digit sequential number | 001, 002 |

### Examples

| Requirement ID   | Meaning |
|------------------|---------|
| REQ-AUTH-001     | First authentication requirement |
| REQ-REG-003     | Third registration requirement |

---

## ID Cross-Referencing

All IDs must be traceable to each other:

```
REQ-AUTH-001 (Requirement)
  └── FLOW-LOGIN-001 (Flow that implements this requirement)
       └── HMS-LOGIN-001 (Test case that validates this flow)
            └── BUG-LOGIN-001 (Bug found by this test case)
```

### Traceability Rules

1. Every test case MUST reference at least one Requirement ID.
2. Every bug report MUST reference the Test Case ID that discovered it.
3. Every flow MUST be referenced by at least one test case.
4. When creating automated tests, the automation script name or test function name MUST include the Test Case ID.

---

## Module Abbreviation Registry

> Maintain a project-level registry of module abbreviations to avoid duplicates.

| Abbreviation | Full Module Name |
|-------------|------------------|
| LOGIN       | Login / Authentication |
| REG         | Registration |
| APPT        | Appointment Booking |
| DASH        | Dashboard |
| PAT         | Patient Management |
| RPT         | Reports |
| ADMIN       | Administration |
| NOTIF       | Notifications |
| PROF        | User Profile |
| BILL        | Billing / Payment |

> Add new abbreviations as modules are identified. Keep abbreviations 2–10 characters, uppercase, and descriptive.

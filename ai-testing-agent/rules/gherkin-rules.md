# Gherkin Rules

> These rules define how Gherkin (Given/When/Then) steps must be written.
> Follow these when writing manual test cases in Gherkin or when converting to automation.

---

## General Gherkin Format

```gherkin
Feature: <Module or Feature Name>

  Scenario: <Test Case ID> - <Short Description>
    Given <precondition>
    When <action>
    And <additional action>
    Then <expected outcome>
    And <additional verification>
```

---

## Keyword Rules

| Keyword  | Purpose | Usage |
|----------|---------|-------|
| `Given`  | Set up preconditions | State, navigation, data setup |
| `When`   | Perform the main action | User interaction, trigger |
| `And`    | Continue previous keyword | Additional steps within same context |
| `Then`   | Verify outcome | Assertions, validations |
| `But`    | Negative verification after Then | "But error message is not displayed" |

### Rules

1. **One action per line.** Do not combine multiple actions in a single step.
   - Bad: `When user enters username and password and clicks login`
   - Good:
     ```gherkin
     When the user enters username "john.doe"
     And the user enters password "P@ssw0rd!"
     And the user clicks the Login button
     ```

2. **Use third person.** Always "the user" not "I" or "you."
   - Bad: `Given I am on the login page`
   - Good: `Given the user is on the login page`

3. **Be specific with data.** Include actual test data in quotes.
   - Bad: `When the user enters a valid email`
   - Good: `When the user enters email "john.doe@example.com"`

4. **Expected results must be verifiable.** State exactly what is visible or measurable.
   - Bad: `Then login is successful`
   - Good: `Then the user is redirected to the Dashboard page`
   - Good: `Then the welcome message "Welcome, John" is displayed`

5. **Keep steps UI-agnostic at the high level.** For business-level Gherkin, describe what the user does, not how the UI works.
   - Bad: `When the user clicks the div with class btn-primary`
   - Good: `When the user clicks the Login button`

---

## Scenario vs Scenario Outline

### Use `Scenario` when:
- Testing a single specific case with fixed data.
- The test case has unique steps that don't repeat with different data.

```gherkin
Scenario: HMS-LOGIN-001 - Valid login with correct credentials
  Given the user is on the login page
  When the user enters username "john.doe@example.com"
  And the user enters password "P@ssw0rd!"
  And the user clicks the Login button
  Then the user is redirected to the Dashboard page
```

### Use `Scenario Outline` when:
- Testing the same flow with multiple data sets.
- Data-driven testing (positive + negative with different inputs).

```gherkin
Scenario Outline: HMS-LOGIN-002 - Login with various credentials
  Given the user is on the login page
  When the user enters username "<username>"
  And the user enters password "<password>"
  And the user clicks the Login button
  Then the message "<message>" is displayed

  Examples:
    | username               | password    | message                    |
    | john.doe@example.com   | P@ssw0rd!   | Welcome, John              |
    | invalid@example.com    | wrong       | Invalid credentials        |
    |                        | P@ssw0rd!   | Username is required       |
    | john.doe@example.com   |             | Password is required       |
```

---

## Tagging Convention

Use tags to classify scenarios for selective execution:

| Tag             | Purpose |
|-----------------|---------|
| `@smoke`        | Critical path — run every time |
| `@regression`   | Core functionality — run every cycle |
| `@system`       | End-to-end flow |
| `@negative`     | Negative / error scenario |
| `@boundary`     | Boundary / edge case |
| `@P1` to `@P4`  | Priority level |
| `@<MODULE>`     | Module tag (e.g., `@LOGIN`, `@REG`) |

### Example

```gherkin
@smoke @P1 @LOGIN
Scenario: HMS-LOGIN-001 - Valid login with correct credentials
  Given the user is on the login page
  ...
```

---

## Step Library (Reusable Step Patterns)

Define reusable step patterns to keep Gherkin consistent across features:

### Navigation
- `Given the user is on the <page name> page`
- `Given the user navigates to "<URL>"`

### Form Interaction
- `When the user enters <field name> "<value>"`
- `When the user selects "<option>" from the <field name> dropdown`
- `When the user checks the <checkbox name> checkbox`
- `When the user clicks the <button name> button`

### Verification
- `Then the <element> "<text>" is displayed`
- `Then the user is redirected to the <page name> page`
- `Then the error message "<message>" is displayed`
- `Then the <field name> field shows validation error "<error>"`
- `Then the <element> is visible`
- `Then the <element> is not visible`

### State
- `Given the user is logged in as "<role>"`
- `Given a patient record exists with name "<name>"`

---

## Common Mistakes to Avoid

| Mistake | Why It's Wrong | Fix |
|---------|---------------|-----|
| Implementation details in steps | Couples Gherkin to UI implementation | Use business language |
| Multiple verifications in one Then | Hard to trace which assertion failed | One Then per verification |
| Vague Given setup | Tests may fail due to unclear preconditions | Be explicit about state and data |
| Missing And between steps | Looks like a new Given/When/Then block | Use And to continue |
| No test data in steps | Non-reproducible test | Include specific values in quotes |

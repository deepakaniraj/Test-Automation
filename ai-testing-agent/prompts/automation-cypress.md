# Prompt: Cypress + TypeScript Automation

> Copy this entire prompt and paste it into ChatGPT / Copilot.
> Then add your inputs below the prompt.

---

## PROMPT — START

```
ROLE
You are an Automation QA Engineer with 10+ years of experience in Cypress automation using TypeScript, following a Page Object + Flow pattern.

OBJECTIVE
Generate complete Cypress + TypeScript automation code for the provided flow using the exact folder structure and conventions specified below.

INPUT I WILL PROVIDE
- Feature / Flow Name
- Manual test steps or Gherkin steps
- XPath-only mode: yes / no
- Locator details (optional — generate realistic ones if not provided)
- Test data (optional — generate sample data if not provided)
- Test Case ID (optional)

FOLDER STRUCTURE (STRICT)
All generated files must follow this structure:

cypress/
  e2e/
    <module>.cy.ts           ← Test spec files
  pages/
    <module>.page.ts         ← Page object files with locators + methods
  flows/
    <module>.flow.ts         ← Reusable business logic
  types/
    <module>.types.ts        ← TypeScript interfaces/types (if needed)
  fixtures/
    <module>.json            ← Test data
  support/
    commands.ts              ← Custom Cypress commands
    e2e.ts                   ← Support file imports

FILE CONVENTIONS

1. Page Object File (pages/<module>.page.ts):
   - Class named <Module>Page
   - All locators as class properties (getters)
   - Methods for each user interaction (click, fill, select, etc.)
   - Methods return `this` for chaining where appropriate
   - If XPath mode: use cy.xpath() for all locators
   - If standard mode: use cy.get() with data-testid, [role], or CSS selectors

2. Flow File (flows/<module>.flow.ts):
   - Class named <Module>Flow
   - Instantiates the page object
   - High-level business methods (e.g., completeRegistration(), loginWithValidCredentials())
   - Each method combines multiple page interactions into one reusable step

3. Test Spec File (e2e/<module>.cy.ts):
   - Uses describe() for the module/flow name
   - Uses it() for each test case — include Test Case ID in name if provided
   - Imports flow and fixtures
   - Includes positive, negative, and edge case tests
   - Uses proper Cypress assertions (should, expect)
   - No arbitrary cy.wait(ms) — use proper waits and assertions

4. Fixture File (fixtures/<module>.json):
   - Valid data set
   - Invalid data set
   - Boundary data set
   - Data organized by scenario name

5. Types File (types/<module>.types.ts) — if applicable:
   - Interfaces for form data, API responses, or fixture shapes

LOCATOR RULES
- If XPath-only mode is requested:
  - Use cy.xpath() for every locator
  - Plugin: cypress-xpath must be imported in support/e2e.ts
- If standard mode:
  - Prefer: [data-testid="..."] > [role="..."] > CSS selector
  - Avoid brittle selectors (tag-only, positional, auto-generated classes)

CODE RULES
- Use TypeScript strict mode conventions
- Use arrow functions consistently
- Add comments mapping test steps to business actions
- Use beforeEach() for common setup (navigation, auth)
- Use afterEach() for cleanup if needed
- No console.log statements
- No hardcoded waits
- Clean, readable, production-ready code

OUTPUT FORMAT (STRICT)
For each file, output:

**File: `<path/filename>`**
```typescript
// code here
```

At the end, provide:

**Run Commands:**
- Open: `npx cypress open`
- Run: `npx cypress run --spec "cypress/e2e/<module>.cy.ts"`

DO NOT:
- Explain testing theory
- Add introductions or conclusions
- Output anything outside the code files and run commands
- Mix multiple unrelated modules in one output
```

## PROMPT — END

---

## How to Use

1. Copy the prompt above.
2. Paste into your AI tool.
3. Below the prompt, add your inputs:

```
FLOW: Patient Registration
XPATH MODE: Yes
TEST CASE ID: HMS-REG-001

STEPS:
1. Navigate to /register
2. Enter first name
3. Enter last name
4. Enter email address
5. Enter phone number (10 digits)
6. Select date of birth
7. Select gender
8. Click Register button
9. Verify confirmation message is displayed

NEGATIVE SCENARIOS:
- Submit with all fields empty
- Submit with duplicate email
- Submit with invalid phone (less than 10 digits)
- Submit with future date of birth
```

4. Review generated code against `examples/example-cypress-structure.md`.
5. Place files in your Cypress project according to the paths shown.

---

## XPath Plugin Setup

If using XPath mode, ensure `cypress-xpath` is installed:

```bash
npm install -D cypress-xpath
```

And imported in `cypress/support/e2e.ts`:

```typescript
import 'cypress-xpath';
```

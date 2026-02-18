# Example: Cypress Project Structure

> This file shows the recommended Cypress + TypeScript project structure using the Page + Flow pattern.
> Use this as a reference when generating or organizing Cypress automation code.

---

## Folder Structure

```
cypress/
├── e2e/                          ← Test spec files
│   ├── login.cy.ts
│   ├── registration.cy.ts
│   └── appointment.cy.ts
│
├── pages/                        ← Page objects (locators + element methods)
│   ├── login.page.ts
│   ├── registration.page.ts
│   └── appointment.page.ts
│
├── flows/                        ← Business logic (reusable multi-step actions)
│   ├── login.flow.ts
│   ├── registration.flow.ts
│   └── appointment.flow.ts
│
├── types/                        ← TypeScript interfaces
│   ├── user.types.ts
│   └── patient.types.ts
│
├── fixtures/                     ← Test data (JSON)
│   ├── users.json
│   ├── patients.json
│   └── appointments.json
│
├── support/
│   ├── commands.ts               ← Custom Cypress commands
│   └── e2e.ts                    ← Support file (imports, plugins)
│
├── tsconfig.json                 ← Cypress TypeScript config
└── cypress.config.ts             ← Cypress configuration
```

---

## File Examples

### Page Object: `cypress/pages/login.page.ts`

```typescript
export class LoginPage {
  // ── Locators ──────────────────────────
  private readonly usernameInput = '[data-testid="input-login-username"]';
  private readonly passwordInput = '[data-testid="input-login-password"]';
  private readonly loginButton = '[data-testid="btn-login-submit"]';
  private readonly errorMessage = '[data-testid="msg-error-login"]';
  private readonly ssoButton = '[data-testid="btn-login-sso"]';

  // ── Actions ───────────────────────────
  visit() {
    cy.visit('/login');
    return this;
  }

  enterUsername(username: string) {
    cy.get(this.usernameInput).clear().type(username);
    return this;
  }

  enterPassword(password: string) {
    cy.get(this.passwordInput).clear().type(password);
    return this;
  }

  clickLogin() {
    cy.get(this.loginButton).click();
    return this;
  }

  // ── Assertions ────────────────────────
  verifyErrorMessage(message: string) {
    cy.get(this.errorMessage).should('be.visible').and('contain.text', message);
    return this;
  }

  verifyLoginButtonDisabled() {
    cy.get(this.loginButton).should('be.disabled');
    return this;
  }
}
```

### Flow: `cypress/flows/login.flow.ts`

```typescript
import { LoginPage } from '../pages/login.page';

export class LoginFlow {
  private loginPage = new LoginPage();

  loginWithValidCredentials(username: string, password: string) {
    this.loginPage
      .visit()
      .enterUsername(username)
      .enterPassword(password)
      .clickLogin();

    // Verify successful login
    cy.url().should('include', '/dashboard');
  }

  loginWithInvalidCredentials(username: string, password: string) {
    this.loginPage
      .visit()
      .enterUsername(username)
      .enterPassword(password)
      .clickLogin();

    this.loginPage.verifyErrorMessage('Invalid credentials');
  }

  attemptLoginWithEmptyFields() {
    this.loginPage.visit().clickLogin();

    this.loginPage.verifyErrorMessage('Username is required');
  }
}
```

### Test Spec: `cypress/e2e/login.cy.ts`

```typescript
import { LoginFlow } from '../flows/login.flow';

describe('Authentication - Login', () => {
  const loginFlow = new LoginFlow();

  beforeEach(() => {
    cy.clearCookies();
    cy.clearLocalStorage();
  });

  // Smoke — Positive
  it('HMS-LOGIN-001 - Valid login with correct credentials', () => {
    cy.fixture('users').then((users) => {
      loginFlow.loginWithValidCredentials(users.valid.username, users.valid.password);
      cy.contains('Welcome, John').should('be.visible');
    });
  });

  // Regression — Negative
  it('HMS-LOGIN-002 - Login with invalid password shows error', () => {
    cy.fixture('users').then((users) => {
      loginFlow.loginWithInvalidCredentials(users.valid.username, 'WrongPass123');
    });
  });

  // Regression — Negative
  it('HMS-LOGIN-003 - Login with empty fields shows validation errors', () => {
    loginFlow.attemptLoginWithEmptyFields();
  });
});
```

### Fixture: `cypress/fixtures/users.json`

```json
{
  "valid": {
    "username": "john.doe@example.com",
    "password": "P@ssw0rd!"
  },
  "invalid": {
    "username": "invalid@example.com",
    "password": "WrongPassword"
  },
  "locked": {
    "username": "locked.user@example.com",
    "password": "P@ssw0rd!"
  }
}
```

### Support: `cypress/support/commands.ts`

```typescript
// Custom command: Login via API (bypass UI for faster setup)
Cypress.Commands.add('loginViaAPI', (username: string, password: string) => {
  cy.request({
    method: 'POST',
    url: '/api/auth/login',
    body: { username, password },
  }).then((response) => {
    expect(response.status).to.eq(200);
    window.localStorage.setItem('authToken', response.body.token);
  });
});

// Type declaration
declare namespace Cypress {
  interface Chainable {
    loginViaAPI(username: string, password: string): Chainable<void>;
  }
}
```

### Support: `cypress/support/e2e.ts`

```typescript
import './commands';

// Import XPath plugin (if using XPath mode)
// import 'cypress-xpath';

// Global error handling
Cypress.on('uncaught:exception', (err) => {
  // Return false to prevent Cypress from failing the test on uncaught exceptions
  if (err.message.includes('ResizeObserver')) {
    return false;
  }
});
```

---

## XPath Variant

If XPath-only mode is requested, the page object changes to:

```typescript
export class LoginPage {
  // ── XPath Locators ────────────────────
  private readonly usernameInput = '//input[@data-testid="input-login-username"]';
  private readonly passwordInput = '//input[@data-testid="input-login-password"]';
  private readonly loginButton = '//button[@data-testid="btn-login-submit"]';
  private readonly errorMessage = '//div[@data-testid="msg-error-login"]';

  enterUsername(username: string) {
    cy.xpath(this.usernameInput).clear().type(username);
    return this;
  }

  // ... same pattern for other methods
}
```

Requires: `npm install -D cypress-xpath` and `import 'cypress-xpath';` in `support/e2e.ts`.

---

## Run Commands

```bash
# Open Cypress UI (interactive mode)
npx cypress open

# Run all tests headless
npx cypress run

# Run specific spec
npx cypress run --spec "cypress/e2e/login.cy.ts"

# Run with specific browser
npx cypress run --browser chrome

# Run with tag (if using grep plugin)
npx cypress run --env grepTags="@smoke"
```

---

## Starter Kit Generation

To ask the AI to generate a full starter Cypress project based on this structure:

```
"Generate a starter Cypress + TypeScript kit using the Page + Flow pattern
for the <Module Name> module. Follow the structure in example-cypress-structure.md.
Include: page object, flow, test spec, fixture, and support files."
```

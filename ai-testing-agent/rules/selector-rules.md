# Selector Rules

> These rules define how UI element locators/selectors must be chosen across all automation frameworks (Cypress, Playwright, Appium).
> Follow this priority order strictly.

---

## Selector Priority (Most Preferred → Least Preferred)

| Priority | Strategy | Example | When to Use |
|----------|----------|---------|-------------|
| 1 | `data-testid` | `[data-testid="login-btn"]` | Always first choice for web |
| 2 | Accessible Role | `getByRole('button', { name: 'Login' })` | When data-testid is not available |
| 3 | Accessible Label | `getByLabel('Username')` | Form inputs with labels |
| 4 | Placeholder Text | `getByPlaceholder('Enter email')` | Inputs without visible labels |
| 5 | Text Content | `getByText('Submit')` | Buttons, links with unique text |
| 6 | CSS Selector | `input.form-control[name="email"]` | When above are not available |
| 7 | XPath | `//button[@type="submit"]` | Only when explicitly requested or no CSS alternative |

---

## Framework-Specific Selector Syntax

### Playwright

```typescript
// Priority 1: data-testid
page.locator('[data-testid="login-btn"]')
page.getByTestId('login-btn')

// Priority 2: Accessible role
page.getByRole('button', { name: 'Login' })
page.getByRole('textbox', { name: 'Username' })

// Priority 3: Label
page.getByLabel('Username')

// Priority 4: Placeholder
page.getByPlaceholder('Enter your email')

// Priority 5: Text
page.getByText('Welcome, John')

// Priority 6: CSS
page.locator('input.form-control[name="email"]')
```

### Cypress

```typescript
// Priority 1: data-testid
cy.get('[data-testid="login-btn"]')

// Priority 2–5: CSS with attributes
cy.get('[role="button"]').contains('Login')
cy.get('label').contains('Username').siblings('input')

// Priority 6: CSS
cy.get('input.form-control[name="email"]')

// Priority 7: XPath (only when XPath mode requested)
cy.xpath('//button[@data-testid="login-btn"]')
```

### Appium (Mobile)

```typescript
// Priority 1: Accessibility ID
$('~login-button')

// Priority 2: Resource ID (Android) / Name (iOS)
$('android=new UiSelector().resourceId("com.app:id/loginBtn")')
$('-ios class chain:**/XCUIElementTypeButton[`name == "Login"`]')

// NEVER: Coordinates or index-based
// BAD: $('//XCUIElementTypeButton[3]')
```

---

## Selector Naming Conventions

When defining `data-testid` values or page object property names:

### Format

```
<element-type>-<context>-<action-or-name>
```

### Examples

| data-testid | Element | Context |
|-------------|---------|---------|
| `btn-login-submit` | Button | Login form submit |
| `input-login-username` | Input | Login username field |
| `input-login-password` | Input | Login password field |
| `link-nav-dashboard` | Link | Navigation to dashboard |
| `modal-confirm-delete` | Modal | Delete confirmation dialog |
| `msg-error-login` | Message | Login error message |
| `table-patients-list` | Table | Patient list table |

### Naming Rules

1. Use lowercase with hyphens (kebab-case).
2. Prefix with element type: `btn`, `input`, `link`, `modal`, `msg`, `table`, `card`, `dropdown`, `checkbox`, `radio`, `tab`, `img`.
3. Include context (module or feature area).
4. Be descriptive but concise — max 4 segments.
5. Never use auto-generated IDs, class names, or CSS framework classes (e.g., `col-md-6`, `MuiButton-root`).

---

## Selectors to AVOID

| Bad Selector | Why | Better Alternative |
|-------------|-----|---------------------|
| `div > div > span:nth-child(3)` | Extremely brittle — any DOM change breaks it | `[data-testid="..."]` |
| `.MuiButton-root` | Framework-generated class — changes on rebuild | `getByRole('button')` |
| `#auto_generated_id_123` | Auto-generated — changes between sessions | `[data-testid="..."]` |
| `//div[3]/div[1]/button` | Positional XPath — breaks on any layout change | `//button[@data-testid="..."]` |
| `body > main > section:first-child` | Deeply coupled to DOM structure | Semantic selector |
| `[style="color: red"]` | Style can change — not semantic | `[data-testid="error-msg"]` |

---

## XPath-Only Mode

When the user requests **XPath-only** selectors:

1. Still prefer attributes over position:
   - Good: `//button[@data-testid="login-btn"]`
   - Good: `//input[@name="username"]`
   - Bad: `//div[3]/button[1]`

2. Use `contains()` for partial text matching:
   - `//button[contains(text(), "Login")]`
   - `//span[contains(@class, "error")]`

3. Use `normalize-space()` for text with extra whitespace:
   - `//button[normalize-space(text())="Log In"]`

4. For Cypress XPath mode, ensure `cypress-xpath` plugin is installed:
   ```typescript
   // cypress/support/e2e.ts
   import 'cypress-xpath';
   ```

---

## Page Object Locator Organization

When creating page objects, organize locators consistently:

```typescript
export class LoginPage {
  // ── Locators ──────────────────────────
  private readonly usernameInput = '[data-testid="input-login-username"]';
  private readonly passwordInput = '[data-testid="input-login-password"]';
  private readonly loginButton = '[data-testid="btn-login-submit"]';
  private readonly errorMessage = '[data-testid="msg-error-login"]';

  // ── Actions ───────────────────────────
  async enterUsername(value: string) { ... }
  async enterPassword(value: string) { ... }
  async clickLogin() { ... }

  // ── Assertions ────────────────────────
  async verifyErrorMessage(expected: string) { ... }
}
```

### Rules for Page Object Locators

1. Define locators as `private readonly` class properties.
2. Group locators together at the top of the class.
3. Name properties descriptively (e.g., `usernameInput` not `input1`).
4. Keep locator values as strings (selectors), not resolved elements.
5. One page object per page/screen — do not mix multiple pages.

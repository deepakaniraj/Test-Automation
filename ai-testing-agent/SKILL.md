# AI Testing Agent — Skills Definition

> This document defines all reusable skills the AI testing agent can perform.
> Each skill has a **Role**, **Objective**, **Inputs**, **Outputs**, and **Step-by-Step Process**.
> Reference the corresponding prompt file in `prompts/` and template in `templates/` for each skill.

---

## Recommended Tech Stack

| Category           | Primary             | Secondary           |
|--------------------|----------------------|----------------------|
| Web Automation     | Cypress (TypeScript) | Playwright (TypeScript) |
| Mobile Automation  | Appium (iOS + Android) | — |
| Test Management    | Allure Reports       | Jira / GitHub Issues |
| Frameworks         | Jest, WebdriverIO    | — |
| CI/CD              | GitHub Actions       | Jenkins              |
| Dashboard          | React / Power BI / Grafana | Static HTML/JS |

---

## Key Implementation Steps

1. Define skill folder structure
2. Create templates and rules
3. Configure AI prompts
4. Set up automation frameworks
5. Establish governance & traceability

---

## Skill 1 — Manual Testcase Generation

**Role:** You are a Senior QA Engineer with 10+ years of experience in manual testing, test design, and test case authoring.

**Objective:** Generate comprehensive, well-structured manual test cases from requirements, user stories, or feature descriptions using the standardized test case template.

**Prompt File:** `prompts/manual-testing.md`
**Template File:** `templates/testcases-template.md`

### Inputs

| Input               | Required | Description |
|----------------------|----------|-------------|
| Feature / Module Name | Yes     | Name of the feature or module under test |
| Requirements / User Story | Yes | SRS, user story, acceptance criteria, or feature description |
| Flow ID              | Optional | Workflow identifier (e.g., FLOW-LOGIN-001) |
| Priority Guidance    | Optional | Which priorities to focus on (P1 critical paths first) |
| Edge Cases / Negatives | Optional | Specific negative or boundary scenarios to include |

### Outputs

- Complete test cases in the format defined by `templates/testcases-template.md`
- Each test case includes: Test Case ID, Title, Module, Preconditions, Steps, Expected Result, Priority, Type (Smoke/Regression/System), Automation Status
- Coverage across: positive flows, negative flows, boundary/edge cases, validation checks

### Step-by-Step Process

1. Parse the provided requirements/story and identify all testable features and acceptance criteria.
2. Break each feature into distinct functional paths (happy path, alternative paths, error paths).
3. For each path, generate one or more test cases using the test case template fields.
4. Assign Test Case IDs following the convention in `rules/test-id-rules.md`.
5. Classify each test case by type: Smoke (critical path), Regression (core functionality), or System (end-to-end).
6. Assign priority: P1 (critical), P2 (high), P3 (medium), P4 (low).
7. Include at least one negative test and one boundary/edge case per feature area.
8. Set Automation Status to "Not Automated" for all generated test cases (will be updated when automation scripts are created).
9. Output only the test cases — no theory, no explanations, no repeated requirements.

---

## Skill 2 — Gherkin to Playwright Automation (TypeScript)

**Role:** You are a Senior SDET with 15+ years of experience in Playwright automation using TypeScript. You convert manual Gherkin test cases into clean, maintainable, production-ready Playwright scripts.

**Objective:** Convert a provided Gherkin manual test case into a fully working Playwright automation test script following POM conventions.

**Prompt File:** `prompts/automation-playwright.md`
**Rule Files:** `rules/selector-rules.md`, `rules/gherkin-rules.md`

### Inputs

| Input            | Required | Description |
|------------------|----------|-------------|
| Test Case ID     | Yes      | Unique test case identifier |
| Requirement ID   | Yes      | Linked requirement or user story ID |
| Gherkin Steps    | Yes      | Full Given / When / And / Then steps |
| Module Name      | Yes      | Module or feature name for the describe block |
| URL              | Optional | Target application URL |
| Locator Details  | Optional | Element locators; if absent, AI generates realistic ones |

### Outputs

- Complete, executable Playwright TypeScript test file
- Proper `test.describe()` block named by Module Name
- Proper `test()` with Test Case ID in the name
- Comments mapping each Gherkin step to code
- No extra prose, no repeated Gherkin, no theory

### Conversion Rules (Strict)

| Gherkin Step                        | Playwright Code                                  |
|-------------------------------------|--------------------------------------------------|
| Given user is on login page         | `await page.goto('<URL>');`                       |
| When user enters username           | `await page.fill(locator, value);`               |
| And user clicks login               | `await page.click(locator);`                     |
| Then error message should display   | `await expect(locator).toBeVisible();`            |

- Use `@playwright/test` with `import { test, expect }`
- Use `await expect()` for all assertions
- Use recommended locator strategies: `getByRole`, `getByLabel`, `locator`, `data-testid`
- No hard waits (`waitForTimeout`) — use auto-waiting best practices
- Add comments mapping each step to the original Gherkin line
- No unnecessary console logs or dummy selectors

### Locator Strategy (When Not Provided)

1. Prefer `data-testid` attributes with realistic names
2. Otherwise use accessible roles or labels
3. Avoid XPath unless absolutely necessary

### Step-by-Step Process

1. Validate that all required inputs are present; infer minimally if something is missing.
2. Derive a short, meaningful description from the Gherkin for the test name.
3. Structure the file: import → describe block (Module Name) → test (Test Case ID + description).
4. Convert each Gherkin step into Playwright code with a comment referencing the original step.
5. Use stable selectors per the locator rules.
6. Ensure all expectations use `await expect()` with auto-waiting.
7. Review: correct naming, clean formatting, no extra logging or commentary.

---

## Skill 3 — Cypress Automation (TypeScript)

**Role:** You are an Automation QA Engineer specializing in Cypress with TypeScript.

**Objective:** Generate Cypress + TypeScript automation code using a page + flow pattern and the standard folder structure (e2e, pages, flows, types, fixtures, support).

**Prompt File:** `prompts/automation-cypress.md`
**Rule Files:** `rules/selector-rules.md`, `rules/output-format-rules.md`
**Example:** `examples/example-cypress-structure.md`

### Inputs

| Input            | Required | Description |
|------------------|----------|-------------|
| Feature / Flow Name | Yes   | Name of the flow to automate (e.g., Patient Registration) |
| Steps or Gherkin | Yes      | Manual test steps or Gherkin for the flow |
| XPath-Only Mode  | Optional | If yes, use XPath-only selectors |
| Test Data        | Optional | Specific data for fixtures |
| Edge Cases       | Optional | Negative or boundary scenarios to cover |

### Outputs

- **Page object files** (`pages/`) with locators and element interaction methods
- **Flow files** (`flows/`) with reusable business logic
- **Test spec files** (`e2e/`) with assertions and stable waits
- **Fixture files** (`fixtures/`) with sample and boundary test data
- **Type definitions** (`types/`) if needed
- **Run commands** and file paths for execution

### Rules

- Use Cypress + TypeScript
- Follow folder structure: `e2e/`, `pages/`, `flows/`, `types/`, `fixtures/`, `support/`
- Use XPath-only selectors when explicitly requested; otherwise use stable selectors
- Follow page + flow pattern: pages for locators/actions, flows for business steps
- Include assertions and stable waits — no arbitrary `cy.wait(ms)`
- Provide `npx cypress run` and `npx cypress open` commands
- Show file paths for every generated file

### Step-by-Step Process

1. Parse the flow and break it into pages and reusable actions.
2. For each page, create a page object with locators and methods (under `pages/`).
3. For each business use case, create flow methods (under `flows/`).
4. Create Cypress test specs (under `e2e/`) that import flows and fixtures.
5. Define fixtures (under `fixtures/`) for representative test data.
6. Reference Test Case IDs when provided.
7. Output code only — no theory or explanations.

---

## Skill 4 — Mobile Automation (Appium — iOS & Android)

**Role:** You are a Senior Mobile SDET with deep experience in Appium cross-platform automation.

**Objective:** Generate cross-platform Appium automation for a given mobile flow including capability configs, page objects, and test cases for both iOS and Android.

**Prompt File:** `prompts/automation-appium.md`
**Rule Files:** `rules/selector-rules.md`

### Inputs

| Input            | Required | Description |
|------------------|----------|-------------|
| Flow Name        | Yes      | Mobile flow to automate (e.g., Patient Login) |
| Platform Targets | Yes      | iOS, Android, or both |
| App Details      | Yes      | Package/bundle IDs, main activity (Android), app path |
| Flow Steps       | Yes      | Manual steps or Gherkin for the flow |
| Device Matrix    | Optional | Simulators, emulators, or real devices |

### Outputs

- **Capability configurations** for iOS and Android
- **Mobile page objects** using accessibility IDs
- **Cross-platform test cases** that reuse logic for both platforms
- **Execution instructions** (how to run tests locally)

### Step-by-Step Process

1. Define separate capability configs for iOS and Android.
2. Identify all screens in the flow and create page objects for each.
3. Use accessibility IDs (preferred) or robust selectors — no coordinates or fragile XPaths.
4. Implement page object methods for common actions.
5. Create cross-platform test cases parameterized by platform.
6. Provide execution instructions (tools, commands, environment setup).

---

## Skill 5 — Defect Reporting

**Role:** You are a QA defect reporting assistant.

**Objective:** Generate structured, professional bug reports from failed test case information, screenshots, and logs, matching the bug report template.

**Prompt File:** `prompts/defect-reporting.md`
**Template File:** `templates/bug-report-template.md`

### Inputs

| Input              | Required | Description |
|--------------------|----------|-------------|
| Failed Test Case   | Yes      | Test Case ID, title, steps, expected vs actual |
| Screenshot References | Optional | Paths or URLs to failure screenshots |
| Logs / Error Messages | Optional | Stack traces or error output |
| Environment        | Optional | QA/UAT/Prod, browser, OS, build info |

### Outputs

- Structured defect report matching `templates/bug-report-template.md`
- Fields: Bug ID (placeholder), Title, Environment, Steps to Reproduce, Actual vs Expected, Severity/Priority, Evidence Links, Linked Test Case IDs
- Optional Jira/GitHub-ready payload

### Step-by-Step Process

1. Extract clear repro steps from the failed test case details.
2. Format actual vs expected results concisely.
3. Reference evidence explicitly (screenshots, logs, videos).
4. Classify severity based on impact if not provided.
5. Link to Test Case ID and Requirement ID.
6. Output only the bug report — concise, professional, no informal commentary.

---

## Skill 6 — Execution Triage

**Role:** You are a test execution triage assistant.

**Objective:** Read test runner output logs, summarize failures, classify root causes, and suggest next actions.

**Prompt File:** `prompts/execution-triage.md`

### Inputs

| Input                | Required | Description |
|----------------------|----------|-------------|
| Test Runner Output   | Yes      | Logs from Cypress, Playwright, or other runner |
| Test ID → Module Map | Optional | Mapping of test IDs to modules/requirements |
| Previous Run Context | Optional | Earlier run results for flaky test detection |

### Outputs

- Structured failure summary table:

| Test Case ID | Status | Root Cause Category | Failure Summary | Suggested Action |
|---|---|---|---|---|
| TC-001 | Failed | bug | Login button returns 500 | Raise defect |

- Root cause categories: **bug**, **locator issue**, **data issue**, **flaky/timing**, **environment**
- Optional high-level summary (top problem categories, patterns)

### Step-by-Step Process

1. Parse test runner output and identify all failed tests.
2. For each failure, extract error message and relevant context.
3. Classify root cause into one of: bug, locator issue, data issue, flaky/timing, environment.
4. Suggest a concrete next action (raise defect, update locator, fix test data, add retry/wait, check environment).
5. If previous run context is provided, flag flaky tests (passed before, failed now, or vice versa).
6. Output structured table — no raw log dumps, only references or short snippets.

---

## Skill 7 — Playwright Execution Tracking + Auto Bug Reporting System

**Role:** You are a Senior QA Automation Architect with 20+ years of experience building enterprise-grade automation ecosystems using Playwright, CI/CD, reporting tools, and defect tracking integration.

**Objective:** Design and implement a complete automation framework that tracks daily executed test case counts, maintains a master execution dashboard, and automatically creates Jira bugs with evidence when tests fail.

### Tech Stack

- Playwright (TypeScript)
- Playwright HTML Reporter + Allure Reporter
- CI/CD (GitHub Actions / Jenkins)
- Jira REST API (for auto bug creation)
- JSON execution result export
- Video + Screenshot capture
- Optional dashboard: React / Power BI / Grafana

### Required Features

#### 7.1 — Execution Tracking

Track per run:
- Total test cases executed
- Passed / Failed / Skipped counts
- Pass percentage
- Execution timestamp
- Environment (QA / UAT / Prod)

#### 7.2 — Master Execution Tracker

Files:
- `master-execution.json` — latest run summary
- `execution-history.json` — append-only array of all runs

Each entry:
```json
{
  "executionDate": "2026-02-18",
  "total": 120,
  "passed": 110,
  "failed": 8,
  "skipped": 2,
  "passPercentage": "91.6%",
  "environment": "QA"
}
```

#### 7.3 — Auto Bug Creation on Failure

When a test fails, automatically:
1. Capture screenshot
2. Capture video
3. Capture stack trace
4. Create Jira bug via REST API
5. Attach screenshot, video, and error log
6. Include Test Case ID in bug title
7. Include Requirement ID in bug description
8. De-duplicate: if open bug exists for same testId + error signature, add comment instead of new issue

#### 7.4 — Visibility to Users

Generate after each run:
- Playwright HTML report
- Allure report
- Public dashboard link
- Execution summary email
- Exportable CSV / Excel summary

Visible fields per test:
- Test Case ID, Status, Duration, Screenshot preview, Failure reason

#### 7.5 — Daily Test Case Count Update

Generate:
- `daily-summary.csv`
- `master-execution.xlsx`

| Date | Total | Passed | Failed | Skipped | Pass % | Environment |
|------|-------|--------|--------|---------|--------|-------------|

### Framework Folder Structure

```
project-root/
├── playwright.config.ts
├── package.json
├── tsconfig.json
├── tests/                    # Test specs organized by module
├── pages/                    # Page objects
├── fixtures/                 # Auth, test data
├── config/                   # Environment configs (QA/UAT/Prod)
├── integrations/
│   └── jira/                 # Jira API client + helpers
├── reporting/
│   ├── custom-reporter.ts    # Custom reporter for tracking
│   └── summary-generator.ts  # JSON/CSV/Excel generator
├── artifacts/
│   ├── screenshots/          # Per-run screenshots
│   ├── videos/               # Per-run videos
│   ├── allure-results/       # Raw Allure data
│   └── html-report/          # Playwright HTML report
├── exports/
│   ├── master-execution.json
│   ├── execution-history.json
│   ├── daily-summary.csv
│   └── master-execution.xlsx
├── dashboard/                # Static dashboard app
└── .github/
    └── workflows/
        └── playwright-tests.yml
```

### Step-by-Step Process

1. Scaffold Playwright TS project with the folder structure above.
2. Configure reporters: HTML, Allure, JSON, custom execution tracker.
3. Configure screenshots and video capture on failure.
4. Add global hooks (afterEach) for failure detection and artifact collection.
5. Implement Jira integration module with de-duplication logic.
6. Implement summary generator: read Playwright JSON results → compute metrics → write JSON/CSV/Excel → update history.
7. Set up CI workflow (GitHub Actions) with steps: checkout, install, test, generate reports, publish artifacts.
8. Generate/refresh dashboard assets reading JSON history.
9. Document run commands for local and CI execution.

---

## Skill 8 — Daily Execution Update

**Role:** You are a QA reporting assistant.

**Objective:** Generate daily execution update reports from test run results using the daily update template.

**Template File:** `templates/daily-update-template.md`

### Inputs

| Input            | Required | Description |
|------------------|----------|-------------|
| Test Run Results | Yes      | Pass/fail/skip counts from latest run |
| Environment      | Yes      | QA, UAT, or Prod |
| New Bugs Logged  | Optional | Bug IDs raised today |
| Retests Done     | Optional | Bug IDs retested and results |
| Risks / Blockers | Optional | Any blockers or risks |

### Outputs

- Completed daily update matching `templates/daily-update-template.md`
- Fields: Date, Environment, Suites Run, Pass/Fail/Skip/Blocked counts, New Bugs, Retests, Risks, Plan for Next Day

### Step-by-Step Process

1. Parse provided run results and compute totals.
2. Fill in the daily update template with all available data.
3. List new bugs raised with IDs and brief summaries.
4. List retested bugs with pass/fail status.
5. Note any risks, blockers, or environment issues.
6. Suggest plan for next day based on current status.

---

## Skill 9 — AI-Powered Requirement Analysis & Test Planning

**Role:** You are a Senior QA Analyst and Test Strategist with 15+ years of experience in requirement analysis, risk-based test planning, and test design.

**Objective:** Analyze requirements/user stories and produce a structured feature summary, risk assessment, suggested test areas, and ambiguity list — as the first step before generating full test cases.

**Prompt File:** `prompts/ai-requirement-analysis.md`
**Template File:** `templates/srs-sds-template.md` (for structuring the input)

### Inputs

| Input                | Required | Description |
|----------------------|----------|-------------|
| Requirement Document | Yes      | SRS, user story, feature spec, or design doc |
| Application Context  | Optional | App type, domain, existing modules |
| Known Risks          | Optional | Any known constraints, compliance needs, or high-risk areas |

### Outputs

- Feature summary table (name, module, roles, goals, flows, integrations)
- Risk assessment table (risk area, level, justification, testing impact)
- Suggested test areas (numbered, with type, priority, flow reference)
- Ambiguities and questions for the team
- Recommended test types (smoke, regression, security, performance, accessibility, visual)

### Step-by-Step Process

1. Parse the full requirement document — do not skip sections.
2. Identify all user roles (explicit and implied).
3. Map every flow (happy paths, alternative paths, exception paths) — assign Flow IDs.
4. Assess risk by considering: data sensitivity, complexity, integration points, user impact, regulatory needs.
5. Suggest test areas covering: positive, negative, boundary, security, usability.
6. Flag every ambiguity, missing detail, or contradiction — never assume silently.
7. Recommend which test types are applicable and why.
8. Output only the structured analysis — no theory, no introductions.

---

## AI-Era Testing Workflow

> This section maps how AI transforms the traditional 14-step manual workflow into a 7-step AI-augmented workflow. Humans and AI work cooperatively at every stage.

### Traditional → AI-Augmented Mapping

| AI Step | AI-Augmented Process | Traditional Steps Covered | AI Skill Used |
|---------|----------------------|---------------------------|---------------|
| 1 | **Automated Requirement Ingestion** — AI parses user stories and design docs, summarizes features, generates risk-based test plan | Steps 1–3 (KT, workflow analysis, app exploration) | Skill 9 |
| 2 | **Smart Test Design** — AI drafts test case outlines, suggests edge cases, writes Gherkin scenarios | Steps 4–6 (templates, test case writing, peer review) | Skill 1 + Skill 9 |
| 3 | **Automated Test Creation** — AI generates automation scripts (Cypress/Playwright/Appium) with self-healing locators | Step 7 (smoke test automation) | Skills 2, 3, 4 |
| 4 | **Parallel Exploratory Testing** — Humans test complex/new areas while AI automation runs; AI highlights high-risk areas and past bug clusters | Step 8 (test execution) | Human + Skill 6 |
| 5 | **AI-Orchestrated Execution & Analysis** — AI runs tests in CI/CD, filters flaky failures, categorizes issues, recommends root causes | Steps 8–10 (execution, bug reporting, retesting) | Skills 5, 6, 7 |
| 6 | **Auto Bug Triage & QA Reports** — AI fills bug reports, classifies issues, generates dashboards and summary reports | Steps 9, 14 (bug report, final docs) | Skills 5, 7, 8 |
| 7 | **Continuous Learning** — Tester reviews AI outputs, corrects mistakes, feeds back into prompts; AI models improve over time | Steps 10–13 (retest, regression, final verification) | All Skills |

### Key Efficiency Gains

- **~40% of tester time** is typically spent maintaining test suites — AI self-healing and auto-generation reduce this dramatically
- **~60–80% faster CI feedback** through predictive test selection and parallel AI-driven execution
- **Higher coverage** through AI-generated test cases that catch edge cases and boundary conditions humans might miss
- **Faster bug resolution** through AI-drafted bug reports with auto-attached evidence

---

## AI Tools and Techniques Catalog

> Reference catalog of AI tools and techniques relevant to each testing activity.

### LLMs for QA

| Tool | Use Case |
|------|----------|
| GPT-4 / Claude / Gemini | Test case generation, script writing, requirement analysis, bug report drafting |
| GitHub Copilot | Code completion for automation scripts, unit test generation |
| Prompt Engineering | A core QA skill — crafting effective prompts using this agent's `prompts/` library |

### Codeless AI Test Platforms

| Tool | Use Case |
|------|----------|
| TestSigma | NLP-based test creation from plain English |
| testRigor | Autonomous test creation and adaptation as app evolves |
| Functionize | ML-powered codeless test automation |
| mabl | Self-healing web test automation with smart locators |
| Testim | AI-driven UI test authoring with smart element recognition |

### Visual Testing

| Tool | Use Case |
|------|----------|
| Applitools | Computer vision for cross-browser/device visual validation |
| Percy | Visual regression snapshot comparison |

### Self-Healing Frameworks

| Technique | How It Works |
|-----------|-------------|
| Smart Locators (mabl, Testim) | ML models identify elements even when selectors change |
| AI XPath Healing | Automatically updates broken XPaths based on DOM analysis |
| Image-Based Recognition | Finds elements by visual appearance, not DOM structure |

### Defect Prediction & Triage

| Technique | How It Works |
|-----------|-------------|
| ML Defect Prediction | Models trained on code changes and past defects predict where bugs are likely |
| NLP Bug Triage | Classifies bug reports by component, severity, and suggests fixes |
| Anomaly Detection | Monitors logs and metrics to detect unexpected behaviors |

### Test Data Generation

| Technique | How It Works |
|-----------|-------------|
| LLM-Based Generation | GPT generates realistic form inputs, user profiles, edge-case data |
| GAN-Based Synthesis | Creates realistic datasets preserving statistical properties without real PII |
| Synthetic Data for Compliance | Generates GDPR/HIPAA-safe test data automatically |

### CI/CD Intelligence

| Technique | How It Works |
|-----------|-------------|
| Predictive Test Selection | ML models pick which tests to run per build based on code changes |
| Flakiness Detection | Statistical analysis of test history to flag unreliable tests |
| Smart Scheduling | AI optimizes test parallelization and ordering for fastest feedback |

---

## Tester Roles and Skills in the AI Era

> Testers are not replaced by AI — they evolve into AI-augmented QA professionals.

### How the Role Changes

| Traditional Role | AI-Era Role |
|-----------------|-------------|
| Write test cases manually | Review and refine AI-generated test cases |
| Maintain automation scripts | Supervise self-healing automation; fix edge cases |
| Execute tests manually | Design test strategy; let AI execute |
| Write bug reports from scratch | Validate and enrich AI-drafted bug reports |
| Track coverage in spreadsheets | Analyze AI-generated dashboards and reports |

### Skills to Develop

| Skill | Why It Matters |
|-------|---------------|
| **Prompt Engineering** | Crafting effective prompts is now a core QA skill — this agent's `prompts/` folder is your toolkit |
| **AI Tool Configuration** | Setting up and tuning AI platforms (mabl, Testim, Copilot) |
| **Data Literacy** | Understanding model behavior, synthetic data, bias detection |
| **Critical Thinking** | Validating AI output, catching hallucinations, spotting missing coverage |
| **Automation Engineering** | Reading, debugging, and extending AI-generated code |
| **Domain Expertise** | Business logic, compliance rules, and user context that AI cannot infer |
| **Communication** | Explaining AI-assisted quality to stakeholders; managing expectations |

### Core Value of Human Testers

> *"We as test engineers will be valuable in the AI era for characteristics: the ability to validate and judge AI tool output, and knowledge of what to test and what NOT to test."* — Ministry of Testing

- **Intuition:** Recognizing when something "feels wrong" even if tests pass
- **Context:** Understanding business impact, user behavior, and domain rules
- **Creativity:** Designing scenarios AI wouldn't think of (usability, accessibility, edge cases)
- **Judgment:** Deciding what to test, what to skip, and when quality is "good enough"
- **Ethics:** Ensuring AI tools are used responsibly and transparently

---

## Guardrails Reference

> All skills in this agent must comply with `rules/ai-guardrails.md`.

Key rules summary:

1. **Never blindly trust AI output** — always review before submitting
2. **Never over-rely on AI** — continue exploratory testing
3. **Protect data privacy** — no PII, secrets, or production data in prompts
4. **Guard against model drift** — re-validate when requirements change
5. **Manage toolchain complexity** — start small, integrate well
6. **Maintain human-in-the-loop** — tester reviews every output
7. **Use AI ethically** — no faking results, acknowledge AI assistance

See `rules/ai-guardrails.md` for the full checklist and detailed "What NOT to do" rules.

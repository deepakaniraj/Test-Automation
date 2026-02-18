# AI Testing Agent

> A structured AI-powered QA assistant that converts your 14-step manual testing workflow into reusable skills, templates, prompts, and rules — ready for use with ChatGPT, Copilot, or any LLM.

---

## Folder Structure

```
ai-testing-agent/
│
├── SKILL.md                          ← All skill definitions (roles, inputs, outputs, steps)
├── README.md                         ← This file — how to use everything
│
├── templates/                        ← Standardized document templates
│   ├── testcases-template.md         ← Manual test case format
│   ├── bug-report-template.md        ← Defect report format
│   ├── daily-update-template.md      ← Daily execution update format
│   └── srs-sds-template.md           ← Requirements / design document format
│
├── prompts/                          ← AI prompt instructions per skill
│   ├── manual-testing.md             ← Generate manual test cases
│   ├── automation-cypress.md         ← Generate Cypress + TS automation
│   ├── automation-playwright.md      ← Generate Playwright + TS automation
│   ├── automation-appium.md          ← Generate Appium mobile automation
│   ├── defect-reporting.md           ← Draft bug reports from failures
│   ├── execution-triage.md           ← Triage test failures and classify root cause
│   └── ai-requirement-analysis.md    ← AI-powered requirement analysis & test planning
│
├── rules/                            ← Strict formatting and convention rules
│   ├── output-format-rules.md        ← How AI output must be structured
│   ├── test-id-rules.md              ← Test Case ID naming conventions
│   ├── gherkin-rules.md              ← Gherkin writing standards
│   ├── selector-rules.md             ← UI locator strategy rules
│   └── ai-guardrails.md              ← Mandatory guardrails for responsible AI use
│
└── examples/                         ← Gold-standard example outputs
    ├── example-testcases.md          ← Sample test cases
    ├── example-bug-report.md         ← Sample bug report
    └── example-cypress-structure.md  ← Sample Cypress project layout
```

---

## How It Maps to Your 14-Step Workflow

| Step | Manual Process                          | AI Agent Support                                      |
|------|-----------------------------------------|-------------------------------------------------------|
| 1    | Receive KT                             | Use `srs-sds-template.md` to structure KT notes       |
| 2    | Understand the workflow                 | Document flows with IDs (FLOW-XXX-001)                |
| 3    | Explore the application                | Manual — note screens, fields, and behaviors          |
| 4    | Prepare templates                      | `templates/` folder — ready to use                    |
| 5    | Train ChatGPT for testcase generation  | `prompts/manual-testing.md` + `rules/` = trained AI   |
| 6    | Write test cases + peer review         | **Skill 1** generates test cases; you review           |
| 7    | Perform smoke testing                  | **Skill 2/3** generates automation; tag `@smoke`       |
| 8    | Execute test cases                     | **Skill 7** tracks execution with reports             |
| 9    | Update bug report with evidence        | **Skill 5** drafts bug reports; **Skill 7** auto-creates Jira bugs |
| 10   | Retest fixed issues                    | Re-run tagged tests; **Skill 6** triages results       |
| 11   | Do regression testing                  | Run `@regression` suite; **Skill 7** tracks results    |
| 12   | Update test cases with changed flow    | Re-run **Skill 1** for changed features                |
| 13   | Final system testing                   | Run full suite; **Skill 7** generates master report    |
| 14   | Submit final documents                 | **Skill 8** generates daily updates; exports CSV/Excel |

---

## AI-Augmented Workflow (7 Steps)

> AI compresses the 14 manual steps into 7 cooperative human+AI steps. See `SKILL.md → AI-Era Testing Workflow` for the full mapping.

| AI Step | Process | Skills Used |
|---------|---------|-------------|
| 1 | **Automated Requirement Ingestion** — AI parses docs, summarizes features, generates risk-based test plan | Skill 9 |
| 2 | **Smart Test Design** — AI drafts test cases, edge cases, Gherkin scenarios | Skill 1 + 9 |
| 3 | **Automated Test Creation** — AI generates Cypress / Playwright / Appium scripts | Skills 2, 3, 4 |
| 4 | **Parallel Exploratory Testing** — Humans test complex areas while AI runs automation | Human + Skill 6 |
| 5 | **AI-Orchestrated Execution & Analysis** — CI/CD runs, flaky filter, root cause | Skills 5, 6, 7 |
| 6 | **Auto Bug Triage & QA Reports** — AI fills bug reports, dashboards, summaries | Skills 5, 7, 8 |
| 7 | **Continuous Learning** — Tester reviews AI outputs, corrects, feeds back | All Skills |

### New in AI-Era

| File | Purpose |
|------|---------|
| `prompts/ai-requirement-analysis.md` | Prompt for AI-powered requirement ingestion and test planning |
| `rules/ai-guardrails.md` | Mandatory guardrails: never blindly trust AI, protect data, maintain human-in-the-loop |
| `SKILL.md → Skill 9` | AI-Powered Requirement Analysis & Test Planning skill definition |
| `SKILL.md → AI Tools Catalog` | Reference catalog of AI tools for each testing activity |
| `SKILL.md → Tester Roles` | How the tester role evolves in the AI era |

---

## How to Use

### 1. Pick a Skill

Open `SKILL.md` and find the skill you need (e.g., "Skill 1 — Manual Testcase Generation").

### 2. Prepare Your Inputs

Each skill lists required and optional inputs in a table. Gather those before prompting the AI.

### 3. Copy the Prompt

Open the corresponding file in `prompts/` (e.g., `prompts/manual-testing.md`). Copy the full prompt text.

### 4. Add Your Inputs

Paste the prompt into ChatGPT / Copilot / your AI tool, then add your specific inputs below it (requirements, Gherkin steps, flow name, etc.).

### 5. Apply Rules

The AI prompt references rules from `rules/`. These are automatically embedded in the prompt files, but you can also paste specific rules for stricter control.

### 6. Review Output Against Templates

Compare the AI's output against the matching template in `templates/` to ensure all fields are present and formatted correctly.

### 7. Use Examples as Reference

If the output doesn't match your expectations, share the relevant example from `examples/` as a "gold standard" for the AI to follow.

---

## Quick Start Examples

### Generate Manual Test Cases

```
1. Open prompts/manual-testing.md — copy the full prompt
2. Add your inputs:
   - Module: "Patient Registration"
   - Requirements: (paste your user story or SRS section)
3. Paste into ChatGPT
4. Review output against templates/testcases-template.md
```

### Generate Cypress Automation

```
1. Open prompts/automation-cypress.md — copy the prompt
2. Add your inputs:
   - Flow: "Patient Registration"
   - Steps: (paste Gherkin or manual steps)
   - XPath mode: yes/no
3. Paste into ChatGPT
4. Review output against examples/example-cypress-structure.md
```

### Generate Playwright Automation from Gherkin

```
1. Open prompts/automation-playwright.md — copy the prompt
2. Add your inputs:
   - Test Case ID: TC-LOGIN-001
   - Requirement ID: REQ-AUTH-001
   - Gherkin: Given/When/Then steps
   - Module: "Authentication"
3. Paste into ChatGPT
4. Get a ready-to-run .spec.ts file
```

### Report a Defect

```
1. Open prompts/defect-reporting.md — copy the prompt
2. Add: failed test details, screenshot paths, logs
3. Paste into ChatGPT
4. Get a bug report matching templates/bug-report-template.md
```

### Triage Test Failures

```
1. Open prompts/execution-triage.md — copy the prompt
2. Paste your test runner output (Cypress/Playwright logs)
3. Get: classified failures table + suggested actions
```

---

## Conventions

- **Test Case IDs:** Follow `rules/test-id-rules.md` — format: `<PROJECT>-<MODULE>-<SEQ>` (e.g., `HMS-LOGIN-001`)
- **Bug IDs:** Format: `BUG-<MODULE>-<SEQ>` (e.g., `BUG-LOGIN-001`)
- **Flow IDs:** Format: `FLOW-<MODULE>-<SEQ>` (e.g., `FLOW-REG-001`)
- **Gherkin:** Follow `rules/gherkin-rules.md` for Given/When/Then style
- **Selectors:** Follow `rules/selector-rules.md` — prefer `data-testid` > accessible roles > CSS > XPath
- **Output Format:** Follow `rules/output-format-rules.md` — no extra theory, strict template compliance

---

## Contributing

To add a new skill:

1. Define it in `SKILL.md` with Role, Objective, Inputs, Outputs, and Step-by-Step Process.
2. Create a matching prompt file in `prompts/`.
3. If it needs a template, add one to `templates/`.
4. If it needs an example, add one to `examples/`.
5. Update this README with the new mapping.

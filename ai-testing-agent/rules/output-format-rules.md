# Output Format Rules

> These rules govern how the AI must structure its output for all skills.
> Reference this file from any prompt that requires strict formatting.

---

## General Rules

1. **No theory or explanations.** Output only the requested artifact (test cases, code, bug report, triage table, etc.). Do not explain methodology, concepts, or rationale unless explicitly asked.

2. **No introductions or conclusions.** Do not start with "Here are the test cases..." or end with "Let me know if you need anything else." Start directly with the output.

3. **No commentary outside the format.** Do not add notes, tips, or suggestions outside the defined template structure. If you must add a note, place it under a clearly labeled "Notes" section at the very end.

4. **Strict template compliance.** Use the exact headings, field names, and table structures defined in the corresponding template file (`templates/`). Do not rename fields, skip sections, or add extra columns.

5. **Use Markdown formatting.** All output must be valid Markdown with proper table syntax, heading levels, and code blocks.

---

## Test Case Output Rules

- Use the exact format from `templates/testcases-template.md`.
- Each test case is a separate section with heading `### <Test Case ID>: <Title>`.
- Field table first, then steps table, then post-conditions.
- Separate test cases with `---` horizontal rule.
- Do not combine multiple verifications in a single step row.
- Expected results must be specific and verifiable — no vague language.

### Forbidden Phrases in Expected Results

| Forbidden | Use Instead |
|-----------|-------------|
| "should work" | "User is redirected to dashboard" |
| "page loads correctly" | "Dashboard page is displayed with welcome message" |
| "no error" | "No error message is displayed; form is submitted" |
| "works fine" | "Record is saved and confirmation toast appears" |
| "as expected" | State the specific expected behavior |

---

## Code Output Rules

- Output each file as a separate block with a **File:** header showing the full relative path.
- Use fenced code blocks with the correct language tag (```typescript, ```json, etc.).
- Include run commands at the end.
- No placeholder comments like `// TODO` or `// Add your code here` — all code must be functional.
- No `console.log` unless it serves a specific debugging purpose requested by the user.

### File Header Format

```
**File: `cypress/pages/login.page.ts`**
```typescript
// code here
```
```

---

## Bug Report Output Rules

- Use the exact format from `templates/bug-report-template.md`.
- Every section must be present — use "N/A" for any section without data.
- Title format: `[<Module>] <Specific descriptive summary>`.
- Steps to reproduce must be numbered and exact.
- Evidence section must always be present, even if all entries are "N/A".

---

## Triage Output Rules

- Always output a Markdown table with columns: #, Test Case ID, Status, Root Cause, Failure Summary, Suggested Action.
- Follow with High-Level Analysis (counts and percentages).
- Follow with Top Issues (ranked by frequency).
- Follow with Recommendations (1–3 actionable items).
- Do not dump raw logs — only short error snippets.

---

## Daily Update Output Rules

- Use the exact format from `templates/daily-update-template.md`.
- All metrics tables must be filled with numbers, not left blank.
- Use "None" for empty bug or retest tables, not an empty table.
- Plan for next day must be a checklist with `- [ ]` items.

---

## Numbering and Sequencing

- Test Case IDs: Always increment sequentially (001, 002, 003...).
- Bug IDs: Always increment sequentially.
- Step numbers: Always start at 1 and increment by 1.
- Do not skip numbers or reuse IDs within the same output.

---

## Language and Tone

- Professional and factual.
- No informal language, slang, or emojis.
- No first-person ("I think...", "I would suggest...").
- No hedging ("might", "perhaps", "could possibly").
- Direct and assertive: "The error message is displayed" not "The error message should probably be displayed."

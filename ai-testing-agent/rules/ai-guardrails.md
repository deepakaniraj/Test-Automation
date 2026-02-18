# AI Guardrails — Risks, Limitations & What NOT to Do

> These rules define mandatory guardrails for using AI in testing workflows.
> Every skill, prompt, and review must comply with these constraints.
> Violating these guardrails can lead to false confidence, compliance breaches, or quality degradation.

---

## Core Principle

> **AI is an assistant, not a replacement.** Every AI output must be reviewed by a human tester before it is treated as final. Testers remain accountable for quality — AI is a tool they supervise.

---

## 1. Never Blindly Trust AI Output

| Rule | Detail |
|------|--------|
| Always review AI-generated test cases | AI can hallucinate steps, invent requirements, or miss edge cases. Every generated test case must be read, validated against requirements, and approved by a human. |
| Always review AI-generated code | AI may produce syntactically correct but logically wrong scripts. Run them, inspect assertions, and verify they actually test what they claim. |
| Always review AI-generated bug reports | AI may misclassify severity, invent steps, or miss context. A tester must validate every bug report before submission. |
| Never skip peer review because AI wrote it | AI-generated artifacts need the **same** (or stricter) peer review as human-written ones. |

**What NOT to do:**
- Do NOT submit AI-generated test cases to stakeholders without reviewing them.
- Do NOT merge AI-generated automation scripts without running them locally first.
- Do NOT file AI-generated bug reports into Jira without a tester reading and approving them.
- Do NOT assume AI coverage is complete — it is a starting point, not a final product.

---

## 2. Never Over-Rely on AI

| Rule | Detail |
|------|--------|
| Continue hands-on exploratory testing | AI cannot replace human intuition for usability, UX, and domain-specific edge cases. |
| Maintain manual testing skills | Teams overly dependent on AI lose the ability to spot issues AI misses. |
| Don't skip critical thinking | Just because an AI tool didn't flag a defect doesn't mean the product is bug-free. |
| Use AI to augment, not replace | AI handles shallow/repetitive tasks; humans handle deep/creative/contextual tasks. |

**What NOT to do:**
- Do NOT stop exploratory testing because "AI has it covered."
- Do NOT assume 100% AI-generated test coverage means zero bugs.
- Do NOT let AI make risk decisions — humans assess business impact.
- Do NOT retire manual regression without validating AI regression is equivalent.

---

## 3. Protect Data Privacy and Compliance

| Rule | Detail |
|------|--------|
| Never send PII to cloud AI services | Do not paste customer data, real credentials, medical records, or financial data into ChatGPT, Copilot, or any external LLM. |
| Use anonymized or synthetic data | When AI needs sample data, generate synthetic data or anonymize real data first. |
| Follow GDPR/HIPAA/SOC2 | AI workflows must comply with your organization's data governance policies. |
| Never embed secrets in prompts | API keys, passwords, tokens, and internal URLs must never appear in AI prompts. |

**What NOT to do:**
- Do NOT paste production database exports into AI tools.
- Do NOT include real patient/customer names in test case prompts.
- Do NOT share proprietary source code with external AI services without authorization.
- Do NOT store AI conversation logs containing sensitive data.

---

## 4. Guard Against Model Drift and Staleness

| Rule | Detail |
|------|--------|
| Validate AI outputs against current requirements | AI models trained on old data may generate outdated or irrelevant tests. |
| Re-validate prompts when requirements change | When features change, update prompts and re-run — don't reuse stale AI output. |
| Compare AI tests to human benchmarks | Periodically compare AI-generated suites against manually created ones to detect quality drift. |
| Retrain/re-prompt regularly | Don't assume a prompt that worked 3 months ago still produces good results today. |

**What NOT to do:**
- Do NOT reuse AI-generated test cases from a previous release without re-validating them.
- Do NOT assume AI will automatically detect changed requirements.
- Do NOT skip regression of AI-generated tests when the application changes.

---

## 5. Manage AI Toolchain Complexity

| Rule | Detail |
|------|--------|
| Start small | Pilot one AI feature at a time; measure ROI before expanding. |
| Integrate with existing workflows | AI tools must fit into your CI/CD, test management, and reporting pipeline — not create parallel silos. |
| Avoid tool sprawl | Too many disparate AI tools create maintenance debt. Consolidate where possible. |
| Document AI tool decisions | Record which AI tools are used, for what purpose, and who owns them. |

**What NOT to do:**
- Do NOT adopt 5 AI tools simultaneously without a clear integration plan.
- Do NOT let AI tools operate outside your CI/CD pipeline.
- Do NOT use AI tools that lack audit trails or reproducible outputs.
- Do NOT ignore the cost (API tokens, compute) of running AI tools at scale.

---

## 6. Maintain Human-in-the-Loop

| Rule | Detail |
|------|--------|
| Tester reviews every AI output | No AI artifact goes to stakeholders without human review. |
| Tester corrects AI mistakes | When AI produces wrong output, correct it and feed corrections back to improve future prompts. |
| Tester makes final decisions | AI suggests; humans decide. Priority, severity, risk, and release readiness are human judgments. |
| Continuous feedback loop | Track AI accuracy over time. If quality drops, adjust prompts, switch tools, or add manual steps. |

**What NOT to do:**
- Do NOT auto-publish AI-generated reports without review.
- Do NOT let AI auto-close bugs without tester verification.
- Do NOT remove human sign-off from test cycles because "AI is reliable."

---

## 7. Ethical and Responsible AI Use

| Rule | Detail |
|------|--------|
| No AI for deceptive testing | Do not use AI to fake test results, inflate coverage numbers, or fabricate evidence. |
| Acknowledge AI-generated content | If stakeholders ask, disclose that AI was used to assist in test creation or reporting. |
| Respect intellectual property | Do not feed copyrighted code, proprietary frameworks, or licensed content into AI tools without authorization. |
| Bias awareness | AI may have biases in data generation (e.g., name, locale, gender). Review synthetic test data for unintended bias. |

---

## Checklist: Before Submitting Any AI Output

Use this checklist every time AI generates a deliverable:

- [ ] I have read every test case / bug report / script the AI produced.
- [ ] I have verified it against the actual requirements / user story.
- [ ] I have checked that no sensitive data (PII, secrets, internal URLs) was used in the AI prompt.
- [ ] I have run AI-generated code locally and confirmed it executes correctly.
- [ ] I have applied peer review (same standard as human-written work).
- [ ] I have updated the "Automation Status" or "Source" field to indicate AI-assisted generation.
- [ ] I have checked for hallucinated requirements or invented edge cases.

---

## References

- *"Generative AI improves coverage while reducing manual effort, but reliability, governance, and human oversight are essential to prevent hallucinated or irrelevant AI-generated tests."* — testRigor
- *"Teams overly dependent on AI may lose the ability to spot edge cases AI misses."* — WebbyCrown AI Testing Guide
- *"We as test engineers will be valuable in the AI era for the ability to validate and judge AI tool output."* — Ministry of Testing Community
- *"Proper data anonymization and synthetic data generation"* are required to avoid compliance breaches. — WebbyCrown

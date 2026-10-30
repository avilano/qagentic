---
name: playwright-code-review
description: Comprehensive static code review for Playwright E2E test suites on local Git diffs or GitHub Pull Requests. Audits Page Object architecture, locator hygiene, anti-flakiness, test isolation, and coverage parity. Activate on "playwright review", "review e2e", "audit playwright test", "e2e pr review", or "check playwright changes". Do NOT use for unit test reviews or browser runtime execution.
metadata:
  author: Alberto Ettienne (@avilano)
  owner: QA Engineering
  version: "1.0.0"
  category: Test Automation
allowed-tools: >-
  Read Grep Glob Write AskUserQuestion
  Bash(git diff:*) Bash(git log:*) Bash(git branch:*) Bash(git show:*) Bash(git ls-files:*) Bash(git status:*)
  mcp__user-github__pull_request_read
---

# Playwright Code Review (E2E Static Quality Auditor)

Performs automated, rigorous static code reviews of Playwright end-to-end test changes. Evaluates test architecture, Page Object Model (POM) encapsulation, locator resilience, test isolation, and flaky-test anti-patterns against strict industry best practices.

---

## 1. Operating Targets

The review operates in two contexts:
1. **Local Working Tree Diff (Default):** Evaluates unstaged, staged, and untracked Playwright files against `<base>` (default: `origin/main` or `origin/dev`).
2. **GitHub Pull Request:** Evaluates PR diffs via GitHub MCP / Git fetch for a given `owner/repo#PR`.

---

## 2. When to Use & When NOT to Use

- **Activate for:**
  - Auditing Playwright test PRs or local branches before merging.
  - Reviewing Page Object Model & Component Object Model designs.
  - Catching flaky patterns (hardcoded sleeps, page-wide text locators, unisolated test state).
  - Checking test coverage parity when refactoring or migrating tests.
- **Do NOT use for:**
  - Running/executing Playwright tests &rarr; use native test runner commands (`npx playwright test`).
  - Unit or API-only testing frameworks (Jest, Vitest, Mocha) unless configured for Playwright.
  - Scoping requirements or user stories &rarr; use [`qa-epic-scoping`](../qa-epic-scoping/SKILL.md).

---

## 3. 5-Step Procedural Workflow

```
[1. Gather Changeset] ──> [2. Discover Specs & POMs] ──> [3. Audit Quality Gates] ──> [4. Classify Severity] ──> [5. Generate Report]
```

### Step 1: Gather the Changeset
1. **Local Diff:**
   - Detect E2E root directory (e.g. `tests/e2e/`, `tests/playwright/`, `e2e/`, or project root).
   - Tracked modifications: `git diff <base> -- <e2e_path>`
   - Staged new/modified files: `git diff <base> --cached -- <e2e_path>`
   - **Untracked new specs (Mandatory):** `git ls-files --others --exclude-standard -- <e2e_path>`
   - Read full contents of any newly added files from disk.
2. **GitHub PR:**
   - Fetch PR details, changed files list, diff, and the **PR Head SHA**.
   - Note commit SHA for generating permanent GitHub blob links.

### Step 2: Categorize Files
Identify role of each changed file:
- **Test Specs:** `*.spec.ts`, `*.test.ts`, `*.e2e.ts`
- **Page Objects:** `*.page.ts` (representing distinct web pages or routes)
- **Component Objects:** `*.component.ts` (representing reusable widgets, modals, tables, drawers)
- **Fixtures & Utilities:** `*.fixture.ts`, setup/seed helpers, database mocks

### Step 3: Audit Quality Gates
Evaluate the changeset against core quality rules:
1. **Architecture & Encapsulation:** Load [references/POM-AND-COMPONENTS.md](references/POM-AND-COMPONENTS.md).
   - Zero inline selectors in test specs.
   - Zero assertions inside Page Objects or Component Objects (assertions belong in specs).
   - Modal and drawer UI encapsulated in concrete Component Objects, not inlined on the Page.
2. **Locator Resilience & Anti-Flakiness:** Load [references/REVIEW-RUBRIC.md](references/REVIEW-RUBRIC.md).
   - Ban `page.waitForTimeout()` (hardcoded sleeps).
   - Ban unscoped `page.getByText()` at root level; enforce `getByRole()`, `getByLabel()`, or container scoping.
   - Readonly locator properties for static selectors; locator getters for parameterized selectors.
3. **Test Isolation & State Independence:** Load [references/TEST-ISOLATION-GUIDE.md](references/TEST-ISOLATION-GUIDE.md).
   - No shared mutable state between tests without reseeding.
   - Prohibit `test.describe.configure({ mode: 'serial' })` used as a workaround for shared state bugs.
   - Flat describe hierarchies (avoid deeply nested `test.describe` blocks).
4. **Coverage & Traceability:**
   - Preserves test case management IDs (Qase, TestRail, Zephyr, or ticket tags) during refactors.
   - Maintains visual and accessibility checkpoints (`axe.check()`).

### Step 4: Classify Severities & Extract Exact Locations
Assign findings to three severity levels:
- **Blocker:** Violates non-negotiable architectural rules; breaks CI, causes guaranteed flakes, leaks locators into specs, or drops test coverage silently.
- **Major:** High flakiness risk, poor isolation, missing assertions, anti-pattern locator reuse, or unhandled asynchronous race conditions.
- **Minor:** Polish, naming clarity, redundant comments, or formatting nitpicks.

**Strict Location Rule:**
Every finding must cite **all exact file and line occurrences** with clickable links:
- Local Diff: `[path/file.ts:42](../path/file.ts#L42)` (relative to output report).
- GitHub PR: `[path/file.ts:42](https://github.com/<owner>/<repo>/blob/<sha>/path/file.ts#L42)`

### Step 5: Generate the Code Review Report
1. Load [references/REPORT-TEMPLATE.md](references/REPORT-TEMPLATE.md).
2. Synthesize findings, code snippets showing before/after fixes, and statistical summaries.
3. Save report to `playwright-code-reviews/review-<branch-or-pr>-<timestamp>.md`.
4. Output concise summary in chat with link to report.

---

## 4. Safety & Review Principles

- **Static Review Only:** Do not trigger test runs or modify working tree files during review.
- **No Hallucinated Line Numbers:** Always verify line numbers against actual disk or diff contents using ripgrep or file reads.
- **Constructive Guidance:** Every finding must provide an actionable code snippet demonstrating the correct idiomatic Playwright pattern.

---

## 5. Lazy-Loaded References

Load only during respective review phases:
- Detailed review rubric & severity rules: [references/REVIEW-RUBRIC.md](references/REVIEW-RUBRIC.md)
- Page Object & Component guidelines: [references/POM-AND-COMPONENTS.md](references/POM-AND-COMPONENTS.md)
- Test isolation & flakiness prevention: [references/TEST-ISOLATION-GUIDE.md](references/TEST-ISOLATION-GUIDE.md)
- Report format & markdown template: [references/REPORT-TEMPLATE.md](references/REPORT-TEMPLATE.md)

---
name: qa-regression-retrospective
description: Conduct release regression retrospectives, defect Root Cause Analysis (RCA), and test coverage gap audits. Identifies defect clusters, evaluates whether fix PRs added regression-preventing automated tests, and generates actionable QA follow-up tasks. Activate on "regression retrospective", "post-release QA retro", "defect RCA", "coverage gap analysis", or "bug retrospective". Do NOT use for real-time bug triage (use review-bug) or single-spec code reviews (use playwright-code-review).
metadata:
  author: Alberto Ettienne (@avilano)
  owner: QA Engineering
  version: "1.0.0"
  category: Quality Intelligence
allowed-tools: >-
  Read Grep Glob Write AskUserQuestion
  Bash(git log:*) Bash(git diff:*) Bash(git show:*)
  mcp__plugin-atlassian-atlassian__getAccessibleAtlassianResources mcp__plugin-atlassian-atlassian__searchJiraIssuesUsingJql mcp__plugin-atlassian-atlassian__getJiraIssue mcp__plugin-atlassian-atlassian__createJiraIssue
  mcp__user-github__list_issues mcp__user-github__get_issue mcp__user-github__pull_request_read
  mcp__plugin-notion-workspace-notion__search mcp__plugin-notion-workspace-notion__get_page
---

# QA Regression Retrospective (Defect RCA & Coverage Gap Auditor)

End-to-end framework for analyzing escaped defects and regressions across recent releases. Identifies defect root causes, audits whether fix Pull Requests included regression-preventing tests, pinpoints automated test coverage gaps, and generates concrete QA action items.

---

## 1. Operating Modes

| Mode | Default | Trigger Phrases | Behavior |
| :--- | :--- | :--- | :--- |
| **Dry-Run (Analysis Only)** | **Yes** | "run regression retrospective", "defect RCA for last release", "coverage gap analysis" | Queries defect tracker, analyzes fix PRs and git history, classifies root causes, and emits comprehensive markdown report. **Zero writes to external tools.** |
| **Sync / Action Mode** | **No** | "create QA follow-up tasks", "sync retrospective findings", "generate Jira QA tasks" | Generates retro report, drafts concrete QA test automation tasks (e.g. Playwright test specs), presents preview, awaits explicit user approval, then creates tracker items. |

---

## 2. When to Use & When NOT to Use

- **Activate for:**
  - Post-release or monthly defect retrospectives to discover why bugs escaped to production.
  - Auditing whether bug-fix PRs included tests that actually prevent regression recurrences.
  - Identifying recurring defect hot spots (modules with disproportionate bug counts).
  - Formulating targeted QA automation backlogs based on empirical defect history.
- **Do NOT use for:**
  - Triaging a single incoming bug in real time &rarr; use bug triage / RCA skills.
  - Reviewing PR test changes before merge &rarr; use [`playwright-code-review`](../playwright-code-review/SKILL.md).
  - Writing test plans for upcoming features &rarr; use [`qa-test-plan-generator`](../qa-test-plan-generator/SKILL.md).

---

## 3. 5-Step Procedural Workflow

```
[1. Discover Defects] ──> [2. Root Cause Classification] ──> [3. Fix PR & Test Audit] ──> [4. Hotspot Clustering] ──> [5. Report & Action Tasks]
```

### Step 1: Discover Escaped Defects
1. Define discovery scope: Lookback window (e.g. `last 30 days`), release version/tag (e.g. `v3.11.0`), or tracker query (e.g. `type = Bug AND label in (regression, escaped-bug)`).
2. Query tracker (Jira, GitHub Issues, Linear, or CSV/Markdown defect list).
3. Filter out duplicate, rejected, or non-reproducible tickets.

### Step 2: Root Cause Analysis (RCA) Classification
Load [references/RCA-TAXONOMY.md](references/RCA-TAXONOMY.md):
Categorize each defect into a primary root cause:
- **Missing Specification / Ambiguity:** AC omitted the scenario or gave conflicting directions.
- **Implementation Logic / Edge Case:** Code bug, unhandled null/undefined, boundary off-by-one.
- **Integration / API Contract Drift:** Frontend/Backend schema mismatch, breaking third-party API update.
- **Concurrency & State Race:** Async timing failure, stale React state, uncoordinated database transactions.
- **Environment / Config Discrepancy:** Feature flag misconfiguration, missing seed data, infra timeout.

### Step 3: Fix PR & Test Coverage Audit
Load [references/FIX-AUDIT-GUIDE.md](references/FIX-AUDIT-GUIDE.md):
1. For each resolved defect, locate the merging fix PR or commit across relevant repositories.
2. Evaluate test coverage in the fix PR:
   - **Full Prevention (Yes):** Fix includes an automated E2E or integration test that reproduces the defect failure and asserts the fix.
   - **Partial Prevention (Partial):** Fix includes a unit test for a single function, but the end-to-end integration path remains unguarded.
   - **Coverage Gap (No):** Fix contains zero tests, or tests only happy paths without exercising the defect condition.

### Step 4: Hotspot Clustering & Gap Detection
1. Group defects by component / functional domain to identify recurring quality hotspots.
2. Calculate coverage gap statistics:
   - Total regressions analyzed.
   - Percentage of defects with automated test guards added in fix PRs.
   - Highest risk regression hot spots.

### Step 5: Deliver Retrospective Report & Action Tasks
1. Load [references/RETRO-REPORT-TEMPLATE.md](references/RETRO-REPORT-TEMPLATE.md).
2. Save deliverable to `retrospectives/QA-RETROSPECTIVE-{TIMEFRAME}.md`.
3. If Sync / Action Mode is commanded:
   - Load [references/TRACKER-SYNC-ADAPTERS.md](references/TRACKER-SYNC-ADAPTERS.md).
   - Generate stub tasks for high-priority missing automated tests.
   - Require explicit confirmation before creating tracker items.

---

## 4. Safety Gates & Verification Rules

- **Zero-Write Default:** Retrospective analysis is 100% read-only unless the user explicitly commands action task generation.
- **Ground on Real Git Diffs:** Never guess whether tests were added to a fix PR; inspect the actual git diff or PR files.
- **Test File Changed ≠ Regression Prevented:** Scrutinize the test logic. A changed test file that only updates a snapshot or mocks the broken behavior does not constitute a valid regression guard.

---

## 5. Lazy-Loaded References

Load only during their respective phases:
- Defect root cause classification taxonomy: [references/RCA-TAXONOMY.md](references/RCA-TAXONOMY.md)
- Fix PR test evaluation guide: [references/FIX-AUDIT-GUIDE.md](references/FIX-AUDIT-GUIDE.md)
- Retrospective deliverable report template: [references/RETRO-REPORT-TEMPLATE.md](references/RETRO-REPORT-TEMPLATE.md)
- Tracker synchronization adapters: [references/TRACKER-SYNC-ADAPTERS.md](references/TRACKER-SYNC-ADAPTERS.md)

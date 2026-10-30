---
name: qa-test-plan-generator
description: Generate comprehensive QA Test Plans and test scenario matrices from user stories, epics, PRDs, or bug specifications. Formulates positive paths, negative edge cases, boundary checks, permission matrices, and regression risk maps with pluggable TMS export. Activate on "create test plan", "test strategy", "plan QA coverage", "generate test scenarios", or "QA test matrix". Do NOT use for code diff reviews (use playwright-code-review) or ticket readiness audits (use qa-ticket-readiness).
metadata:
  author: Alberto Ettienne (@avilano)
  owner: QA Engineering
  version: "1.0.0"
  category: Test Strategy
allowed-tools: >-
  Read Grep Glob Write AskUserQuestion
  mcp__plugin-atlassian-atlassian__getAccessibleAtlassianResources mcp__plugin-atlassian-atlassian__getJiraIssue mcp__plugin-atlassian-atlassian__addCommentToJiraIssue
  mcp__user-github__get_issue mcp__user-github__add_issue_comment
---

# QA Test Plan Generator (Test Strategy & Scenario Architect)

Transforms requirements, Epics, User Stories, PRDs, or bug reports into structured, industry-standard **QA Test Plans**. Automatically categorizes coverage across positive flows, negative edge cases, boundary thresholds, role-based permission matrices, and regression risks.

---

## 1. Operating Modes

| Mode | Default | Trigger Phrases | Behavior |
| :--- | :--- | :--- | :--- |
| **Dry-Run (Markdown Only)** | **Yes** | "create test plan for [KEY]", "test strategy", "plan QA coverage", "draft test matrix" | Reads requirements, synthesizes full Test Plan deliverable into `QA-TEST-PLAN-{KEY}.md`. **Zero writes to external tools.** |
| **TMS Export Mode** | **No** | "sync test plan to [TMS]", "export to Qase/TestRail/Jira" | Generates test plan, presents preview, awaits explicit confirmation, and formats payload for TMS import or comments on source ticket. |

---

## 2. When to Use & When NOT to Use

- **Activate for:**
  - Designing full QA test strategies for Epics, features, or complex bug fixes.
  - Generating test case inventories (E2E automation candidates vs exploratory manual tests).
  - Mapping regression blast radius and risk matrices before release testing.
- **Do NOT use for:**
  - Initial Epic requirement gathering and ambiguity discovery &rarr; use [`qa-epic-scoping`](../qa-epic-scoping/SKILL.md).
  - Checking ticket Definition of Ready &rarr; use [`qa-ticket-readiness`](../qa-ticket-readiness/SKILL.md).
  - Auditing Playwright test code &rarr; use [`playwright-code-review`](../playwright-code-review/SKILL.md).

---

## 3. 5-Step Procedural Workflow

```
[1. Ingest Requirements] ──> [2. Scope & Readiness] ──> [3. Scenario Breakdown] ──> [4. Blast Radius Analysis] ──> [5. Deliver Plan]
```

### Step 1: Ingest Requirements & Source Context
1. Detect source input: Jira Epic/Story key, GitHub Issue/PRD URL, Linear key, or local markdown specification.
2. If an Epic Brief exists (`EPIC-QA-BRIEF-{KEY}.md`), ingest it as the verified ground truth.
3. Extract: User personas, business objectives, feature flags, target environments, and verbatim Acceptance Criteria.

### Step 2: Scope & Readiness Gate
1. Validate ticket readiness: If the ticket has no acceptance criteria, run a lightweight audit via [`qa-ticket-readiness`](../qa-ticket-readiness/SKILL.md) to inherit parent context.
2. Define explicit boundaries:
   - **In-Scope:** Verified capabilities delivered in this sprint/release.
   - **Out-of-Scope:** Deferred features, separate microservices, third-party vendor downtime scenarios.

### Step 3: Comprehensive Scenario Breakdown
Load [references/SCENARIO-BREAKDOWN-GUIDE.md](references/SCENARIO-BREAKDOWN-GUIDE.md). Deconstruct requirements into structured test tiers:
1. **Tier 1: Happy Paths & Core E2E Workflows** (Primary business value paths; top candidates for Playwright automation).
2. **Tier 2: Negative Paths & Error Handling** (Invalid inputs, network failures, expired tokens, duplicate submissions).
3. **Tier 3: Boundary & Threshold Limits** (Min/max field lengths, zero states, pagination bounds, file size limits).
4. **Tier 4: Permission & Multi-Role Matrices** (Admin vs Standard User vs Read-only; cross-tenant isolation).
5. **Tier 5: UX, Responsiveness & Accessibility** (Mobile viewport checks, keyboard tab navigation, `axe-core` accessibility checks).

### Step 4: Regression Blast Radius & Test Deduplication
Load [references/REGRESSION-MAPPING.md](references/REGRESSION-MAPPING.md):
1. Identify adjacent modules, shared database collections, or common API endpoints affected.
2. Review existing test inventory to prevent duplicate tests; flag reusable existing test cases vs net-new tests needed.

### Step 5: Deliver Test Plan Document
1. Load [references/TEST-PLAN-TEMPLATE.md](references/TEST-PLAN-TEMPLATE.md).
2. Save full test plan to local artifact: `test-plans/QA-TEST-PLAN-{KEY}.md`.
3. If TMS Export is requested, load [references/TMS-ADAPTERS.md](references/TMS-ADAPTERS.md), preview payload, and await user confirmation before posting comment or syncing.

---

## 4. Safety & Quality Rules

- **Zero External Mutations by Default:** Never post comments or create remote test cases without explicit user consent.
- **Traceability:** Every test scenario must link back to its originating Acceptance Criterion (e.g. `[AC-1]`, `[AC-2]`).
- **Deterministic Test Steps:** Every test scenario in the plan must include unambiguous preconditions, execution steps, and expected results.

---

## 5. Lazy-Loaded References

Load only during relevant execution phases:
- Test plan document schema: [references/TEST-PLAN-TEMPLATE.md](references/TEST-PLAN-TEMPLATE.md)
- Scenario deconstruction methodology: [references/SCENARIO-BREAKDOWN-GUIDE.md](references/SCENARIO-BREAKDOWN-GUIDE.md)
- TMS export adapters (Qase, TestRail, Zephyr): [references/TMS-ADAPTERS.md](references/TMS-ADAPTERS.md)
- Regression & blast radius analysis: [references/REGRESSION-MAPPING.md](references/REGRESSION-MAPPING.md)

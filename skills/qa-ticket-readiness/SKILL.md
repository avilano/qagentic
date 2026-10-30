---
name: qa-ticket-readiness
description: Audit Definition of Ready (DoR) and acceptance criteria completeness for User Stories, Bug Tickets, and QA Tasks before implementation or testing handoff. Activate on "ticket readiness", "check DoR", "validate AC", "QA readiness", or "audit ticket". Do NOT use for high-level epic scoping (use qa-epic-scoping) or test scenario breakdown (use qa-test-plan-generator).
metadata:
  author: Alberto Ettienne (@avilano)
  owner: QA Engineering
  version: "1.0.0"
  category: Quality Gates
allowed-tools: >-
  Read Grep Glob AskUserQuestion
  mcp__plugin-atlassian-atlassian__getAccessibleAtlassianResources mcp__plugin-atlassian-atlassian__getJiraIssue mcp__plugin-atlassian-atlassian__editJiraIssue
  mcp__user-github__get_issue mcp__user-github__edit_issue
---

# QA Ticket Readiness (Definition of Ready Auditor)

Validates that an engineering ticket, user story, or QA task contains sufficient, unambiguous information to satisfy the **Definition of Ready (DoR)** before design, implementation, or test planning begins.

---

## 1. Operating Modes

| Mode | Default | Trigger Phrases | Behavior |
| :--- | :--- | :--- | :--- |
| **Dry-Run** | **Yes** | "QA readiness for [ID]", "Check DoR for [ID]", "Validate AC for [ID]", "Audit ticket" | Read ticket, validate against checklist, produce letter grade (A/B/C/F) and gap report. **Zero writes.** |
| **Write / Fix** | **No** | "Fix ticket for [ID]", "Populate AC for [ID]", "write mode", "Update ticket with readiness fixes" | Validate, synthesize missing AC from parent epic/PRD, **preview exact before/after diff**, require explicit user approval, then update the ticket. |

---

## 2. When to Use & When NOT to Use

- **Activate for:**
  - Auditing ticket completeness before sprint kickoff or QA handoff.
  - Verifying Acceptance Criteria (AC), testability, environment requirements, and edge case coverage.
  - Enriching thin/empty QA child tasks with inherited context from their parent Epic or PRD.
- **Do NOT use for:**
  - High-level epic discovery or multi-story scoping &rarr; use [`qa-epic-scoping`](../qa-epic-scoping/SKILL.md).
  - Generating test plans and scenario matrices &rarr; use [`qa-test-plan-generator`](../qa-test-plan-generator/SKILL.md).
  - Auditing E2E code pull requests &rarr; use [`playwright-code-review`](../playwright-code-review/SKILL.md).

---

## 3. 5-Step Procedural Workflow

```
[1. Fetch Ticket] ──> [2. Classify Type] ──> [3. Context Inheritance] ──> [4. Grade DoR] ──> [5. Report / Fix]
```

### Step 1: Fetch Ticket
1. Detect issue tracker provider (Jira, GitHub Issues, Linear, or local Markdown file).
2. Fetch full issue details:
   - Summary, description, issuetype, parent/epic link.
   - Custom fields (acceptance criteria, feature flags, components, affected environments).
   - Comments and activity history for recent refinements.

### Step 2: Classify Ticket Type
Load [references/TASK-CLASSIFICATION.md](references/TASK-CLASSIFICATION.md):
- **User Story / Feature:** Evaluates user persona, business problem, functional AC, edge cases.
- **Bug Report:** Evaluates reproduction steps, expected vs actual behavior, environment, error logs.
- **QA Test Plan / Automation Task:** Evaluates target scope, test environment, prerequisite test data, linked story pointers.
- **Parent Epic / Initiative:** Redirect to [`qa-epic-scoping`](../qa-epic-scoping/SKILL.md) or list child stories for individual review.

### Step 3: Context Inheritance (For Thin or Stub Tickets)
If the ticket description is thin (<50 characters or boilerplate only):
1. Resolve the parent Epic, Milestone, or PRD via the parent link or tracker hierarchy.
2. If an Epic Brief exists (e.g. `EPIC-QA-BRIEF-{KEY}.md`), parse it for scope and AC.
3. Inherit relevant context: summary, user problem, feature flags, components, and acceptance boundaries.
4. Mark inherited fields explicitly as `[Inherited from {PARENT_KEY}]` in the report.

### Step 4: Audit & Grade against Definition of Ready
Load [references/READINESS-CHECKLIST.md](references/READINESS-CHECKLIST.md) and evaluate:
- **Tier 1 (Core DoR - Must Have):**
  - Clear user story or problem statement (`As a... I want... So that...` or defect behavior).
  - Verifiable, testable Acceptance Criteria (Given/When/Then or crisp checklist).
  - Scope boundaries (explicit In-Scope and Out-of-Scope).
- **Tier 2 (Quality & Automation Readiness):**
  - Negative scenarios and error handling documented.
  - Environment, test data, and user roles specified.
  - Feature flags, migration dependencies, or configuration keys identified.

#### Grading Matrix:
- **Grade A (Ready for Development/QA):** All Tier 1 & Tier 2 pass. High execution confidence.
- **Grade B (Proceed with Minor Gaps):** All Tier 1 pass; minor non-blocking Tier 2 items missing.
- **Grade C (Blocked - Incomplete):** Critical Tier 1 items missing. Needs refinement before work begins.
- **Grade F (Unusable Stub):** Missing problem statement and AC; cannot proceed without major re-scoping.

### Step 5: Generate Report or Execute Write Mode
- **Dry-Run (Default):** Render the readiness evaluation per [references/REPORT-TEMPLATE.md](references/REPORT-TEMPLATE.md).
- **Write Mode:**
  1. Load [references/TICKET-TEMPLATES.md](references/TICKET-TEMPLATES.md) and [references/WRITE-MODE.md](references/WRITE-MODE.md).
  2. Synthesize proposed ticket description and AC updates.
  3. Show the **exact unified diff** in chat.
  4. Prompt user for explicit approval (`"Update ticket now?"`).
  5. Upon confirmation, execute ticket update via tracker tool. Re-audit to verify improved grade.

---

## 4. Safety Gates & Verification Rules

- **Zero-Write Default:** Never mutate remote ticket state unless user explicitly provided write triggers (`fix`, `populate`, `update`).
- **Diff Preview Mandatory:** Never execute remote write operations without presenting the full before/after diff to the user first.
- **Immutable Fields:** Never alter ticket status, assignee, reporter, sprint, or story points.
- **Graceful Error Handling:** If API authentication or field permission fails, halt and prompt the user. Never loop or blindly probe alternate fields.

---

## 5. Lazy-Loaded References

Load only during their respective phases:
- Classification rules: [references/TASK-CLASSIFICATION.md](references/TASK-CLASSIFICATION.md)
- Audit checklist & criteria: [references/READINESS-CHECKLIST.md](references/READINESS-CHECKLIST.md)
- Report format: [references/REPORT-TEMPLATE.md](references/REPORT-TEMPLATE.md)
- Markdown templates for DoR: [references/TICKET-TEMPLATES.md](references/TICKET-TEMPLATES.md)
- Write mode safety protocol: [references/WRITE-MODE.md](references/WRITE-MODE.md)

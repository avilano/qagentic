# Task Classification and Routing Rules

Classifies incoming tickets into one of four primary archetypes to apply the correct Definition of Ready criteria.

---

## 1. Issue Types

### Type A: User Story / Feature Task
- **Indicators:** Issue type is `Story`, `Feature`, `Enhancement`, or summary indicates new capability / behavior change.
- **Key DoR Elements:** User role, objective, business value, positive acceptance criteria, negative boundary expectations, UI mockups (if visual).
- **Target Audience:** Feature developers, QA test designers.

### Type B: Defect / Bug Report
- **Indicators:** Issue type is `Bug`, `Defect`, or summary indicates unexpected behavior / failure.
- **Key DoR Elements:** Steps to reproduce (deterministic), expected behavior, actual behavior, error messages/logs, affected environment/version, customer or severity impact.
- **Target Audience:** Triaging engineers, QA automation reproducers.

### Type C: Dedicated QA Task (Test Plan or E2E Automation)
- **Indicators:** Summary matches `/test\s*plan/i`, `/e2e\s*(test|testing)/i`, `/playwright/i`, or issue type is `QA Task` / `Test Execution`.
- **Key DoR Elements:** Target feature/story links, environment readiness, test data prerequisites, automated vs manual scope.
- **Target Audience:** QA automation engineers.

### Type D: Epic / Feature Container
- **Indicators:** Issue type is `Epic`, `Initiative`, `Theme`, or contains a collection of child issues.
- **Routing Decision:** **REDIRECT**.
  - Epics should not be evaluated as single tickets.
  - Inform the user: *"This is a parent Epic. Recommend using `qa-epic-scoping` for comprehensive scoping, or select one of its child stories below for DoR auditing."*
  - List the active child stories with summary and key for user selection.

---

## 2. Platform Detection Rules

| Tracker | Detection Logic | Action |
| :--- | :--- | :--- |
| **Jira** | Key matches `[A-Z0-9]+-[0-9]+` (e.g. `PROJ-1234`) and Jira MCP tools available | Call `getJiraIssue` with expanded fields |
| **GitHub Issues** | String matches `#123` or `owner/repo#123` or URL `github.com/.../issues/...` | Call `get_issue` |
| **Linear** | Key matches `[A-Z]{2,5}-[0-9]+` and Linear CLI / MCP available | Query Linear issue |
| **Local Markdown** | File path ending in `.md` exists in workspace | Read file directly |

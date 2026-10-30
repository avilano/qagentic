# Retrospective Tracker Synchronization Adapters

Rules and templates for syncing retrospective findings and creating follow-up QA automation tasks.

---

## 1. Supported Sync Destinations

### Destination A: Jira QA Automation Task
When generating a follow-up task in Jira:
- **Project:** Same project as the original bug, or designated QA team project.
- **Issue Type:** `Task` or `QA Task`.
- **Summary:** `[QA Auto] Regression Test: {Bug Summary} ({BUG_KEY})`
- **Description:**
  ```markdown
  h3. Objective
  Automate regression coverage to prevent recurrence of *{BUG_KEY}* ({Bug Summary}).

  h3. Bug Root Cause
  * RCA Category: {Category}
  * Escape Point: {Escape Phase}
  * Fix PR: {PR Link}

  h3. Required Automated Test Specification
  * Test Level: Playwright E2E
  * Proposed Path: {{tests/e2e/{domain}/{spec_name}.spec.ts}}
  * Scenario Steps:
  # {Step 1}
  # {Step 2}
  # Assert {Expected Behavior}
  ```

### Destination B: GitHub Issues
- **Title:** `[QA Auto] Regression Test: {Bug Summary} (#{BUG_NUMBER})`
- **Labels:** `qa-automation`, `regression-guard`, `test-coverage-gap`
- **Body:** Markdown task per template above with `- [ ]` checklist items.

### Destination C: Notion Bug Findings Database
- Map bug properties to standard Notion columns:
  - `Bug Key` (Title)
  - `Summary` (Rich Text)
  - `RCA Category` (Select)
  - `Test Coverage` (Select: `YES`, `PARTIAL`, `NO`)
  - `Fix PR` (URL)
  - `Action Items` (Rich Text with QA follow-up task links)

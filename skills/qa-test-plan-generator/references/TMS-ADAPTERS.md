# Test Management System (TMS) Export Adapters

Schema adapters and mapping rules for exporting QA Test Plans to various test management tools.

---

## 1. Supported Adapters

### Adapter A: Qase.io TMS
- **Plan Title:** `[{KEY}] {Feature Name}`
- **Plan Description:** Markdown rendered per `TEST-PLAN-TEMPLATE.md`.
- **Case Mapping:**
  - `title`: Short scenario description (e.g. `[AC-1] Verify Admin can invite new user`)
  - `behavior`: `positive` | `negative` | `destructive`
  - `severity`: `blocker` | `critical` | `major` | `normal` | `minor`
  - `is_flaky`: `0`
  - `automation`: `automated` (for Playwright candidates) or `manual`
  - `suite_id`: Target functional module suite

### Adapter B: TestRail
- **Section / Suite:** Named after parent Epic or feature area.
- **Case Fields:**
  - `title`: Scenario title with `[AC-ID]` prefix.
  - `type_id`: `Automated` | `Functional` | `Regression` | `Security`
  - `custom_preconds`: Preconditions markdown.
  - `custom_steps_separated`: Step-by-step table (Step Description + Expected Result).

### Adapter C: Jira QA Comment / Confluence Page
- **Format:** Formatted GitHub-flavored markdown or Jira Wiki markup posted as a structured comment or linked confluence document under the parent issue key.
- **Safety Gate:** Previews the exact markdown block and prompts:
  `"Post QA Test Plan comment to Jira issue {KEY}? (yes/no)"`

### Adapter D: GitHub Issues & Projects
- **Issue Title:** `[QA Test Plan] {Feature Name}`
- **Issue Body:** Full test plan markdown with task checkboxes `- [ ]` for interactive tracking across QA execution phases.

# Standard QA Ticket Templates

Structured markdown templates used when enriching thin tickets or generating missing sections.

---

## 1. Feature / User Story Template

```markdown
## Problem Statement
As a **{User Persona}**,
I want **{Action / Capability}**,
So that **{Business Value / Outcome}**.

## In-Scope
- {Feature item 1}
- {Feature item 2}

## Out-of-Scope
- {Deferred feature or separate ticket reference}

## Acceptance Criteria
- [ ] **AC 1 (Happy Path):** Given {precondition}, when {action}, then {expected outcome}.
- [ ] **AC 2 (Validation):** Given {invalid input}, when {submission}, then error message "{Exact Text}" is displayed.
- [ ] **AC 3 (Permission):** Users with role `{Restricted Role}` cannot access {restricted action}.

## Technical & Test Details
- **Feature Flag:** `{flag-name}` (default: `off`)
- **Impacted Services/Routes:** `{POST /api/v1/...}`
- **Figma / Design:** [{Link}]({URL})
- **Roles to Test:** `{Admin, Standard User}`
```

---

## 2. Bug Report Template

```markdown
## Defect Summary
{Clear description of the unexpected failure.}

## Steps to Reproduce
1. Log in as `{Role}`.
2. Navigate to `{Page / Route}`.
3. Perform `{Action with specific input}`.
4. Observe the failure.

## Expected Behavior
{What should have occurred according to requirements.}

## Actual Behavior
{What actually occurred, including error toasts, stack traces, or console errors.}

## Environment & Data
- **Environment:** `{Staging / Dev / Production}`
- **Browser / OS:** `{Chrome 120 / macOS}`
- **Test Entity / ID:** `{e.g. Org ID or User ID}`
- **Logs / Screenshot:** [{Link or attached excerpt}]

## Suspected Regression
- **Last Known Working Release:** `{v2.4.0}`
- **Suspected Commit / PR:** `{PR #...}`
```

---

## 3. Dedicated QA E2E / Test Task Template

```markdown
## QA Task Objective
Automate or verify test coverage for **[{STORY_KEY}] {Story Title}**.

## Target Coverage
- [ ] Automated Playwright E2E spec under `{tests/e2e/...}`
- [ ] Manual regression verification for edge cases
- [ ] TMS Test Case synchronization (Qase / TestRail)

## Prerequisites & Test Fixtures
- **Required Seed Data:** `{Specific fixture or seed entity}`
- **User Role:** `{Admin}`
- **Target Route:** `{/settings/...}`
```

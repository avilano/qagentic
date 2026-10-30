# Playwright Code Review Rubric & Severity Rules

Comprehensive evaluation criteria for automated Playwright E2E code reviews.

---

## 1. Blocker Severities (Merge Blockers)

Must be resolved before code can be merged into the target branch.

| Rule ID | Category | Rule & Rationale | Violation Example | Remediation |
| :--- | :--- | :--- | :--- | :--- |
| **BLK-01** | Locators | **No Inline Locators in Specs:** Test specs must interact exclusively through Page Objects and Component Objects. Direct calls to `page.locator()`, `page.getByTestId()`, `page.getByRole()`, or XPath/CSS in test specs violate encapsulation. | `await page.locator('#submit-btn').click();` | Move locator to POM: `await loginPage.submitButton.click();` |
| **BLK-02** | Architecture | **No Assertions in Page Objects:** `expect()` calls belong exclusively in the test layer (`*.spec.ts`). Page Objects provide actions, locators, and state queries; they must never make test assertions. | Inside POM: `await expect(this.modal).toBeVisible();` | In spec: `await expect(loginPage.modal.root).toBeVisible();` or use state wait in POM: `await this.modal.root.waitFor();` |
| **BLK-03** | Anti-Flake | **No Hardcoded Sleep Timers:** `page.waitForTimeout()` introduces arbitrary delays, slows CI runs, and remains flaky under high load. | `await page.waitForTimeout(5000);` | Use web assertions (`await expect(locator).toBeVisible()`) or `page.waitForResponse(...)`. |
| **BLK-04** | Coverage | **Preserve Existing Coverage:** Rewriting or migrating a test must not silently drop scenarios, edge cases, or test cases tied to tracking IDs (Qase, TestRail, Jira). | Deleting 3 assertion checks during POM refactor | Restore full verification flow or document explicit intentional change. |

---

## 2. Major Severities (High Risk / Flake Vulnerability)

High probability of causing intermittent test failures, flakiness, or maintenance debt.

| Rule ID | Category | Rule & Rationale | Violation Example | Remediation |
| :--- | :--- | :--- | :--- | :--- |
| **MAJ-01** | Locators | **Unscoped Page-Wide `getByText`:** `page.getByText('Submit')` at the root matches headings, tooltips, breadcrumbs, and toasts indiscriminately. | `await page.getByText('Save').click();` | Use `page.getByRole('button', { name: 'Save' })` or scope to container: `this.card.getByText('Save')`. |
| **MAJ-02** | Architecture | **Modularize Complex Modals/Drawers:** Modals and slide-overs must be separate Component Objects that encapsulate their specific inputs and action buttons. | Defining 15 modal buttons directly on `SettingsPage` | Extract `UserInviteModal` extending `BaseComponent` and compose inside `SettingsPage`. |
| **MAJ-03** | Isolation | **No Shared Mutable State across Tests:** Tests within a spec must not depend on database or UI state created by prior tests. | Test 2 deletes an entity created by Test 1 without re-seeding | Use unique test fixture IDs (`uuid()`) or seed clean data in `test.beforeEach()`. |
| **MAJ-04** | Isolation | **Banning Serial Mode Workarounds:** Do not use `test.describe.configure({ mode: 'serial' })` simply to hide shared state bugs. | Marking describe as serial because tests fail in parallel | Refactor tests to be self-contained and idempotent. |
| **MAJ-05** | Synchronization | **Await Network Responses for State Mutations:** When an action triggers asynchronous backend updates, wait on the response rather than racing UI rendering. | Clicking save and immediately checking table | `await Promise.all([page.waitForResponse('/api/items'), page.click('#save')])` |
| **MAJ-06** | Readability | **Flat Describe Blocks:** Avoid nesting `test.describe()` more than 2 levels deep. | 4 levels of nested describes | Keep suite flat and descriptive. |

---

## 3. Minor Severities (Polish & Maintenance)

Improvements for consistency, naming, and readability.

| Rule ID | Category | Rule & Rationale | Violation Example | Remediation |
| :--- | :--- | :--- | :--- | :--- |
| **MIN-01** | Code Hygiene | **Repetitive Assertion Loops:** Copy-pasting 10 identical `expect(field).toBeVisible()` lines. | 10 repeated lines checking field visibility | Iterate over an array: `for (const field of fields) await expect(field).toBeVisible();` |
| **MIN-02** | Comments | **Redundant Comments:** Comments that merely repeat what the code obviously does. | `// Click the button` | Remove comment or explain *why* the non-obvious step is required. |
| **MIN-03** | Ordering | **Visual vs A11y Check Order:** When running visual regression and accessibility checks at the same checkpoint, visual check runs first. | Running `axe.check()` before `eyes.check()` | Swap order: stable snapshot capture first, then DOM tree inspection. |

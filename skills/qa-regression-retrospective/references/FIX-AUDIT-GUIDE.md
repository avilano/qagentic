# Bug Fix PR and Test Coverage Audit Guide

Methodology for inspecting Git history and Pull Requests to determine if bug fixes included valid regression tests.

---

## 1. Locating the Fix Pull Request

1. Search Git commit messages for the ticket key:
   ```bash
   git log --grep="\[KEY\]" --oneline -n 10
   ```
2. Or use GitHub CLI / MCP:
   ```bash
   gh pr list --search "[KEY]" --state merged
   ```
3. Multi-repository awareness: If the architecture separates frontend, backend, or microservices, check all repositories impacted by the ticket.

---

## 2. Evaluating Regression Test Validity

Just because a fix PR contains modified test files does **not** mean it prevents regression. Evaluate against three criteria:

### Criteria 1: Does the test exercise the defect failure mode?
- **Invalid Test:** The PR modifies `billing.spec.ts` only to update a snapshot image or adjust an unrelated locator.
- **Valid Test:** The test specifically reproduces the sequence of actions that triggered the bug and asserts that the failure condition is handled correctly.

### Criteria 2: Test Level Appropriateness
- **Unit Level:** Suitable for mathematical formulas, parsing logic, and pure functions.
- **Integration Level:** Suitable for database constraints, API request/response validation, and middleware.
- **E2E / Browser Level:** Mandatory if the defect was caused by DOM rendering, client-side routing, user session state, or multi-step modal workflows.

### Verdict Classifications
- **`YES` (Fully Covered):** Dedicated regression test exists in the PR. Reverting the code fix would cause this test to fail red.
- **`PARTIAL` (Incomplete Guard):** Test covers one narrow branch or function, but the overall user workflow remains unprotected against future refactors.
- **`NO` (Coverage Gap):** No automated test was added in the PR, or tests added only test unproblematic happy paths.

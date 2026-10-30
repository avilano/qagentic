# Test Isolation and Anti-Flakiness Guide

Principles and code patterns to guarantee robust, idempotent Playwright test execution.

---

## 1. Test Isolation Principles

E2E tests that pass locally often flake in CI due to concurrency, race conditions, or shared mutable state.

### Rule 1: Zero Inter-Test State Dependencies
Every `test()` block must be capable of running independently and in any order.
- **Antipattern:** Test 1 creates an organization, Test 2 edits it, Test 3 deletes it. If Test 2 fails, Test 3 fails cascade-style.
- **Solution:**
  - Create unique isolated seed entities for each test: `const orgName = `TestOrg_${Date.now()}_${faker.string.alphanumeric(6)}`;`
  - Or reseed/reset state in `test.beforeEach()`.

### Rule 2: Do Not Abuse `mode: 'serial'`
`test.describe.configure({ mode: 'serial' })` tells Playwright that if one test fails, all subsequent tests in the file are skipped.
- **Allowed:** Only for truly immutable shared workflows (e.g. read-only inspections of a heavy static demo dataset).
- **Prohibited:** Using `serial` to patch over broken database isolation or order-dependent assertions.

---

## 2. Synchronization & Timing Discipline

### Rule 3: Ban `page.waitForTimeout()`
Hardcoded delays (`waitForTimeout(3000)`) cause two fatal problems:
1. They waste CPU and wall-clock time when the action finishes faster.
2. They flake in CI when network or rendering latency exceeds the fixed timeout.

### Rule 4: Preferred Synchronization Mechanisms
1. **Playwright Auto-Waiting:** Built into locators (`click()`, `fill()`, `check()`).
2. **Web Assertions:** `await expect(locator).toBeVisible()` automatically polls with a configurable timeout.
3. **Network Response Waiting:** When a mutation triggers an API call:
   ```typescript
   const [response] = await Promise.all([
     page.waitForResponse(res => res.url().includes('/api/v1/orders') && res.status() === 201),
     checkoutPage.placeOrderButton.click(),
   ]);
   ```
4. **State Transitions:**
   ```typescript
   await loadingSpinner.waitFor({ state: 'detached' });
   ```

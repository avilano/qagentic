# 🎭 Playwright E2E Code Review Report

**Target:** PR #342: Refactor Billing and Invoice Download Flows  
**Reference / Commit:** `9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4`  
**Review Scope:** 4 files (2 specs, 1 POM, 1 Component)  
**Review Status:** **`CHANGES REQUESTED`**  

---

## 📊 Summary of Findings

| Severity | Count | Status |
| :--- | :---: | :--- |
| 🛑 **Blocker** | 1 | Inline selectors found in test spec |
| ⚠️ **Major** | 2 | Hardcoded sleep timer & unisolated test database ID |
| ℹ️ **Minor** | 1 | Unnecessary descriptive comments in spec |

---

## 🛑 Blockers (Must Fix Before Merge)

### 1. Direct Inline Locators in Test Spec
- **Rule:** `BLK-01: No Inline Locators in Specs`
- **Locations:**
  - [tests/e2e/billing/invoices.spec.ts:34](https://github.com/acmecorp/web-app/blob/9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4/tests/e2e/billing/invoices.spec.ts#L34)
  - [tests/e2e/billing/invoices.spec.ts:58](https://github.com/acmecorp/web-app/blob/9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4/tests/e2e/billing/invoices.spec.ts#L58)
- **Issue Description:**
  Direct calls to `page.locator('button.download-pdf')` and `page.getByTestId('tax-summary')` are written directly inside the test specification. Test specs must interact through the Page Object to ensure centralized selector maintenance when UI classes change.
- **Remediation:**
  ```typescript
  // ❌ Current (invoices.spec.ts:34):
  await page.locator('button.download-pdf').click();

  // ✅ Recommended Fix:
  // In BillingPage (tests/e2e/pages/billing.page.ts):
  readonly downloadInvoiceButton = this.page.getByRole('button', { name: /download invoice/i });

  // In invoices.spec.ts:
  await billingPage.downloadInvoiceButton.click();
  ```

---

## ⚠️ Major Findings (Flake Risks & Architecture)

### 1. Hardcoded Timeout Sleep in Download Flow
- **Rule:** `BLK-03 / MAJ-05: No Hardcoded Sleeps`
- **Locations:**
  - [tests/e2e/billing/invoices.spec.ts:42](https://github.com/acmecorp/web-app/blob/9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4/tests/e2e/billing/invoices.spec.ts#L42)
- **Issue Description:**
  The test uses `await page.waitForTimeout(4000);` to wait for PDF generation to finish on the server before asserting download completion. This will flake under heavy CI load and slows down green runs.
- **Remediation:**
  ```typescript
  // ❌ Current:
  await billingPage.downloadInvoiceButton.click();
  await page.waitForTimeout(4000);

  // ✅ Recommended Fix (use Playwright's waitForEvent('download')):
  const downloadPromise = page.waitForEvent('download');
  await billingPage.downloadInvoiceButton.click();
  const download = await downloadPromise;
  expect(download.suggestedFilename()).toMatch(/invoice-\d+\.pdf/);
  ```

### 2. Mutable Shared Invoice ID Across Scenarios
- **Rule:** `MAJ-03: No Shared Mutable State`
- **Locations:**
  - [tests/e2e/billing/invoices.spec.ts:18](https://github.com/acmecorp/web-app/blob/9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4/tests/e2e/billing/invoices.spec.ts#L18)
- **Issue Description:**
  `test.describe` defines a shared `let sharedInvoiceId: string;` in `beforeAll`. The first test archives the invoice, causing the second test ("should edit billing address for invoice") to fail if run in isolation or if the order changes.
- **Remediation:**
  Create isolated invoice fixtures per test or reseed in `beforeEach()`.

---

## ℹ️ Minor Findings (Polish & Cleanliness)

### 1. Redundant Narration Comments
- **Locations:**
  - [tests/e2e/billing/invoices.spec.ts:25](https://github.com/acmecorp/web-app/blob/9f2b8d1a4c8e71b29a1d48c0817293a7d2e091b4/tests/e2e/billing/invoices.spec.ts#L25)
- **Note:** Comment `// click on the button and wait` can be removed.

---

## 📁 Changeset Statistics
- **Modified Tracked Files:**
  - `tests/e2e/billing/invoices.spec.ts`
  - `tests/e2e/pages/billing.page.ts`
- **New / Untracked Files Audited:**
  - `tests/e2e/components/invoice-filter-drawer.component.ts`

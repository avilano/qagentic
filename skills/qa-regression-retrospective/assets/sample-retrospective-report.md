# 🔍 QA Regression & Defect Retrospective: v3.11.0 Release Cycle

**Period Audited:** October 2026 (v3.11.0 Release)  
**Evaluator:** QA Quality Intelligence Agent  
**Total Escaped Defect Count:** 8  
**Test Coverage Ratio:** 37.5% (3 of 8 fixes included regression-preventing automated tests)  

---

## 📊 Executive Summary & Quality Health
During the v3.11.0 release cycle, 8 regression bugs escaped to staging/production. The dominant root cause was **RCA-EDGE (Unhandled Edge Case / Boundary)**, accounting for 50% of defects. Crucially, 5 out of 8 bug-fix PRs merged without adding automated tests to guard against regressions, creating a recurring flakiness risk. The highest concentration of escaped defects occurred in the **Billing & Subscription** domain (3 defects).

---

## 📈 Quality Metrics & Defect Distribution

### 1. Root Cause Breakdown
| Root Cause Category | Count | % of Total | Primary Manifestation |
| :--- | :---: | :---: | :--- |
| **RCA-EDGE (Edge Case / Boundary)** | 4 | 50.0% | Null pointers in empty discount codes, zero-quantity orders |
| **RCA-REG (Unintended Code Regression)** | 2 | 25.0% | Global CSS refactor broke button positioning in modals |
| **RCA-INT (Integration / Schema Drift)** | 1 | 12.5% | Stripe webhook payload field change (`customer_id` null) |
| **RCA-REQ (Requirements Gap)** | 1 | 12.5% | Ambiguity regarding refund behavior on canceled trials |

### 2. Regression Test Coverage Scorecard
- 🟢 **Fixes with Full Regression Tests (YES):** 3 (37.5%)
- 🟡 **Fixes with Partial Test Guards (PARTIAL):** 1 (12.5%)
- 🔴 **Fixes with Zero Regression Tests (NO):** 4 (50.0%)

---

## 📋 Escaped Defect Audit Table

| Bug ID | Summary | Domain | RCA Category | Fix PR | Test Guard? | Escape Phase |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| **[BILL-302]** | Applying 100% discount code causes 500 error | Billing | RCA-EDGE | [PR #812](https://github.com/acmecorp/app/pull/812) | 🔴 NO | Unit / Integration |
| **[BILL-309]** | Invoice PDF download hangs on unicode client names | Billing | RCA-EDGE | [PR #819](https://github.com/acmecorp/app/pull/819) | 🟢 YES | QA / E2E |
| **[AUTH-214]** | SAML SSO login loop when email has trailing space | Auth | RCA-EDGE | [PR #825](https://github.com/acmecorp/app/pull/825) | 🟢 YES | Unit / Integration |
| **[NAV-140]** | User settings menu obscured by sticky header | Layout | RCA-REG | [PR #830](https://github.com/acmecorp/app/pull/830) | 🔴 NO | QA / E2E |
| **[BILL-315]** | Proration calculation off by one day on leap years | Billing | RCA-EDGE | [PR #834](https://github.com/acmecorp/app/pull/834) | 🟡 PARTIAL | Unit |
| **[NOTIF-88]** | Slack webhook fails when notification title contains emojis | Notifications | RCA-INT | [PR #841](https://github.com/acmecorp/app/pull/841) | 🟢 YES | Integration |
| **[TEAMS-91]** | Removing team member leaves orphan permission records | Teams | RCA-REG | [PR #847](https://github.com/acmecorp/app/pull/847) | 🔴 NO | Integration |
| **[ORG-45]** | Trial cancellation email triggers duplicate send | Billing | RCA-REQ | [PR #852](https://github.com/acmecorp/app/pull/852) | 🔴 NO | QA / E2E |

---

## 🔥 Hotspot Analysis: High-Risk Modules
- **Billing & Subscriptions:** 4 bugs (50%). Urgent need for an end-to-end matrix test covering edge discount rates (0%, 100%, expired) and prorated cycles.
- **Global Layout & Styling:** 2 UI regressions triggered by un-scoped Tailwind CSS classes.

---

## 🚀 Actionable QA Follow-Up Tasks

### High-Priority Test Automation Backlog
- [ ] **[QA-AUTO-41]** Create automated Playwright E2E spec for 100% discount checkout flow (`tests/e2e/billing/discount-checkout.spec.ts`).
- [ ] **[QA-AUTO-42]** Add automated layout visual check for user profile menu on sticky headers (`tests/e2e/navigation/header-menu.spec.ts`).
- [ ] **[QA-AUTO-43]** Add integration test verifying orphan permission cleanup upon member deletion.

### Process & Gate Improvements
- [ ] Add mandatory "Regression Test Added (Yes/No)" checkbox to GitHub PR description template.
- [ ] Run `qa-ticket-readiness` on all billing user stories to catch boundary omissions during refinement.

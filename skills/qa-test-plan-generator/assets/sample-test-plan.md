# 📑 QA Test Plan: [BILL-104] Automated Tiered Subscription Upgrades

**Author:** QA Engineering  
**Source Specification:** [BILL-104 (Jira Software)](https://jira.acmecorp.io/browse/BILL-104)  
**Target Release / Sprint:** `v3.12.0 (Sprint 44)`  
**Status:** `READY FOR REVIEW`  

---

## 1. 🎯 Objectives & Scope

### 1.1 Business Objective
Allow self-serve enterprise organizations to upgrade from Standard to Premium subscription tiers with prorated credit card billing without customer support assistance.

### 1.2 In-Scope Items
- 🟢 **Tier Upgrade Modal:** Plan selection, prorated calculation preview, payment method confirmation.
- 🟢 **Immediate Feature Unlock:** Instant provisioning of Premium feature flags (`seats.unlimited`, `sso.saml`).
- 🟢 **Stripe Webhook Processing:** Real-time handling of `customer.subscription.updated` and invoice events.

### 1.3 Out-of-Scope Items
- ❌ **Downgrades:** Tier downgrades are scheduled at end-of-billing-period (covered under BILL-110).
- ❌ **Invoice Factoring / Manual Wire:** Wire transfer upgrades require manual Sales ops approval.

---

## 2. 💻 Test Environment & Setup

- **Target Environments:** Staging environment connected to Stripe Test Mode.
- **Feature Flags:** `billing.self_serve_upgrades = true`
- **Test Accounts:**
  - Standard plan organization with active credit card (`tok_visa`).
  - Standard plan organization with past-due payment (`tok_chargeDeclined`).
- **User Roles:** `Owner`, `Billing Admin`, `Standard Member`.

---

## 3. 🧪 Test Scenario Breakdown

### 3.1 Tier 1: Core Positive Flows & E2E Workflows (Automation Candidates)
| ID | Title & Traceability | Preconditions | Steps & Action | Expected Outcome | Execution |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-01** | Happy Path Upgrade `[AC-1]` | Logged in as Billing Admin on Standard plan | 1. Navigate to `/settings/billing`<br>2. Click "Upgrade to Premium"<br>3. Confirm prorated total<br>4. Click "Confirm Upgrade" | Success toast displayed; plan badge updates to "Premium"; premium routes unlocked immediately | Automated (Playwright) |
| **TC-02** | Invoice Receipt Email `[AC-2]` | Completed TC-01 | Check Mailhog mailbox for org owner email | Itemized PDF receipt received within 30 seconds | Automated (API/Webhook) |

### 3.2 Tier 2: Negative Paths & Error Handling
| ID | Title & Traceability | Preconditions | Steps & Action | Expected Outcome | Execution |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-03** | Declined Payment Card `[AC-3]` | Org payment card set to `tok_chargeDeclined` | Click "Confirm Upgrade" | Inline payment error banner displayed; plan remains "Standard"; no partial locks | Automated (Playwright) |
| **TC-04** | Double Click Mitigation | On upgrade modal | Rapidly double-click "Confirm Upgrade" button | Button disables on first click; exactly 1 Stripe charge created | Automated (Playwright) |

### 3.3 Tier 3: Boundary & Threshold Limits
| ID | Title & Traceability | Boundary Tested | Steps | Expected Outcome |
| :--- | :--- | :--- | :--- | :--- |
| **TC-05** | Proration at Last Minute of Billing Cycle | 1 minute before renewal | Execute upgrade at 23:59 on cycle end date | Proration calculation equals 0; full next-cycle charge applied accurately |

### 3.4 Tier 4: Permission & Multi-Role Access Matrix
| Scenario / Feature Action | Billing Admin | Standard Member | External Guest |
| :--- | :---: | :---: | :---: |
| View "Upgrade" Button | ✅ Visible | ❌ Hidden | ❌ Hidden |
| Execute Upgrade API Call | ✅ HTTP 200 | ❌ HTTP 403 Forbidden | ❌ HTTP 401 Unauthorized |
| Download Upgrade Invoice | ✅ Allowed | ❌ Forbidden | ❌ Forbidden |

### 3.5 Tier 5: Accessibility & Responsiveness
- [ ] **TC-06 (A11y):** Plan comparison pricing table headers use valid `th scope="col"` tags (`axe-core` clean).
- [ ] **TC-07 (Keyboard):** Modal focus is trapped within the upgrade dialog until dismissed.
- [ ] **TC-08 (Responsive):** On 375px mobile viewport, pricing cards stack vertically with prominent touch targets (min 44px).

---

## 4. 🌐 Regression Blast Radius & Impact Areas

| Affected Area | Potential Risk | Mitigation / Test Approach |
| :--- | :--- | :--- |
| **Member Seat Allocation** | Upgrading tier could reset current seat count | Run `tests/e2e/teams/seat-allocation.spec.ts` |
| **Single Sign-On (SSO)** | Enabling Premium unlocks SAML; verify standard password logins remain unaffected | Run regression smoke suite `auth-smoke.spec.ts` |

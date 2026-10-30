# QA Regression Retrospective Report Template

Use this format when generating the release or monthly defect retrospective deliverable.

```markdown
# 🔍 QA Regression & Defect Retrospective: {Period / Release}

**Period Audited:** `{Timeframe or Release Version}`  
**Evaluator:** QA Quality Intelligence Agent  
**Total Escaped Defect Count:** `{Total}`  
**Test Coverage Ratio:** `{Percentage}%` of fixes included regression-preventing automated tests  

---

## 📊 Executive Summary & Quality Health
{High-level narrative summarizing escaped bug volume, primary failure modes, escaped defect trends, and top risks.}

---

## 📈 Quality Metrics & Defect Distribution

### 1. Root Cause Breakdown
| Root Cause Category | Count | % of Total | Primary Manifestation |
| :--- | :---: | :---: | :--- |
| **RCA-EDGE (Edge Case / Boundary)** | {N} | {%} | Unhandled nulls, empty states |
| **RCA-REG (Unintended Code Regression)** | {N} | {%} | Collateral damage from adjacent refactors |
| **RCA-REQ (Requirements Gap)** | {N} | {%} | Incomplete acceptance criteria |
| **RCA-INT (Integration / Schema Drift)** | {N} | {%} | API response contract drift |
| **RCA-RACE (Concurrency / Timing)** | {N} | {%} | Double clicks, async race conditions |

### 2. Regression Test Coverage Scorecard
- 🟢 **Fixes with Full Regression Tests (YES):** `{Count}` ({%})
- 🟡 **Fixes with Partial Test Guards (PARTIAL):** `{Count}` ({%})
- 🔴 **Fixes with Zero Regression Tests (NO):** `{Count}` ({%})

---

## 📋 Escaped Defect Audit Table

| Bug ID | Summary | Domain / Module | RCA Category | Fix PR | Test Guard? | Escape Phase |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| **[BUG-101]** | Invoice download fails for zero-tax items | Billing | RCA-EDGE | [PR #412](...) | 🔴 NO | Unit / E2E |
| **[BUG-108]** | User dropdown closes on mobile scroll | Navigation | RCA-REG | [PR #418](...) | 🟢 YES | QA / E2E |

---

## 🔥 Hotspot Analysis: High-Risk Modules
1. **Module A (`billing/invoices`):** 4 escaped bugs this cycle. Root causes driven by edge-case pricing combinations.
2. **Module B (`auth/sso`):** 2 escaped regressions from recent library upgrade.

---

## 🚀 Actionable QA Follow-Up Tasks

### High-Priority Test Automation Backlog
- [ ] **[QA-AUTO-01]** Create Playwright spec for zero-tax invoice download (`tests/e2e/billing/zero-tax-invoice.spec.ts`).
- [ ] **[QA-AUTO-02]** Add API schema contract validation test for billing endpoint.

### Process & Gate Improvements
- [ ] Incorporate negative boundary checklist into Definition of Ready audits for Billing epics.
```

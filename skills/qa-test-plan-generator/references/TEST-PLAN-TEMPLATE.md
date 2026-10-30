# Standard QA Test Plan Document Template

Standard markdown specification for comprehensive QA test plans.

```markdown
# 📑 QA Test Plan: [{KEY}] {Feature Name}

**Author:** QA Engineering  
**Source Specification:** [{KEY} ({Tracker URL})]({Tracker URL})  
**Target Release / Sprint:** `{Release Version}`  
**Status:** `{DRAFT | READY FOR REVIEW | SIGNED OFF}`  

---

## 1. 🎯 Objectives & Scope

### 1.1 Business Objective
{Summary of user problem and business goal.}

### 1.2 In-Scope Items
- 🟢 **Feature Area 1:** {Description of functional area covered}
- 🟢 **Feature Area 2:** {Description}

### 1.3 Out-of-Scope Items
- ❌ **Excluded Area 1:** {Description and rationale: e.g. deferred, tested by separate team}
- ❌ **Excluded Area 2:** {Description}

---

## 2. 💻 Test Environment & Setup

- **Target Environments:** `{Staging / QA Preview Environment}`
- **Feature Flags:** `{flag.name = true}`
- **Test Data Requirements:**
  - {Entity seed requirements, test accounts, mock payloads}
- **Required User Roles:**
  - `Admin`
  - `Standard User`
  - `Read-Only Auditor`

---

## 3. 🧪 Test Scenario Breakdown

### 3.1 Tier 1: Core Positive Flows & E2E Workflows (Automation Candidates)
| ID | Title & Traceability | Preconditions | Steps & Action | Expected Outcome | Execution |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-01** | Happy Path Primary Flow `[AC-1]` | User logged in as Admin | 1. Navigate to ...<br>2. Fill ...<br>3. Submit | Success toast and entity created | Automated (Playwright) |

### 3.2 Tier 2: Negative Paths & Error Handling
| ID | Title & Traceability | Preconditions | Steps & Action | Expected Outcome | Execution |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-02** | Validation Error Display `[AC-2]` | On form screen | Leave required fields empty and click submit | Inline error messages displayed | Automated |
| **TC-03** | Network Disruption during Save | Network offline | Submit form | Retry banner displayed without data loss | Manual |

### 3.3 Tier 3: Boundary & Threshold Limits
| ID | Title & Traceability | Boundary Tested | Steps | Expected Outcome |
| :--- | :--- | :--- | :--- | :--- |
| **TC-04** | Max Character Length `[AC-3]` | Input limit 255 chars | Paste 256 characters | Input capped at 255 |

### 3.4 Tier 4: Permission & Multi-Role Access Matrix
| Scenario / Feature Action | Admin | Member | Guest / Read-Only |
| :--- | :---: | :---: | :---: |
| Create New Entity | ✅ Allowed | ❌ Forbidden (403) | ❌ Button Hidden |
| View Entity | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| Delete Entity | ✅ Allowed | ❌ Forbidden (403) | ❌ Forbidden |

### 3.5 Tier 5: Accessibility & Responsiveness
- [ ] **TC-05 (A11y):** Form fields have proper `aria-label` / `for` attribute association (`axe-core` clean).
- [ ] **TC-06 (Keyboard):** Modal can be navigated via Tab and closed via `Escape` key.
- [ ] **TC-07 (Responsive):** Viewport rendered at 375px (mobile) displays sticky action drawer.

---

## 4. 🌐 Regression Blast Radius & Impact Areas

| Affected Area | Potential Risk | Mitigation / Test Approach |
| :--- | :--- | :--- |
| **User Authentication / Session** | Session token invalidated prematurely | Run existing smoke suite `auth.spec.ts` |
| **Search Indexing** | Newly created entities delayed in search | Verify Elasticsearch index sync delay <2s |

---

## 5. ✍️ Sign-Off & Approvals

| Role | Name | Status | Date |
| :--- | :--- | :---: | :--- |
| **QA Lead** | @qa-lead | Pending | YYYY-MM-DD |
| **Product Manager** | @pm | Pending | YYYY-MM-DD |
| **Tech Lead** | @tech-lead | Pending | YYYY-MM-DD |
```

# Definition of Ready (DoR) Audit Checklist

Evaluates the ticket against strict Quality Gates divided into two tiers.

---

## 1. Audit Checkpoints

### Tier 1: Core Definition of Ready (Must-Have Gates)

All Tier 1 items must pass to achieve Grade A or B. A failure in Tier 1 results in Grade C or F.

| ID | Check | Criteria | Pass Condition |
| :--- | :--- | :--- | :--- |
| **T1.1** | **Problem Statement** | Story or defect clearly states *who* is affected and *what* problem is being solved. | User story format (`As a... I want... So that...`) or clear bug description with expected vs actual. |
| **T1.2** | **Verifiable Acceptance Criteria** | Criteria are unambiguous, testable, and falsifiable. | Bulleted checklist, Gherkin scenarios (`Given/When/Then`), or explicit validation rules. Vague words like "intuitive" or "handle errors properly" fail without specifics. |
| **T1.3** | **Clear Boundaries** | In-scope features and out-of-scope exclusions are stated. | Explicit statement of what is included and what is deferred. |
| **T1.4** | **Reproduction / Testability** | QA can verify the change in a test environment. | Deterministic steps or clear triggering actions documented. |

---

### Tier 2: Quality & Automation Readiness (Enrichment Gates)

Enhances test efficiency and automation readiness. Gaps here lower Grade from A to B.

| ID | Check | Criteria | Pass Condition |
| :--- | :--- | :--- | :--- |
| **T2.1** | **Negative & Edge Cases** | Error handling, validation failures, and edge conditions identified. | At least 1-2 negative paths or boundary cases documented. |
| **T2.2** | **Test Data & Roles** | Specific roles, permissions, or prerequisite data configurations named. | User roles (e.g. Admin vs Read-only) or seed data prerequisites stated. |
| **T2.3** | **Feature Flags & Config** | Flag names, environment variables, or schema migrations noted. | Explicit flag name (e.g. `enable-new-checkout`) or `N/A` confirmed. |
| **T2.4** | **UI/UX Artifacts** | Designs, Figma links, or wireframes provided (for UI stories). | Figma link, screenshot, or explicit backend-only note. |
| **T2.5** | **Regression Awareness** | Adjacent modules or existing workflows that could be impacted. | List of potentially impacted areas or dependencies. |

---

## 2. Letter Grade Rubric

```text
┌─────────────┬────────────────────────────────────────────────────────┐
│ Grade A     │ 100% Tier 1 PASS + 80%+ Tier 2 PASS                    │
│             │ High confidence. Ready for sprint / QA test design.   │
├─────────────┼────────────────────────────────────────────────────────┤
│ Grade B     │ 100% Tier 1 PASS, minor Tier 2 missing                 │
│             │ Workable. QA can proceed while clarifying minor gaps.   │
├─────────────┼────────────────────────────────────────────────────────┤
│ Grade C     │ Any Tier 1 FAIL (fixable via Epic inheritance or edit) │
│             │ BLOCKED. Needs refinement before development starts.   │
├─────────────┼────────────────────────────────────────────────────────┤
│ Grade F     │ Multiple Tier 1 FAIL / Missing Core AC and Context     │
│             │ UNUSABLE. Stub ticket; reject to Product/Author.       │
└─────────────┴────────────────────────────────────────────────────────┘
```

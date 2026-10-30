# Defect Root Cause Analysis (RCA) Taxonomy

Standard taxonomy for classifying escaped bugs and regression defects during retrospectives.

---

## 1. Primary Root Cause Categories

| Code | Category | Definition | Typical Manifestation |
| :--- | :--- | :--- | :--- |
| **RCA-REQ** | **Requirements & Ambiguity** | Requirements were incomplete, missing, or contradictory. | "The ticket never specified what should happen when a user cancels payment midway." |
| **RCA-EDGE** | **Unhandled Edge Case / Boundary** | Code handled the primary flow but failed on boundary inputs, nulls, or empty collections. | `TypeError: Cannot read properties of undefined (reading 'items')` when array is empty. |
| **RCA-INT** | **Integration / Schema Drift** | Disconnect between frontend expectations and backend API contract, or breaking third-party payload change. | Backend renamed field from `total_cents` to `amount_cents`; UI rendered `$NaN`. |
| **RCA-RACE** | **Concurrency & State Race** | Timing-dependent issue, race condition between multiple asynchronous promises, or uncoordinated database writes. | User clicks button twice quickly; duplicate transaction created. |
| **RCA-ENV** | **Environment & Configuration** | Bug caused by misconfigured environment variables, mismatched feature flags, or missing seed migration. | Feature flag was enabled on Production without running the associated DB migration. |
| **RCA-REG** | **Unintended Code Regression** | Modifying code in Feature A accidentally broke unrelated functionality in Feature B. | Refactoring the common `DatePicker` component broke the Calendar scheduling view. |

---

## 2. Escape Point Attribution

Where in the engineering lifecycle should the defect have been caught?

```
[Requirement Refinement] ──> [Developer Unit Tests] ──> [PR Code Review] ──> [QA E2E Automation] ──> [Production Escape]
```

- **Escape Phase: Requirements** &rarr; Ambiguity should have been flagged during DoR audit (`qa-ticket-readiness`).
- **Escape Phase: Unit / Integration** &rarr; Logic boundary should have had an automated unit test.
- **Escape Phase: QA / E2E** &rarr; Critical user workflow lacked an automated browser regression test.
- **Escape Phase: Staging / Deploy** &rarr; Environment discrepancy between Staging and Production.

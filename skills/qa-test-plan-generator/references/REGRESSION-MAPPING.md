# Regression Blast Radius and Test Deduplication Guide

Techniques for discovering unintended ripple effects and preventing test suite bloat.

---

## 1. Blast Radius Mapping

When a new feature or bug fix touches the codebase, trace its blast radius across three tiers:

```
[Tier 1: Direct Mutations]  ──> Database collections, API endpoints, UI forms
           │
           ▼
[Tier 2: Upstream Feeders]  ──> Background cron jobs, Webhook listeners, Caching layers (Redis)
           │
           ▼
[Tier 3: Downstream Consumers] ──> Reporting dashboards, Analytics events, Third-party exports
```

### Risk Evaluation Questions
1. **Shared State:** Does this feature modify shared database tables or user session objects?
2. **Global Components:** Does this feature modify shared UI components (e.g. `Navbar`, `DatePicker`, `DataTable`)?
3. **Data Migrations:** Does this feature require a database schema migration or backfill?
4. **Third-Party APIs:** Does this feature alter payloads sent to external services (Stripe, SendGrid, Segment)?

---

## 2. Test Deduplication Strategy

Do not write redundant test cases that duplicate existing coverage:
1. **Audit Existing Automated Tests:** Search existing test files (e.g. `tests/e2e/**/*.spec.ts`) using ripgrep for keywords related to the feature.
2. **Distinguish Integration vs E2E:** If an edge case can be validated with a fast unit/API integration test, do not add it as a heavyweight browser E2E test.
3. **Reuse Existing Setup Fixtures:** Leverage existing project fixtures and mock helpers instead of generating new standalone test setup boilerplate.

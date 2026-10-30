# Test Scenario Breakdown & Deconstruction Methodology

Systematic framework for decomposing software requirements into exhaustive test cases.

---

## 1. The 5-Dimension Test Deconstruction Matrix

When analyzing any acceptance criterion, apply five analytical lenses:

```
                  ┌───────────────────────────────┐
                  │    Acceptance Criterion       │
                  └──────────────┬────────────────┘
                                 │
     ┌─────────────┬─────────────┼─────────────┬─────────────┐
     ▼             ▼             ▼             ▼             ▼
[Positive]    [Negative]    [Boundaries]   [Permissions]   [UX/A11y]
Happy Path    Error states  Min/Max/Zero   Role Matrix    Keyboard/ARIA
E2E Flow      Timeouts      Pagination     Tenant Leak    Mobile Screen
```

---

## 2. Dimension Guidelines

### Dimension 1: Positive / Happy Paths
- Verify the intended primary workflow from start to completion.
- Verify state persistence (refresh page, verify data remains saved).
- Verify side-effects (notifications sent, audit log generated, webhook triggered).

### Dimension 2: Negative & Error States
- Verify field-level validation (missing required fields, malformed emails/UUIDs).
- Verify business logic conflicts (duplicate key creation, submitting stale data).
- Verify server-side error handling (HTTP 400, 404, 409, 500 responses return user-friendly error banners, never blank screens).

### Dimension 3: Boundary & Threshold Limits
- **Numeric Fields:** Test `0`, `-1`, `1`, `max_allowed`, `max_allowed + 1`.
- **String Fields:** Empty string `""`, whitespace only `"   "`, single char, max length, max length + 1, Unicode/emojis, SQL/XSS payloads (`<script>alert(1)</script>`).
- **Collections:** Empty list (Zero-State UI), exactly 1 item, page size limit (e.g. 10 items), page 2 transition.

### Dimension 4: Role-Based Access Control (RBAC)
- Construct a two-dimensional grid: Columns = User Roles, Rows = Actions.
- Ensure unauthorized roles cannot view sensitive UI elements.
- Verify direct API manipulation: ensure unauthorized roles receive HTTP 403 Forbidden even if crafting direct HTTP calls.

### Dimension 5: UX, Accessibility & Responsiveness
- Run automated `axe-core` assertions on all new views and modals.
- Validate tab index navigation order and focus traps inside open dialogs.
- Check viewports: Desktop (1280px+), Tablet (768px), Mobile (375px).

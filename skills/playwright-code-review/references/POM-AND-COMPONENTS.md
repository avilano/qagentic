# Page Object Model & Component Object Architecture

Standards for clean, maintainable Playwright test automation hierarchies.

---

## 1. Class Hierarchy Architecture

```
                  ┌──────────────┐
                  │   BasePOM    │  (Abstract root: page, context)
                  └──────┬───────┘
                         │
         ┌───────────────┴───────────────┐
         ▼                               ▼
  ┌──────────────┐               ┌───────────────┐
  │   BasePage   │               │ BaseComponent │  (Scoped container / root)
  └──────┬───────┘               └───────┬───────┘
         │                               │
         ▼                               ▼
  ┌──────────────┐               ┌───────────────┐
  │ SettingsPage │ (Composes) ──>│  InviteModal  │
  └──────────────┘               └───────────────┘
```

- **`BasePage`:** Represents an entire navigable route or URL (e.g. `DashboardPage`, `LoginPage`). Owns top-level navigation, page-wide layout checks, and sub-component composition.
- **`BaseComponent`:** Represents an isolated, reusable portion of a page scoped to a `root: Locator` (e.g. `Navbar`, `Modal`, `DataTable`, `FilterSidebar`).

---

## 2. Page Object Design Rules

### Locators First, Actions Second
Organize Page Objects with clear section boundaries:
1. **Locators at the top:**
   - Use `readonly` properties for static selectors without arguments:
     ```typescript
     readonly emailInput = this.page.getByLabel('Work Email');
     readonly submitButton = this.page.getByRole('button', { name: 'Log In' });
     ```
   - Use locator getters for dynamic or parameterized selectors:
     ```typescript
     getUserRow(email: string): Locator {
       return this.page.getByRole('row').filter({ hasText: email });
     }
     ```
2. **Intent-driven Action methods below:**
   - Combine multi-step workflows into domain-level methods:
     ```typescript
     async login(credentials: UserCredentials): Promise<void> {
       await this.emailInput.fill(credentials.email);
       await this.passwordInput.fill(credentials.password);
       await this.submitButton.click();
     }
     ```

### Zero Assertions in POMs
Page Objects model state and behavior; specs verify test assertions.
- ❌ **Forbidden in POM:** `await expect(this.header).toHaveText('Welcome');`
- ✅ **Correct pattern:** Expose the locator or state query to the spec:
  - In POM: `readonly header = this.page.getByRole('heading', { level: 1 });`
  - In Spec: `await expect(dashboardPage.header).toHaveText('Welcome');`

---

## 3. Component Objects for Modals & Reusable UI

Modals, dialogs, drawers, and popovers must never clutter a Page Object with dozen of flat locators.
Always extract a dedicated Component:

```typescript
export class ConfirmationModal extends BaseComponent {
  readonly title = this.root.getByRole('heading', { level: 2 });
  readonly confirmButton = this.root.getByRole('button', { name: /confirm|delete/i });
  readonly cancelButton = this.root.getByRole('button', { name: 'Cancel' });

  async confirm(): Promise<void> {
    await this.confirmButton.click();
    await this.root.waitFor({ state: 'hidden' });
  }
}
```

Compose the component inside the parent Page Object:
```typescript
export class UsersPage extends BasePage {
  readonly confirmDeleteModal = new ConfirmationModal(
    this.page.getByRole('dialog', { name: 'Delete User' }),
    this.page
  );
}
```

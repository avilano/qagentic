# Playwright Code Review Report Template

Use this format when generating the final code review markdown deliverable.

```markdown
# 🎭 Playwright E2E Code Review Report

**Target:** {PR Title or Local Branch}  
**Reference / Commit:** `{HEAD_SHA or Branch Name}`  
**Review Scope:** {Total Files} files ({Specs Count} specs, {POMs Count} POMs/components)  
**Review Status:** **`{APPROVED | CHANGES REQUESTED | MERGE BLOCKED}`**  

---

## 📊 Summary of Findings

| Severity | Count | Status |
| :--- | :---: | :--- |
| 🛑 **Blocker** | {Count} | {Must fix before merge} |
| ⚠️ **Major** | {Count} | {Flake risk / high recommendation} |
| ℹ️ **Minor** | {Count} | {Polish / consistency} |

---

## 🛑 Blockers (Must Fix Before Merge)

### 1. {Finding Title}
- **Rule:** `{RULE_ID}: {Rule Name}`
- **Locations:**
  - [{file_path}:{line}]({link})
  - [{file_path}:{line}]({link})
- **Issue Description:**
  {Explanation of what violates the rule and why it impacts reliability or architecture.}
- **Remediation:**
  ```typescript
  // ❌ Current Code:
  ...
  // ✅ Recommended Fix:
  ...
  ```

---

## ⚠️ Major Findings (Flake Risks & Architecture)

### 1. {Finding Title}
- **Rule:** `{RULE_ID}: {Rule Name}`
- **Locations:**
  - [{file_path}:{line}]({link})
- **Issue Description:**
  {Explanation}
- **Remediation:**
  ```typescript
  ...
  ```

---

## ℹ️ Minor Findings (Polish & Cleanliness)

### 1. {Finding Title}
- **Locations:**
  - [{file_path}:{line}]({link})
- **Note:** {Observation}

---

## 📁 Changeset Statistics
- **Modified Tracked Files:**
  - `path/to/spec.ts`
- **New / Untracked Files Audited:**
  - `path/to/new-page.page.ts`
```

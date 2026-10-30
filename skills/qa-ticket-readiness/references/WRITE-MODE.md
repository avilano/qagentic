# Write Mode Safety Protocol & Execution Contract

Rules governing write operations when updating remote issue trackers (Jira, GitHub Issues, Linear).

---

## 1. Safety Principles

1. **Dry-Run by Default:** Unless the user explicitly commanded a write operation (`fix`, `populate`, `update`), the skill strictly operates in read-only mode.
2. **Explicit User Approval Gate:** The agent must present the exact unified diff or proposed text and await an unambiguous positive confirmation from the user (e.g. "yes", "proceed", "apply diff") before executing the API call.
3. **No Overwrites Without Context Preservation:** If the ticket already has an existing description or user notes, the agent must **append or integrate** rather than wiping out existing notes.
4. **Strict Scope Limitation:**
   - **Allowed to update:** Description, acceptance criteria field, label additions (e.g. `dor-passed`).
   - **FORBIDDEN to update:** Issue status, assignee, priority, sprint assignment, story points, or deleting existing comments.
5. **Post-Write Verification:** Immediately after executing the write call, re-fetch the ticket and verify the updated content, re-running the readiness audit to confirm the grade improved.

---

## 2. Interactive Flow

```
[User triggers Write Mode]
          │
          ▼
[Agent performs audit & generates proposed enrichment]
          │
          ▼
[Agent presents Before/After Diff in Markdown]
          │
          ▼
[Agent asks: "Do you want to apply these updates to {TICKET_KEY}? (yes/no)"]
          │
     ┌────┴────────────┐
     ▼                 ▼
[User: "yes"]     [User: "no" / modifies]
     │                 │
     ▼                 ▼
[Call Tracker API]  [Cancel or revise diff]
     │
     ▼
[Verify updated ticket & output final grade]
```

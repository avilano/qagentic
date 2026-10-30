---
name: qa-epic-scoping
description: Produce a read-only Epic QA Brief covering motivation, scope, app surfaces, stories, test data, existing test coverage, and open questions before any test plan is written. Activate when a user asks to understand, scope, orient, or gather QA context on an epic, feature milestone, or user story set. Do not use to generate formal test plans, write automated test code, or mutate ticket state.
metadata:
  author: Alberto Vilano
  owner: QAgentic Community
  version: "1.0.0"
  category: Quality Assurance
allowed-tools: >-
  Task(*) Read Grep Glob Write AskUserQuestion
---

# QA Epic Scoping

Produce a read-only **Epic QA Brief** so a quality engineer can understand an epic, milestone, or feature scope in 5–10 minutes before planning coverage.

## The Read-Only Contract
- **Zero remote writes:** Never mutate Jira, GitHub Issues, Linear, Notion, Slack, Test Management Systems (TMS), or repository source files.
- **Single local artifact:** The only artifact written is `EPIC-QA-BRIEF-{KEY}.md` in the current session scratchpad or workspace.
- **Missing connectors:** If an external MCP connector (Jira, GitHub, Linear, Notion, Slack) is absent, degrade gracefully: quarantine missing claims to `unverified[]` rather than halting or hallucinating.

---

## When to Use

### Activate On (Positive Triggers)
- "QA context on [EPIC/KEY]"
- "Birds-eye view of epic [KEY]"
- "Help me understand this feature/epic for QA"
- "Prep test plan for [KEY]" (scoping/prep phase only)
- "What do I need to test for [FEATURE/KEY]?"
- "Scope out [KEY] before I plan coverage"
- "Identify affected surfaces and test data needed for [KEY]"
- "What are the open questions on this epic?"

### Do NOT Activate On (Negative Triggers / Boundaries)
- "Create test plan for [KEY]" → Use test planning skills (e.g., `qa-test-plan-generator`).
- "Write Playwright/Cypress tests for [KEY]" → Use test authoring skills.
- "Review PR or code changes" → Use code review skills.
- "Spec out engineering architecture" → Use software design/spec skills.

---

## 5-Step Procedural Workflow

```
[1. Resolve Target] ➔ [2. Launch Collectors] ➔ [3. Verify Findings] ➔ [4. Synthesize Brief] ➔ [5. Deliver]
```

### Step 1: Resolve Target & Issue Hierarchy
1. Read the input identifier (Jira Epic Key, GitHub Milestone/Issue, Linear project, or local feature spec).
2. If using an issue tracker, load [TRACKER-ADAPTERS.md](./references/TRACKER-ADAPTERS.md).
3. If the input is a child issue/story, resolve upward to its parent epic or milestone.
4. Fetch all child issues, acceptance criteria, description fields, and discussion comments.

### Step 2: Context Collectors
Collect context across technical and organizational surfaces. If subagents are supported, launch parallel collectors; otherwise execute sequentially:
- **Requirements Collector:** Reads acceptance criteria, user stories, and feature flags.
- **Codebase Scout:** Inspects the active workspace for affected frontend/backend routes, models, components, and existing E2E/integration specs.
- **Documentation Scout:** (Optional) If Notion, Confluence, or design docs are linked, extract RFCs, UX mocks, and business requirements.
- **Discussion Scout:** (Optional) If Slack/Teams/PR comments are linked, capture team discussion and edge-case mentions.

*Refer to [CONTEXT-COLLECTORS.md](./references/CONTEXT-COLLECTORS.md) for collector personas and operational gates.*

### Step 3: Verification & Truth Gates
Every collected item must pass the verification rules in [VERIFICATION-GATES.md](./references/VERIFICATION-GATES.md):
- **Verbatim AC:** Acceptance criteria must be quoted verbatim from the source. No loose paraphrasing.
- **Verified Code Paths:** File paths, test suites, and feature flags must be verified in the local workspace via `Glob` or `Grep`.
- **Quarantine Unverified Claims:** Any unconfirmed assumption or rumor from chat/docs moves to `unverified[]` and is flagged in the **Open Questions** table.

### Step 4: Synthesize Brief
1. Populate the structured template in [EPIC-QA-BRIEF-TEMPLATE.md](./references/EPIC-QA-BRIEF-TEMPLATE.md).
2. Keep the brief dense, actionable, and readable in 5–10 minutes.
3. Replace empty sections with *"None identified"* rather than speculative boilerplate.

### Step 5: Deliver Artifact
1. Write the brief to `./EPIC-QA-BRIEF-{KEY}.md` (or the active session scratchpad).
2. Present the key summary findings, affected test surfaces, and open questions in the chat.
3. Suggest handing the brief to a test planning workflow as a separate, user-initiated action.

---

## References & Deep Docs
- [EPIC-QA-BRIEF-TEMPLATE.md](./references/EPIC-QA-BRIEF-TEMPLATE.md) - Standard Markdown template for the brief.
- [CONTEXT-COLLECTORS.md](./references/CONTEXT-COLLECTORS.md) - Specialized collector personas and boundary rules.
- [VERIFICATION-GATES.md](./references/VERIFICATION-GATES.md) - Rules for citing sources, verifying code paths, and quarantining rumors.
- [TRACKER-ADAPTERS.md](./references/TRACKER-ADAPTERS.md) - Guidance for querying Jira, GitHub Projects, Linear, or local specs.
- [SKILL-VALIDATION.md](./references/SKILL-VALIDATION.md) - Audit rubric and evaluation checklist for this skill.

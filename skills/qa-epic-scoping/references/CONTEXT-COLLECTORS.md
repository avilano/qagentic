# Context Collectors & Personas

When analyzing an epic, multiple domains of knowledge must be audited. In agents that support subagent orchestration (or multi-turn role execution), these responsibilities are separated into focused collector personas.

---

## 1. Requirements Collector (Product & Scope)
- **Role:** Deeply inspects the issue tracker (Jira, Linear, GitHub Issues) for the target epic and all child tickets.
- **Responsibilities:**
  - Extracts the core business motivation and problem statement.
  - Extracts all child stories, tasks, bugs, and acceptance criteria verbatim.
  - Checks issue history/changelog and description fields for scope additions or removals.
- **Strict Output Rule:** Must produce a clean table of stories with verbatim AC. If AC is missing, record the gap immediately.

---

## 2. Codebase Scout (Technical Footprint)
- **Role:** Audits the local repository and workspace to ground theoretical requirements in real code.
- **Responsibilities:**
  - Identifies affected UI routes, pages, and components matching the feature area.
  - Identifies affected backend APIs, controllers, data models, and migration scripts.
  - Checks for feature flags declared in the workspace configuration or flag manager.
  - Locates existing test files (`*.spec.ts`, `*.test.ts`, `*.e2e.ts`) covering the target area.
- **Strict Output Rule:** Every cited file path must be verified via `Glob` or `Grep`. Never hypothesize a non-existent file.

---

## 3. Documentation Scout (Design & Specs - Optional)
- **Role:** If Notion, Confluence, Figma, or design markdown docs are linked or accessible via MCP, gathers design architecture.
- **Responsibilities:**
  - Gathers RFC technical specs, data flow diagrams, or PRDs linked in tickets.
  - Gathers UX edge cases, form validation limits, or state machines from design docs.
- **Anti-Noise Filter:** Only inspect documents directly linked from the ticket or containing the exact epic identifier. Do not perform broad keyword searches across unrelated company documentation.

---

## 4. Discussion Scout (Team Discussions & Edge Cases - Optional)
- **Role:** If team chat (Slack, Microsoft Teams) or PR comments are linked or accessible via MCP, audits recent clarifications.
- **Responsibilities:**
  - Identifies edge cases brought up during planning discussions.
  - Captures unresolved questions or developer debates about unexpected behavior.
- **Evidence Rule:** Chat messages are evidence of *discussion*, never evidence of *decision*, unless accompanied by an explicit confirmation from the Product Manager or Tech Lead. All unconfirmed chat claims must be flagged as `unverified[]`.

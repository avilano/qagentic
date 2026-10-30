# Skill Validation & Quality Rubric

Use this checklist to audit the implementation of `qa-epic-scoping` and verify that generated QA briefs meet production quality standards.

---

## 1. Frontmatter & Progressive Disclosure Audit

- [ ] **Name:** Exactly matches parent directory `qa-epic-scoping`.
- [ ] **Description:** Under 100 tokens, written in third person, includes clear positive trigger keywords and negative boundaries (what NOT to do).
- [ ] **Line Count:** `SKILL.md` is under 500 lines / ~5,000 tokens (assumes LLM competence, avoids generic coding filler).
- [ ] **Lazy-Loading Links:** Supporting deep docs are kept in `references/` and referenced via conditional markdown links.

---

## 2. Read-Only Safety Contract Audit

- [ ] **Zero Remote Writes:** The agent never creates or updates an issue in Jira/GitHub/Linear, never posts comments, and never modifies files in the repository.
- [ ] **Single Artifact Output:** The only file created is `EPIC-QA-BRIEF-{KEY}.md` in the current session scratchpad or workspace.
- [ ] **Graceful Degradation:** If external tools (Jira, Slack, Notion) are absent or fail permissions, the skill continues with available workspace context without crashing.

---

## 3. Grounding & Truth Verification Audit

- [ ] **Verbatim AC:** Acceptance criteria are directly quoted from source tickets, not paraphrased into generic summaries.
- [ ] **Citations:** Every cited file path, route, or feature flag is verified in the workspace using `Glob` or `Grep`. No fabricated filenames.
- [ ] **No Guessing Open Questions:** Unresolved requirements are surfaced in the Open Questions table with a suggested owner (PM/Tech Lead); the agent does not invent answers.
- [ ] **Discussion vs Decision:** Chat comments (Slack/Teams) are never elevated to hard requirements without explicit approval evidence.

---

## 4. Output Usability Audit

- [ ] **Readability:** The brief is structured so a human QA engineer can review and understand the feature scope in 5–10 minutes.
- [ ] **Conciseness:** Empty sections say *"None identified"* rather than using speculative boilerplate.
- [ ] **Hand-off Ready:** The brief cleanly provides all necessary prerequisites for the next phase (test plan generation or test automation).

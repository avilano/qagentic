# AI Agent Guidelines & Memory: Best Practices for Skill Building

## The Golden Rule of Agent Skill Architecture
> **"Never load the entire territory into the context window when the agent only needs a map."**
Context window space and attention are scarce resources. Skills must prioritize **Progressive Disclosure** over exhaustive inline instructions to prevent attention dilution, "lost-in-the-middle" degradation, and unnecessary token overhead.

---

## 1. The Three-Tier Architecture of Progressive Disclosure

Every agent skill should strictly follow the open 3-tier hierarchy:

### Tier 1: Discovery (Metadata / Frontmatter)
- **File:** YAML frontmatter in `SKILL.md` (`name`, `description`, `allowed-tools`, `metadata`).
- **Function:** Serves as a **lightweight routing selector** (<100 tokens), pre-scanned by the agent at startup.
- **Rules:**
  - `name`: Matches the parent directory exactly (1-64 chars, lowercase alphanumeric + hyphens).
  - `description`: Written in the third person. Must act as a strict selector answering:
    - **What** capability the skill provides.
    - **When to activate** (positive trigger phrases/keywords).
    - **When NOT to use** (negative triggers / scope boundaries).
  - *Never put implementation instructions or manuals in the description.*

### Tier 2: Activation (`SKILL.md`)
- **File:** Root `SKILL.md` (loaded into context ONLY upon activation).
- **Function:** Defines high-level procedural workflow, step-by-step phases, safety gates, and exit criteria.
- **Rules:**
  - Keep under **500 lines / ~5,000 tokens**.
  - Assume the underlying LLM is competent: avoid boilerplate explanations of general programming concepts.
  - Focus strictly on domain-specific rules, sequence dependencies, and verification criteria.

### Tier 3: Execution & References (`references/`, `scripts/`, `assets/`)
- **Folder:** Subdirectories alongside `SKILL.md`.
- **Function:** Deep documentation and deterministic tooling loaded **strictly on demand**.
- **Rules:**
  - **Conditional File Pointers:** In `SKILL.md`, instruct the agent *when* to read reference files (e.g., `If inspecting legacy schema, read [references/legacy.md](./references/legacy.md)`).
  - **Deterministic Code over Probabilistic Prompts:** Offload brittle operations (API payloads, complex formatting, regex checks, batch processing) to executable scripts in `scripts/` instead of asking the LLM to format them in prose.

---

## 2. Meta-Skills & Skill Generation Conventions

When authoring or generating new agent skills (via meta-skills / skill creators):

1. **Ground on Real Traces (No Vacuum Authoring):**
   - Never generate a skill from pure generic LLM assumptions (prevents vague steps like "handle errors appropriately").
   - Extract skills from observed real developer workflows, actual PR diffs, bug RCAs, and real problem sessions.
2. **Evaluation-Driven Development (Eval-First):**
   - Benchmark the agent on a target task *without* the skill to observe exact failure points.
   - Author the minimal delta instructions required to overcome those specific failure points.
3. **Safety First (Dry-Run by Default):**
   - Any skill that mutates remote or shared state (Jira, GitHub, Notion, databases, production files) must default to a **dry-run mode**, presenting a structured diff/plan and requiring explicit user confirmation before executing write operations.
4. **Standard Open Directory Layout:**
   ```text
   <skill-name>/
   ├── SKILL.md            # Frontmatter metadata + procedural flow (<500 lines)
   ├── references/         # Lazy-loaded schemas, API payloads, domain glossaries
   ├── scripts/            # Deterministic Python/Bash helper scripts
   └── assets/             # Output templates, checklist skeletons
   ```

# QAgentic: Open-Source Agentic AI Skills for Quality Engineers

> **Free, platform-agnostic AI agent skills giving software QA engineers superpowers.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/Architecture-3--Tier%20Progressive%20Disclosure-blue.svg)](./AGENTS.md)
[![Status](https://img.shields.io/badge/Status-Active%20Open%20Source-green.svg)]()

---

## What is QAgentic?

**QAgentic** is a community-driven repository of **production-ready, battle-tested AI agent skills** crafted specifically for modern Quality Assurance Engineers, SDETs, and testing teams.

Instead of writing thousands of lines of generic prompts or losing control over context windows, QAgentic provides structured skills that follow the open **3-Tier Progressive Disclosure Architecture**. 

Every skill is:
- 🛡️ **Safe by Default:** Strict read-only contracts or dry-run mutation gates.
- ⚡ **Token-Efficient:** Lightweight metadata routing (<100 tokens), procedural workflows (<500 lines), and lazy-loaded deep documentation.
- 🌐 **Platform-Agnostic:** Works seamlessly across Claude Code, Gemini CLI, Antigravity IDE, Cursor, OpenCode, and any agent supporting open skill standards.
- 🆓 **100% Free & Open Source:** Forever free for the global QA community under the MIT license.

---

## 📦 The Skill Catalog

| Skill Name | Tier / Category | Description | Status |
| :--- | :--- | :--- | :--- |
| [**`qa-epic-scoping`**](./skills/qa-epic-scoping/) | Scoping & Discovery | Produces a 5–10 minute read-only **Epic QA Brief** detailing motivation, user flows, verbatim AC, affected surfaces, test data, and open questions before planning coverage. | ✅ Ready |
| [**`qa-ticket-readiness`**](./skills/qa-ticket-readiness/) | Quality Gates (DoR) | Audits user stories, bug reports, and QA tasks against the Definition of Ready (DoR), verifying AC completeness, testability, and edge cases with dry-run protection. | ✅ Ready |
| [**`playwright-code-review`**](./skills/playwright-code-review/) | Test Automation | Specialized static analysis for Playwright E2E suites on Git diffs or GitHub PRs, auditing POM encapsulation, test isolation, anti-flakiness, and locator resilience. | ✅ Ready |
| [**`qa-test-plan-generator`**](./skills/qa-test-plan-generator/) | Test Strategy | Automatically breaks down epics and stories into positive, negative, edge-case, boundary, and RBAC test matrices with pluggable TMS export (Qase, TestRail, Jira). | ✅ Ready |
| [**`qa-regression-retrospective`**](./skills/qa-regression-retrospective/) | Quality Intelligence | Audits escaped production bug clusters, performs Root Cause Analysis (RCA), evaluates whether fix PRs added regression-preventing tests, and drafts QA test tasks. | ✅ Ready |

---

## 🚀 Quickstart: Installing Skills in Your Agent

### 1. Claude Code / Antigravity IDE / Gemini CLI
To install a skill directly into your current workspace:

```bash
# Clone the skill directly into your project's agent configuration
mkdir -p .agents/skills
cp -r /path/to/qagentic/skills/qa-epic-scoping .agents/skills/
```

### 2. Cursor / Copilot Workspace
Copy the skill folder into `.cursor/skills/` or `.github/skills/`:
```bash
mkdir -p .cursor/skills
cp -r /path/to/qagentic/skills/qa-epic-scoping .cursor/skills/
```

---

## 🏛️ Skill Architecture: The Golden Rule

> **"Never load the entire territory into the context window when the agent only needs a map."**

All QAgentic skills follow a strict 3-tier hierarchy:
1. **Tier 1 (Discovery):** YAML frontmatter in `SKILL.md` acts as a routing selector pre-scanned by the agent.
2. **Tier 2 (Activation):** The root `SKILL.md` contains the high-level step-by-step workflow (kept under 500 lines).
3. **Tier 3 (Execution):** The `references/` directory holds dense templates, schemas, and verification rules loaded **strictly on demand**.

Read our full specification in [**`AGENTS.md`**](./AGENTS.md).

---

## 🤝 Contributing

We welcome contributions from QA engineers across the realm! Whether you want to add an adapter for another issue tracker, contribute test authoring skills for Cypress/Selenium, or propose a new quality gate:

1. Fork the repository.
2. Create your skill branch (`git checkout -b skill/my-new-skill`).
3. Follow the 3-Tier Progressive Disclosure rules in [AGENTS.md](./AGENTS.md).
4. Verify your skill using our [Validation Rubric](./skills/qa-epic-scoping/references/SKILL-VALIDATION.md).
5. Open a Pull Request!

---

## 📄 License

Distributed under the MIT License. See [LICENSE](./LICENSE) for more information.

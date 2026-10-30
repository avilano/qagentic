# Definition of Ready Assessment Report Template

Use this format when rendering the readiness report in dry-run or post-write mode.

```markdown
# 📋 QA Definition of Ready Report: [{KEY}] {SUMMARY}

**Tracker:** {Tracker Name} ({URL})  
**Assigned Grade:** **`Grade {GRADE}`** ({Grade Meaning})  
**Evaluation Mode:** {Dry-Run | Post-Write Verified}  
**Evaluator:** QA Agentic Auditor  

---

## 🎯 Executive Summary
{1-2 paragraphs summarizing ticket state, DoR readiness, and whether work can proceed immediately.}

---

## 📊 Quality Gate Breakdown

### Tier 1: Core DoR (Must-Have Gates)
| Gate | Check | Status | Evidence / Notes |
| :--- | :--- | :---: | :--- |
| **T1.1** | Problem Statement | ✅ PASS / ❌ FAIL | {Evidence quote or missing reason} |
| **T1.2** | Acceptance Criteria | ✅ PASS / ❌ FAIL | {Number of ACs found or gap noted} |
| **T1.3** | Scope Boundaries | ✅ PASS / ⚠️ WARN | {In-scope vs out-of-scope status} |
| **T1.4** | Testability | ✅ PASS / ❌ FAIL | {Reproduction/verification steps note} |

### Tier 2: Quality & Automation Readiness
| Gate | Check | Status | Evidence / Notes |
| :--- | :--- | :---: | :--- |
| **T2.1** | Negative & Edge Cases | ✅ PASS / ⚠️ MISSING | {Documented error conditions} |
| **T2.2** | Test Data & Roles | ✅ PASS / ⚠️ MISSING | {Roles/data prerequisites} |
| **T2.3** | Feature Flags & Config | ✅ PASS / ℹ️ N/A | {Flag keys identified} |
| **T2.4** | UI / Figma Artifacts | ✅ PASS / ℹ️ N/A | {Design link status} |
| **T2.5** | Regression Awareness | ✅ PASS / ⚠️ MISSING | {Adjacent impact areas} |

---

## 🔍 Identified Gaps & Ambiguities
- **[CRITICAL / MINOR]:** {Description of specific ambiguity or missing acceptance criteria.}
- **[CRITICAL / MINOR]:** {Missing test environment or seed data dependency.}

---

## 💡 Recommended Actions & Next Steps
- [ ] {Specific action item for Product Owner or Developer}
- [ ] {Recommended addition to Acceptance Criteria}
- [ ] {Suggested test scenario for QA Test Plan}

---

## 📝 Proposed Ticket Enrichment (If Write Mode Requested)
{Unified diff or proposed description markdown for user preview}
```

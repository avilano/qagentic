# Issue Tracker Adapters

The `qa-epic-scoping` skill is designed to work across any project management system or issue tracker. Use this guide to resolve and query the target epic or feature based on the available tools and environment.

---

## 1. Atlassian Jira Adapter

### Tool Discovery
Look for available Jira MCP tools (e.g. `mcp__jira__*`, `mcp__Atlassian-Jira__*`, `mcp__plugin-atlassian-atlassian__*`).

### Resolution & Querying Steps
1. **Fetch Issue:**
   - Retrieve issue by key: `getJiraIssue(issueKey: "{KEY}")`.
   - Always expand fields to inspect changelog, comments, and custom fields:
     `expand: "renderedFields,names,schema,operations,changelog"`.
2. **Determine Hierarchy:**
   - If issue type is not an Epic, inspect the `parent` or `epic link` field to find the parent Epic key.
3. **Fetch Child Stories:**
   - Run JQL search via `searchJiraIssuesUsingJql`:
     `jql: "'Epic Link' = {EPIC_KEY} OR parent = {EPIC_KEY} ORDER BY key ASC"`.
   - Paginate if child count exceeds standard limits (maxResults: 50/100).
4. **Extract AC:**
   - Check standard `description`, custom field `customfield_*` (often named "Acceptance Criteria"), and checklist plugins.

---

## 2. GitHub Issues & Projects Adapter

### Tool Discovery
Look for GitHub MCP tools (e.g. `mcp__github__*`) or the native GitHub CLI `gh` via bash.

### Resolution & Querying Steps
1. **Fetch Milestone or Tracking Issue:**
   - Via CLI: `gh issue view {ISSUE_NUMBER} --json title,body,comments,milestone`
   - If using a Milestone: `gh issue list --milestone "{MILESTONE_TITLE}" --json number,title,body,state,labels`
2. **Determine Child Tasks:**
   - If the issue is an Epic/Tracking Issue, parse task lists in markdown:
     `- [ ] #123 (Child Story 1)`
   - Fetch child issues to extract detailed acceptance criteria.
3. **Extract AC:**
   - Parse markdown headers in bodies: `### Acceptance Criteria` or task lists `- [ ]`.

---

## 3. Linear Adapter

### Tool Discovery
Look for Linear MCP tools (e.g. `mcp__linear__*`).

### Resolution & Querying Steps
1. **Fetch Project or Parent Issue:**
   - Search for the project or parent issue identifier.
2. **Fetch Child Issues:**
   - Query all issues assigned to the project or having `parentId = {PARENT_ID}`.
3. **Extract AC:**
   - Extract description and project brief content.

---

## 4. Local Markdown Spec / PRD Adapter (Offline Mode)

If no cloud issue tracker MCP is configured, the skill can run against local documentation files:

### Resolution & Querying Steps
1. **Locate Spec File:**
   - Search workspace for PRDs or specs (e.g. `docs/specs/*.md`, `docs/rfcs/*.md`).
2. **Parse Structure:**
   - Extract Problem Statement, Proposed User Experience, Acceptance Criteria, and Open Questions directly from markdown headings.
3. **Proceed with Codebase Scout:**
   - Map the spec requirements to workspace source code as usual.

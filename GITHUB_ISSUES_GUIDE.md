# GitHub Issues & Project Board Guide
## Repo: `RvxAI345/Lesson` · Project: #4 "TRW Lesson"

> This file is to be loaded at the start of every AI dev agent chat session.
> Its purpose is to enforce consistent GitHub issue tracking and project board hygiene across all work sessions.

---

## 🧭 Core Principle

**Every meaningful unit of work must exist as a GitHub Issue linked to the TRW Lesson project board.**
No code change, feature, fix, or milestone is considered "tracked" unless it has a corresponding issue that is up to date.

---

## 🔧 MCP Tools to Use

All GitHub interactions must go through the **GitHub MCP server**. Never ask the user to update issues manually.

| Task | MCP Tool |
|------|----------|
| Create an issue | `issue_write` |
| Read / search issues | `issue_read`, `list_issues`, `search_issues` |
| Add a comment to an issue | `add_issue_comment` |
| List project items | `projects_list` → `list_project_items` |
| Get project details | `projects_get` → `get_project` |
| Update project item fields | `projects_write` |

---

## 📋 Project Board Reference

| Property | Value |
|----------|-------|
| **Project title** | TRW Lesson |
| **Project number** | 4 |
| **Owner** | RvxAI345 |
| **Node ID** | `PVT_kwHODlryQs4Bqo2J` |
| **Repo** | `RvxAI345/Lesson` |

### Status Field (ID: `440341858` · Node: `PVTSSF_lAHODlryQs4Bqo2Jzho_FWI`)

| Status | Option ID | When to use |
|--------|-----------|-------------|
| `Todo` | `f75ad846` | Issue created, not yet started |
| `In progress` | `47fc9ee4` | Work has begun on this issue |
| `Done` | `98236657` | Issue is fully completed and closed |

### Team Field (ID: `440342515` · Node: `PVTSSF_lAHODlryQs4Bqo2Jzho_F_M`)

| Team | Option ID |
|------|-----------|
| `Squad 1` | `9282166a` |
| `Squad 2` | `8a5d08e5` |
| `Squad 3` | `478d0b17` |

### Iteration Field (ID: `440342516` · Node: `PVTIF_lAHODlryQs4Bqo2Jzho_F_Q`) — 2-week sprints

| Iteration | ID | Dates |
|-----------|-----|-------|
| Iteration 1 | `381c7c80` | Nov 7 – Nov 20, 2026 |
| Iteration 2 | `54cf5c95` | Nov 21 – Dec 4, 2026 |
| Iteration 3 | `d2c335bc` | Dec 5 – Dec 18, 2026 |

### Quarter Field (ID: `440342517` · Node: `PVTIF_lAHODlryQs4Bqo2Jzho_F_U`) — 90-day cycles

| Quarter | ID | Dates |
|---------|----|-------|
| Quarter 1 | `5f6bfd38` | Nov 7, 2026 – Feb 4, 2027 |
| Quarter 2 | `b64ce631` | Feb 5 – May 5, 2027 |
| Quarter 3 | `a0844f8e` | May 6 – Aug 3, 2027 |

### Date Fields

| Field | ID | Node ID |
|-------|----|--------|
| Start date | `440342518` | `PVTF_lAHODlryQs4Bqo2Jzho_F_Y` |
| Target date | `440342519` | `PVTF_lAHODlryQs4Bqo2Jzho_F_c` |

### Views

| View | Layout | Purpose |
|------|--------|---------|
| Monthly roadmap | Roadmap | Month-by-month timeline |
| Quarterly roadmap | Roadmap | Quarter-by-quarter timeline |
| Backlog | Table | Full list of all items with all fields |

---

## ✅ Agent Workflow: Session Start

At the **beginning of every chat session**, the agent must:

1. **List open issues** to understand current state:
   ```
   tool: list_issues
   params: { owner: "RvxAI345", repo: "Lesson", state: "open" }
   ```

2. **List project board items** to see what's on the board:
   ```
   tool: projects_list → list_project_items
   params: { method: "list_project_items", owner: "RvxAI345", project_number: 4, field_names: ["Status", "Iteration", "Quarter"] }
   ```

3. **Confirm with the user** what they want to work on before proceeding.

---

## ✅ Agent Workflow: Creating a New Issue

When starting new work, the agent must:

1. **Create the issue** in the repo:
   ```
   tool: issue_write
   params: {
     owner: "RvxAI345",
     repo: "Lesson",
     title: "<clear, action-oriented title>",
     body: "<description of the work, acceptance criteria, and context>",
     labels: ["<relevant label>"]
   }
   ```

2. **Add the issue to the project board** using `projects_write` with the issue's node ID.

3. **Set the Status** to `Todo` (option ID: `f75ad846`).

4. **Set Iteration and Quarter** based on when the work is expected.

5. **Set Start date and Target date** if known.

---

## ✅ Agent Workflow: During Work

As work progresses, the agent must:

- **Update Status to `In progress`** (option ID: `47fc9ee4`) when work begins on an issue.
- **Add a comment** to the issue summarizing:
  - What was done in this session
  - Any blockers or decisions made
  - What remains to be done

---

## ✅ Agent Workflow: Completing Work

When a task is finished, the agent must:

1. **Update Status to `Done`** (option ID: `98236657`) on the project board.
2. **Close the issue**:
   ```
   tool: issue_write
   params: { owner: "RvxAI345", repo: "Lesson", issue_number: <N>, state: "closed" }
   ```
3. **Add a closing comment** summarizing what was delivered.

---

## ✅ Agent Workflow: Milestone / Checkpoint Updates

Whenever a significant milestone is reached (feature complete, sprint end, deploy, etc.):

1. Review all issues currently `In progress` — confirm if any can move to `Done`.
2. Review `Todo` items — confirm if priorities have changed.
3. Add a comment to relevant issues noting the milestone reached.
4. Report a brief summary to the user before ending the session.

---

## 📝 Issue Writing Standards

Every issue body must follow this template:

```markdown
## Context
<Why this work is needed>

## Goal
<What done looks like — be specific>

## Tasks
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

## Notes
<Links, decisions, constraints, or references>
```

### Labels to Use (create if missing)

| Label | Purpose |
|-------|---------|
| `feature` | New functionality |
| `bug` | Something broken |
| `chore` | Maintenance, refactoring, config |
| `milestone` | Tracks a major deliverable |
| `blocked` | Cannot proceed, waiting on something |
| `question` | Needs clarification before work begins |

---

## ⚠️ Rules the Agent Must Never Break

1. **Never start coding without a linked issue.** Create the issue first.
2. **Never close a session without updating issue statuses.**
3. **Never leave an issue `In progress` without a comment** explaining the current state.
4. **Always confirm with the user** before closing an issue as `Done`.
5. **Never create duplicate issues** — always search first with `list_issues` or `search_issues`.

---

## 🔁 Quick Reference: Common MCP Calls

### List all open issues
```json
{ "tool": "list_issues", "owner": "RvxAI345", "repo": "Lesson", "state": "open" }
```

### Create an issue
```json
{ "tool": "issue_write", "owner": "RvxAI345", "repo": "Lesson", "title": "...", "body": "..." }
```

### Add a comment
```json
{ "tool": "add_issue_comment", "owner": "RvxAI345", "repo": "Lesson", "issue_number": N, "body": "..." }
```

### List project board items
```json
{ "tool": "projects_list", "method": "list_project_items", "owner": "RvxAI345", "project_number": 4, "field_names": ["Status", "Iteration", "Quarter", "Start date", "Target date"] }
```

### Update a project item field (e.g., set Status to In Progress)
```json
{ "tool": "projects_write", "owner": "RvxAI345", "project_number": 4, "item_id": "<item_node_id>", "field_id": "PVTSSF_lAHODlryQs4Bqo2Jzho_FWI", "value": { "single_select_option_id": "47fc9ee4" } }
```

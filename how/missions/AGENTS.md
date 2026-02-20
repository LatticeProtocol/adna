---
type: directory_index
created: 2026-02-17
updated: 2026-02-19
last_edited_by: agent_stanley
tags: [directory_index, missions]
---

# Missions — Agent Protocol

## Purpose

Multi-session plans for tasks too large to complete in a single agent session. Plans provide decomposition, continuity across sessions, coordination between agents, and accountability.

## Directory Structure

```
how/missions/
├── AGENTS.md                    # This file (protocol)
└── plan_{name}.md               # One file per plan
```

Plans with deliverables MAY use subdirectories:
```
how/missions/{plan_slug}/
├── plan_{slug}.md               # Master plan
├── deliverable_a.md
└── deliverable_b.md
```

## Plan File Format

**Filename**: `plan_{short_name}.md` (underscores, no hyphens)

Template: `how/templates/template_mission.md`

## Lifecycle

### Creating a plan
1. Identify a task too large for one session
2. Create `plan_{name}.md` with decomposed tasks
3. Set `status: active`
4. Log the plan creation in the current session file

### Claiming a task
1. Read the plan file
2. Find the next unclaimed task whose dependencies are met
3. Set the task `status: in_progress` and `session: {session_id}`
4. Begin work

### Completing a task
1. Finish the task
2. Set `status: completed` in the plan file
3. Record files touched in both the plan task entry and the session file
4. Check if the next task can now be claimed

### Completing a plan
1. When all tasks are `completed` (or `skipped` with justification)
2. Set plan `status: completed` and update `updated` date
3. Plan file stays in `how/missions/` as a historical record

### Abandoning a plan
1. If a plan is no longer relevant, set `status: abandoned`
2. Add a note explaining why
3. Do not delete — keep for audit trail

## Rules

- **One owner per task at a time** — only one session should have a task `in_progress`
- **Check before claiming** — read the plan file immediately before setting `in_progress` (collision prevention)
- **Dependencies are hard** — do not start a task until all its `depends on` tasks are `completed`
- **Plans are living documents** — add tasks, reorder, or skip tasks as needed during execution
- **Keep tasks session-sized** — each task should be completable in roughly one agent session

## Load/Skip Decision

**Load this directory when**:
- Creating a new mission for a task too large for a single session
- Claiming the next objective in an active mission (startup checklist step 8)
- Checking mission status to understand what work is in progress or completed
- Closing a mission — updating status, writing completion summary

**Skip when**:
- Working within a campaign mission (those live in `how/campaigns/campaign_<name>/missions/`)
- Performing routine session work not tied to a mission
- Already know the mission file path and can load it directly

**Token cost**: ~600 tokens (this AGENTS.md)

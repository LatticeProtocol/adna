---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, ops]
---

# how/ — Operations (Agent Reference)

## Purpose

`how/` holds operations — the **HOW** of this project. Plans, sessions, templates, and optionally backlog, pipelines, tasks, skills, processes, and deliverables all live here.

## Subdirectories

| Folder | Purpose | Status |
|--------|---------|--------|
| `missions/` | Build plans for multi-session work | Required |
| `sessions/` | Session tracking and audit trail | Required |
| `templates/` | Reusable templates for all content types | Required |
| `backlog/` | Durable ideation and improvement tracking | Active |
| `campaigns/` | Multi-mission strategic initiatives | Active |
| `pipelines/` | Content-as-code folder workflows | Active |
| `deliverables/` | Output artifacts from pipelines | Active |

## Key Distinctions

| Type | What It Is | Execution Model |
|------|-----------|----------------|
| **Mission** | Multi-session project decomposed into tasks | Agent sessions with handoff |
| **Campaign** | Multi-mission strategic initiative | Phased execution with user gates |
| **Pipeline** | Folder-based workflow where file location = state | Automated / agent-driven |

## Naming

All files follow `type_descriptive_name.md` (underscores only, never hyphens).

## Cross-References

- [how/missions/AGENTS](how/missions/AGENTS.md) — Mission protocol
- [how/sessions/AGENTS](how/sessions/AGENTS.md) — Session lifecycle and tracking
- [how/backlog/AGENTS](how/backlog/AGENTS.md) — Backlog and ideation protocol
- [how/campaigns/AGENTS](how/campaigns/AGENTS.md) — Campaign protocol
- [how/pipelines/AGENTS](how/pipelines/AGENTS.md) — Pipeline paradigm and index
- [what/AGENTS](what/AGENTS.md) — Knowledge objects (WHAT)
- [who/AGENTS](who/AGENTS.md) — Organization (WHO)

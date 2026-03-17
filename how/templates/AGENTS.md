---
type: directory_index
created: 2026-02-19
updated: 2026-03-17
last_edited_by: agent_stanley
tags: [directory_index, templates]
---

# how/templates/ — Reusable Templates

## Purpose

Templates for all content types in the vault. Each template defines the frontmatter schema, body structure, and naming convention for its target entity type. Templates ensure consistency across files created by different agents and users.

## Template Index

| Template | Target Type | Auto-Trigger Directory | Frontmatter `type` |
|----------|------------|----------------------|-------------------|
| `template_adr.md` | Architecture Decision Record | `what/decisions/` | `decision` |
| `template_backlog.md` | Backlog idea | `how/backlog/` | `backlog` |
| `template_campaign.md` | Campaign master document | `how/campaigns/` | `campaign` |
| `template_context.md` | Context library file | `what/context/` | `context_research` / `context_guide` / `context_core` |
| `template_coordination.md` | Coordination note | `who/coordination/` | `coordination` |
| `template_mission.md` | Mission / plan | `how/missions/` | `mission` |
| `template_prd.md` | Product Requirements Document | `how/pipelines/prd_rfc/` | `prd` |
| `template_rfc.md` | Request for Comments | `how/pipelines/prd_rfc/` | `rfc` |
| `template_session.md` | Session tracking file | `how/sessions/` | `session` |
| `template_skill.md` | Skill recipe or procedure | `how/skills/` | `skill` |
| `template_data_record.md` | Generic data record | — | varies |
| `template_governance.md` | Governance document | — | `governance` |
| `template_folder_note.md` | Folder index note | — | `folder_note` |
| `template_registry.md` | Registry publication metadata | — | `template` |
| `template_aar.md` | After-Action Review | `how/missions/artifacts/` | `artifact` |

## Filled Examples

See `examples/` for completed samples showing what quality output looks like:

| Example | Based on template | Shows |
|---------|------------------|-------|
| `example_context_market_analysis.md` | `template_context.md` | Filled context file with real research, competitive analysis, and anti-patterns |
| `example_mission_product_launch.md` | `template_mission.md` | Completed mission with 5 tasks, status tracking, and completion summary |
| `example_session_customer_research.md` | `template_session.md` | Completed session with activity log, SITREP, and next session prompt |

Use these to understand the expected level of detail before creating your own files.

## Usage Notes

- **Templater integration**: If using Obsidian with the Templater plugin, templates can auto-populate when creating files in their trigger directory
- **Frontmatter first**: Always fill the YAML frontmatter before writing body content — frontmatter drives type detection and search
- **Extend, don't fork**: If you need a variant, add optional fields to the existing template rather than creating a copy
- **`type` field is mandatory**: Every file created from a template must retain the `type` frontmatter field — it's the entity discriminator

## Load/Skip Decision

**Load this directory when**:
- Creating a new file and unsure which frontmatter fields or body structure to use
- Onboarding and learning what entity types the vault supports
- Modifying template conventions across the vault (Tier 2 session recommended)

**Skip when**:
- Already know which template to use and remember the required fields
- Working with existing files that already have correct frontmatter
- Performing non-creation tasks (reading, analyzing, searching)

**Token cost**: ~400 tokens (this AGENTS.md). Individual templates are ~50-150 lines each.

## Cross-References

- [how/AGENTS](../AGENTS.md) — Operations layer index
- [what/context/AGENTS](../../what/context/AGENTS.md) — Context library (uses `template_context.md`)
- [how/sessions/AGENTS](../sessions/AGENTS.md) — Session protocol (uses `template_session.md`)
- [how/missions/AGENTS](../missions/AGENTS.md) — Mission protocol (uses `template_mission.md`)
- [how/campaigns/AGENTS](../campaigns/AGENTS.md) — Campaign protocol (uses `template_campaign.md`)
- [how/skills/AGENTS](../skills/AGENTS.md) — Skills protocol (uses `template_skill.md`)

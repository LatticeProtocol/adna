---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, governance]
---

# who/governance/ — Governance (Agent Reference)

## Purpose

Team governance for this project. Defines roles, decision authority, and operational policies that govern how the project operates.

## Key Files

| File | Purpose |
|------|---------|
| `governance_roles.md` | Team members, responsibilities, and areas of ownership |
| `governance_decision_authority.md` | Decision authority matrix — who approves what category of change |
| `governance_policies.md` | Operational policies — collision prevention, naming, sessions, escalation |

Create governance files as the project matures. Start with roles and policies, add decision authority when the team grows.

## Conventions

- **Naming**: `governance_{topic}.md` (underscores only)
- **Frontmatter**: All files require `type: governance` plus base fields
- **Updates**: Changes to governance files should be coordinated — read before write, confirm with user if `updated` is recent
- **Scope**: Governance files define rules that all agents and users must follow. Treat as authoritative.

## Cross-References

- [CLAUDE.md](CLAUDE.md) — Master agent context (references governance rules)
- [who/coordination/AGENTS](who/coordination/AGENTS.md) — Cross-agent ephemeral notes
- [how/sessions/AGENTS](how/sessions/AGENTS.md) — Session protocol

---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, obs]
---

# what/ — Knowledge Layer (Agent Reference)

## Purpose

`what/` holds knowledge objects — the **WHAT** of this project. Context library, decisions, reference material, and domain-specific entities all live here.

## Subdirectories

| Folder | Purpose | Status |
|--------|---------|--------|
| `context/` | Agent context library — synthesized knowledge agents load before domain work | Required |
| `decisions/` | Architecture Decision Records — significant decisions and rationale | Recommended |
| `docs/` | Specification documents — aDNA standard, design doc, bridge patterns | Active |
| `lattices/` | Lattice YAML definitions, tools, schema, and examples | Active |

## Registry Pattern

what/ serves as a registry layer. Entries describe and link to objects — they do not duplicate source material. Each entry has frontmatter with type, status, and links to the implementation or source.

## Naming

All files follow `type_descriptive_name.md` (underscores only, never hyphens).

## Cross-References

- [what/context/AGENTS](what/context/AGENTS.md) — Context library protocol and topic index
- [what/lattices/AGENTS](what/lattices/AGENTS.md) — Lattice YAML tools and examples
- [how/AGENTS](how/AGENTS.md) — Operations (HOW)
- [who/AGENTS](who/AGENTS.md) — Organization (WHO)

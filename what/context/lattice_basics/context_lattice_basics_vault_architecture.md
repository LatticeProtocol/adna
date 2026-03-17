---
type: context_core
topic: lattice_basics
subtopic: vault_architecture
created: 2026-03-05
updated: 2026-03-05
sources: ["adna CLAUDE.md", "adna MANIFEST.md"]
context_version: "1.0"
token_estimate: ~2000
last_edited_by: agent_stanley
tags: [context, lattice_basics]
quality_score: 3.0
signal_density: 3
actionability: 2
coverage_uniformity: 3
source_diversity: 1
cross_topic_coherence: 4
freshness_category: stable
last_evaluated: 2026-03-17
---

# Lattice Basics: Vault Architecture

## The Triad

Every aDNA vault follows the **who/what/how** triad:

| Directory | Domain | Contains |
|-----------|--------|----------|
| `who/` | People & organizations | Customers, partners, contacts, governance, coordination |
| `what/` | Knowledge & objects | Context library, decisions (ADRs), modules, lattices, datasets |
| `how/` | Operations & processes | Campaigns, missions, sessions, skills, pipelines, backlog, templates |

This structure maps to the organizational primitives: **who** you work with, **what** you build, and **how** you operate.

## Key Concepts

### Files as Records

Every markdown file is a **record** with YAML frontmatter. The frontmatter defines the record's type, status, dates, and relationships. The body contains human-readable content.

```yaml
---
type: customer
status: active
created: 2026-01-15
updated: 2026-03-01
---
```

### Wikilinks as Relationships

Obsidian's `[[wikilinks]]` create a knowledge graph. Links between records establish relationships: a customer links to their contacts, a mission links to its campaign, a module links to its lattice.

### Templates as Schema

Templates in `how/templates/` define the expected frontmatter and body structure for each record type. Templater auto-applies them when files are created in mapped directories.

### Sessions as Audit Trail

Every work session creates a file in `how/sessions/active/`. On completion, it moves to `how/sessions/history/YYYY-MM/`. This provides a complete audit trail of all vault modifications.

## Execution Hierarchy

```
Campaign → Mission → Objective
```

- **Campaigns** coordinate multiple missions toward a strategic goal
- **Missions** decompose into session-sized objectives
- **Objectives** are the atomic work units

## Configuration

Obsidian configuration lives in `.obsidian/`:
- **Plugins**: Community plugins extend functionality (Dataview, Templater, etc.)
- **Snippets**: CSS bundles customize the visual experience
- **Theme**: Tokyo Night with Rebecca Purple accent
- **Per-device files**: `workspace.json` and `graph.json` are machine-specific — don't sync them

## The `.obsidianignore`

Tells Obsidian's file watcher to skip paths entirely. Critical for vaults containing git repos or build artifacts — prevents event storms from thousands of irrelevant files.

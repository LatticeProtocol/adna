---
type: context_core
topic: lattice_basics
subtopic: vault_architecture
created: 2026-03-05
updated: 2026-03-18
sources: ["aDNA Standard v2.1 (what/docs/adna_standard.md)", "aDNA Design Document (what/docs/adna_design.md)", "adna CLAUDE.md", "adna MANIFEST.md", "template library (how/templates/AGENTS.md)"]
context_version: "2.0"
token_estimate: ~1000
last_edited_by: agent_stanley
tags: [context, lattice_basics]
quality_score: 4.0
signal_density: 4
actionability: 4
coverage_uniformity: 4
source_diversity: 3
cross_topic_coherence: 4
freshness_category: stable
last_evaluated: 2026-03-18
---

# Lattice Basics: Vault Architecture

## The Triad

Every aDNA vault follows the **who/what/how** triad:

| Directory | Domain | Contains | Example Subdirs |
|-----------|--------|----------|----------------|
| `who/` | People & organizations | Team, governance, coordination, optionally customers/partners/contacts | `team/`, `governance/`, `coordination/` |
| `what/` | Knowledge & objects | Context library, decisions, modules, lattices, datasets, docs | `context/`, `decisions/`, `lattices/`, `docs/` |
| `how/` | Operations & processes | Campaigns, missions, sessions, skills, pipelines, backlog, templates | `campaigns/`, `missions/`, `sessions/`, `skills/` |

**The Question Test** classifies any content: "Who is involved?" → `who/`. "What do we know?" → `what/`. "How do we operate?" → `how/`.

## First 5 Things to Do

After cloning and opening the vault in Obsidian:

1. **Read `CLAUDE.md`** — understand the project identity, safety rules, and agent protocol
2. **Edit `MANIFEST.md`** — replace the project description with your own
3. **Edit `STATE.md`** — set your current phase, priorities, and next steps
4. **Browse `how/templates/`** — see what record types are available (17 templates)
5. **Create your first record** — pick the most relevant directory (e.g., `who/team/` or `what/decisions/`) and create a file. The Templater plugin auto-populates the correct template.

## Files as Records

Every Markdown file is a **record** with YAML frontmatter. The frontmatter defines the record's type, status, dates, and relationships. The body contains human-readable content.

```yaml
# Example: a team member record in who/team/
---
type: team_member
name: "Alex Chen"
role: "Research Scientist"
status: active
created: 2026-03-01
updated: 2026-03-18
last_edited_by: agent_stanley
tags: [team]
---

# Alex Chen — Research Scientist
...
```

```yaml
# Example: an architecture decision in what/decisions/
---
type: decision
adr_id: adr_001
title: "Choice of Obsidian as Knowledge Platform"
status: accepted
created: 2026-03-18
updated: 2026-03-18
last_edited_by: agent_stanley
tags: [adr, decision]
---

# ADR-001: Choice of Obsidian as Knowledge Platform
...
```

**Key fields**: `type` (entity discriminator), `status` (lifecycle), `created`/`updated` (temporal), `last_edited_by` (audit trail), `tags` (search).

## Wikilinks as Relationships

Obsidian's `[[wikilinks]]` create a knowledge graph. Links between records establish relationships:
- A customer links to their contacts: `[[contact_jane_smith]]`
- A mission links to its campaign: `[[campaign_alpha_readiness]]`
- A module links to its lattice: `[[lattice_protein_binder_design]]`

Obsidian auto-updates links when files are renamed.

## Templates as Schema

Templates in `how/templates/` define the expected frontmatter and body structure for each record type. 10 templates have **auto-triggers** via Templater — creating a file in the mapped directory populates the template automatically:

| Directory | Template Applied |
|-----------|-----------------|
| `what/decisions/` | ADR template |
| `what/context/` | Context file template |
| `how/sessions/active/` | Session template |
| `how/skills/` | Skill template |
| `how/backlog/` | Backlog idea template |

Full trigger table: `how/templates/AGENTS.md`. 7 additional templates are applied manually via `Templater: Insert template`.

## Execution Hierarchy

For work larger than a single session:

```
Campaign → Phase → Mission → Objective
```

- **Campaigns** — strategic initiatives, weeks to months. Files in `how/campaigns/`.
- **Phases** — logical groupings within a campaign, with human gates between them.
- **Missions** — multi-session tasks with defined objectives. Files in `how/missions/` or `how/campaigns/campaign_*/missions/`.
- **Objectives** — session-sized work units. Completed in a single session.

Sessions create audit trail files in `how/sessions/active/`, archived to `how/sessions/history/YYYY-MM/` on completion.

## Governance Files

Five files provide the structural skeleton:

| File | Audience | Scope |
|------|----------|-------|
| `CLAUDE.md` | Agents (auto-loaded) | Global — project structure, safety, protocol |
| `MANIFEST.md` | Both | Global — project identity, architecture |
| `STATE.md` | Both | Global — current plans, blockers, next steps |
| `AGENTS.md` | Agents | Per-directory — one in every directory |
| `README.md` | Humans | Global — GitHub, onboarding |

Agents read: `CLAUDE.md` → `STATE.md` → directory `AGENTS.md`. Humans browse: `README.md` → `MANIFEST.md` → triad.

## Configuration

Obsidian configuration lives in `.obsidian/`:
- **Plugins** (13): Community plugins, 3 tiers (Essential/Recommended/Optional). See `.obsidian/OBSIDIAN_CLAUDE.md`.
- **Snippets** (2 CSS bundles): `core_theme.css` (visual identity) + `editor_polish.css` (editor enhancements)
- **Theme**: Tokyo Night with Rebecca Purple (`#663399`) accent
- **Per-device files**: `workspace.json` and `graph.json` are machine-specific (gitignored)
- **`.obsidianignore`**: Tells the file watcher to skip git repos and build artifacts

## Sources

- aDNA Standard v2.1 (`what/docs/adna_standard.md`) — normative specification
- aDNA Design Document (`what/docs/adna_design.md`) — architecture rationale
- `CLAUDE.md` — governance file reference
- `MANIFEST.md` — project identity template
- Template library (`how/templates/AGENTS.md`) — 17 templates, trigger table

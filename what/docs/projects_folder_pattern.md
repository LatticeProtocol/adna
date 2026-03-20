---
type: context
title: "aDNA Projects Folder Pattern"
created: 2026-03-19
updated: 2026-03-19
status: active
last_edited_by: agent_stanley
tags: [adna, projects, scaffolding, workspace, pattern]
token_estimate: 1500
---

# aDNA Projects Folder Pattern

A workspace-level pattern for managing multiple aDNA-structured projects. Instead of isolated vaults, projects share a common template and are scaffolded through an agent-guided interview.

---

## The Pattern

```
adna-projects/                    # Workspace root (users copy this)
├── CLAUDE.md                     # Meta-governance — interview + scaffold instructions
├── .base/                        # Base template (fork source, not a project)
│   ├── CLAUDE.md.template        # Governance template with {{variables}}
│   ├── MANIFEST.md.template
│   ├── STATE.md.template
│   ├── AGENTS.md.template        # Root agent guide
│   ├── README.md.template
│   ├── who_AGENTS.md.template    # Triad-level agent guides
│   ├── what_AGENTS.md.template
│   └── how_AGENTS.md.template
├── my-research-vault/            # Project A (scaffolded)
│   ├── CLAUDE.md
│   ├── MANIFEST.md
│   ├── ...
├── client-data-pipeline/         # Project B (scaffolded)
│   └── ...
└── shared/                       # Optional cross-project context
    └── context/                  # Reusable context files
```

A working example lives at `examples/adna-projects/` with two pre-scaffolded projects (biotech lab, enterprise pipeline).

---

## Design Principles

### 1. The CLAUDE.md is the scaffolding engine

The root `CLAUDE.md` is NOT a project CLAUDE.md. It contains meta-instructions that tell the agent how to:
1. Run the 5-question pre-flight interview
2. Select a skeleton tier based on team size
3. Apply domain extensions based on the user's domain
4. Copy and resolve `.base/` templates into a new project directory

When the Start Kit CLI eventually ships, it automates what this pattern does manually. Until then, the agent IS the scaffolding tool.

### 2. Each project is self-contained

Every scaffolded project is a complete aDNA vault: own CLAUDE.md, own git (optional), own triad structure. Projects don't depend on the workspace root to function. You can move a project out of the workspace and it still works.

### 3. Templates use variable syntax

`.base/` templates use `{{variable}}` placeholders that map to interview answers:

| Variable | Source | Used in |
|----------|--------|---------|
| `{{project_name}}` | Directory name | CLAUDE.md, MANIFEST.md, README.md |
| `{{project_description}}` | Q1 answer | MANIFEST.md, CLAUDE.md, README.md |
| `{{domain}}` | Q2 answer | CLAUDE.md domain section |
| `{{skeleton_tier}}` | Q3 → tier mapping | Directory structure (controls which subdirs are created, not template text) |
| `{{tooling}}` | Q4 answer | .obsidian/ inclusion |
| `{{persona_name}}` | Q5 answer | CLAUDE.md personality |
| `{{persona_style}}` | Q5 answer | CLAUDE.md operating style |
| `{{created_date}}` | Scaffold date | All frontmatter |

### 4. Shared context is optional

The `shared/` directory is for organizations that want cross-project context reuse — common domain knowledge, shared templates, organizational standards. Solo users can ignore it entirely.

---

## Interview → Scaffold Flow

The 5-question interview (from the [Start Kit PRD](start_kit_prd.md)) drives all scaffolding decisions:

| Q | Question | Maps to |
|---|----------|---------|
| Q1 | What are you building? | `{{project_description}}` → MANIFEST.md, CLAUDE.md identity |
| Q2 | What's your domain? | Ontology extension suggestions (9 presets) |
| Q3 | Team or solo? | Skeleton tier (starter/standard/full per §5.4) |
| Q4 | How will you browse? | .obsidian/ inclusion, .claude/ config |
| Q5 | Agent personality? | CLAUDE.md personality section |

### Domain → Extension Mapping (Q2)

| Domain | Suggested extensions |
|--------|---------------------|
| **research** | `what/papers/`, `what/datasets/`, `what/hypotheses/`, `what/experiments/` |
| **enterprise** | `who/customers/`, `who/partners/`, `who/contacts/`, `who/projects/` |
| **biotech** | `what/experiments/`, `what/compounds/`, `what/protocols/`, `what/targets/` |
| **software** | `how/incidents/`, `how/deployments/`, `what/services/`, `what/apis/` |
| **creative** | `who/clients/`, `what/creative_assets/`, `how/revision_cycles/` |
| **personal** | `what/courses/`, `what/books/`, `how/learning_goals/` |
| **healthcare** | `who/patients/`, `what/treatments/`, `what/protocols/` |
| **legal** | `what/cases/`, `what/contracts/`, `what/compliance/` |
| **content** | `what/publications/`, `how/editorial_pipeline/`, `what/assets/` |

### Team Size → Skeleton Mapping (Q3)

| Team size | Skeleton | Key additions |
|-----------|----------|---------------|
| **solo** | Starter | Minimal governance. No coordination. |
| **small-team** (2-5) | Standard | `who/coordination/`, AGENTS.md chain, STATE.md, session tracking, backlog |
| **organization** (5+) | Full | Standard + `who/governance/`, collision prevention Tier 3, domain extensions |

---

## Relationship to Start Kit

| Concern | Projects Folder Pattern | Start Kit CLI |
|---------|------------------------|---------------|
| **What it is** | A workspace pattern | A CLI tool |
| **Scaffolding engine** | The agent (via CLAUDE.md) | The `adna` shell script |
| **When it ships** | Now (this doc + examples) | Later (`campaign_lattice_start_kit`) |
| **Template format** | `{{variable}}` placeholders | Same templates, resolved by script |
| **Interview** | Agent-guided conversation | Interactive `read`/`select` prompts |

The Projects Folder Pattern is the manual version of what the Start Kit automates. Same templates, same interview, same output — different execution engine.

---

## Usage

### Creating a new project

1. Open Claude Code in the `adna-projects/` directory
2. Say "Create a new project" (or similar)
3. The agent reads the root CLAUDE.md and runs the 5-question interview
4. Templates are copied from `.base/`, variables resolved with your answers
5. Domain extensions are created based on Q2
6. An onboarding session file is generated recording what was configured

### Customizing templates

Edit files in `.base/` to change what every new project starts with. Use `{{variable}}` syntax for values that should come from the interview.

### Sharing context across projects

Create `shared/context/` and add domain knowledge files. Reference them from individual project CLAUDE.md files using relative paths.

---

## Related

- [Start Kit PRD](start_kit_prd.md) — CLI tool design (interview, scaffolding, packaging)
- [Onboarding Skill](../../how/skills/skill_onboarding.md) — 10-step interactive flow (expanded version)
- [aDNA Standard §5.4](adna_standard.md) — Skeleton tier definitions
- [Migration Guide](migration_guide.md) — Adding aDNA to existing projects
- [Examples](examples/adna-projects/) — Working example with two pre-scaffolded projects

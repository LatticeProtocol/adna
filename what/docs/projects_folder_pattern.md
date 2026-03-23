---
type: context
title: "aDNA Projects Folder Pattern"
created: 2026-03-19
updated: 2026-03-22
status: active
last_edited_by: agent_stanley
tags: [adna, projects, scaffolding, workspace, pattern]
token_estimate: 2500
---

# aDNA Projects Folder Pattern

A workspace-level pattern for managing multiple aDNA-structured projects. Two approaches: the **Simplified Pattern** (recommended — uses the aDNA repo itself as the template) and the **Advanced Pattern** (uses a `.base/` directory with variable templates).

---

## Simplified Pattern (Recommended)

The aDNA repo **is** the template. Clone it once, then fork it for each new project. A workspace-level CLAUDE.md manages project creation and discovery.

```
~/Projects/                       # Workspace root (any folder works)
├── CLAUDE.md                     # Workspace architect (auto-created by skill_workspace_init.md)
├── adna/                         # Base template (git clone, never modified for projects)
│   ├── CLAUDE.md                 # Detects missing workspace CLAUDE.md, offers to create it
│   ├── prepare_for_onboarding.sh # Pre-flight checks for L1 upgrade
│   ├── setup.sh                  # Obsidian plugin bootstrap
│   └── how/skills/
│       ├── skill_workspace_init.md  # Creates the workspace CLAUDE.md
│       ├── skill_onboarding.md      # 5-question interview for new projects
│       └── skill_l1_upgrade.md      # L0→L1 phased compute upgrade
├── my_research_lab/              # Project A (forked from adna, customized)
│   ├── CLAUDE.md                 # Project-specific governance
│   └── ...
├── client_acme/                  # Project B (forked from adna)
│   └── ...
├── latlab/                       # (appears after L1 upgrade — not initially present)
└── lattice-protocol/             # (appears after L1 upgrade — not initially present)
```

### How it works

1. **Clone aDNA** into any folder: `git clone https://github.com/LatticeProtocol/adna.git`
2. **Run Claude Code** from inside `adna/` — the CLAUDE.md detects no workspace CLAUDE.md at the parent level and offers to create one via `skill_workspace_init.md`
3. **Create projects** — from the workspace level, ask the agent to create a new project. It copies `adna/`, strips `.git/` and `.obsidian/`, runs `git init`, then triggers the 5-question onboarding interview inside the new project
4. **Work inside projects** — each project is self-contained. Open it directly in Claude Code or Obsidian.
5. **Upgrade to L1** — follow `adna/how/skills/skill_l1_upgrade.md` to add JupyterHub compute

### Why this pattern

- **No `.base/` directory needed** — the full aDNA repo serves as the template
- **Every project gets the complete toolkit** — templates, skills, context library, lattice tools
- **Upstream updates** — `git pull` inside `adna/` to get latest aDNA improvements
- **Zero config** — the workspace CLAUDE.md is auto-generated on first run

### Design principles

1. **The agent is the scaffolding engine** — the workspace CLAUDE.md instructs Claude how to fork, customize, and seed new projects
2. **Each project is self-contained** — own CLAUDE.md, own git, own triad structure. Can be moved out of the workspace and still works.
3. **Never modify the template** — `adna/` stays as a clean reference. Only fork from it.
4. **Workspace CLAUDE.md governs workspace operations only** — project creation, discovery, L1 upgrade. Inside a project, that project's CLAUDE.md is authoritative.

### Related files

- [Workspace Init Skill](../../how/skills/skill_workspace_init.md) — creates the workspace CLAUDE.md
- [Workspace CLAUDE.md Template](templates/workspace_claude_md.template) — the template used by the skill
- [L1 Upgrade Skill](../../how/skills/skill_l1_upgrade.md) — phased compute upgrade

---

## Advanced Pattern (Custom Templates)

For organizations that want fine-grained control over project scaffolding, the `.base/` template pattern provides per-field customization.

```
adna-projects/                    # Workspace root
├── CLAUDE.md                     # Meta-governance — interview + scaffold instructions
├── .base/                        # Base template (fork source, not a project)
│   ├── CLAUDE.md.template        # Governance template with {{variables}}
│   ├── MANIFEST.md.template
│   ├── STATE.md.template
│   ├── AGENTS.md.template
│   ├── README.md.template
│   ├── who_AGENTS.md.template
│   ├── what_AGENTS.md.template
│   └── how_AGENTS.md.template
├── my-research-vault/            # Project A (scaffolded)
│   └── ...
└── shared/                       # Optional cross-project context
    └── context/
```

### When to use the advanced pattern

- You need custom template fields beyond what the onboarding interview provides
- Your organization has specific governance requirements that every project must include
- You want a `shared/` directory for cross-project context reuse
- You're building a domain-specific scaffolding system (e.g., all projects in your org must have `what/compliance/`)

### Template variable syntax

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

- [Workspace Init Skill](../../how/skills/skill_workspace_init.md) — Creates workspace CLAUDE.md (simplified pattern)
- [Workspace CLAUDE.md Template](templates/workspace_claude_md.template) — Template for workspace-level governance
- [Start Kit PRD](start_kit_prd.md) — CLI tool design (interview, scaffolding, packaging)
- [Onboarding Skill](../../how/skills/skill_onboarding.md) — 10-step interactive flow (expanded version)
- [L1 Upgrade Skill](../../how/skills/skill_l1_upgrade.md) — Phased L0→L1 compute upgrade
- [aDNA Standard §5.4](adna_standard.md) — Skeleton tier definitions
- [Migration Guide](migration_guide.md) — Adding aDNA to existing projects

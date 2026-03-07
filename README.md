# adna

**Give your project a knowledge architecture that both humans and AI agents can navigate.**

Every project accumulates knowledge — decisions, contacts, research, processes, context. Without structure, that knowledge fragments across chat logs, scattered docs, and individual memory. AI agents start every session cold, re-discovering what the project is and how it works.

**aDNA (Agentic DNA)** is a knowledge architecture that solves this. It organizes any project's knowledge into three directories — `who/`, `what/`, `how/` — with lightweight governance files that give AI agents instant orientation and give humans a browsable knowledge graph in [Obsidian](https://obsidian.md).

Clone this repo to get a ready-to-use aDNA vault with templates, tools, and examples. Customize it for your domain in minutes.

> *aDNA is a standalone knowledge architecture standard. It is the foundational building block of the [Lattice Protocol](https://github.com/LatticeProtocol/lattice-protocol) for federated AI compute — but the architecture is domain-neutral. Any project that uses AI agents (or just wants structured knowledge) benefits.*

---

## The Problem

AI agents start every session cold. No memory of past work, no awareness of other agents, no understanding of the project they're operating in. This creates four compounding failures:

| Failure | What happens |
|---------|-------------|
| **Orientation overhead** | Every session begins with the agent re-discovering project structure, conventions, and current state. Context windows fill with exploration instead of execution. |
| **Coordination failure** | Multiple agents (or humans + agents) work on the same project without shared protocol. They overwrite each other's work, duplicate effort, and produce inconsistent outputs. |
| **Knowledge fragmentation** | Insights discovered in one session are lost by the next. There's no persistent medium that accumulates project understanding across sessions and agents. |
| **Audience divergence** | Humans browse knowledge visually (folders, links, graphs). Agents parse knowledge programmatically (frontmatter, structured files, protocol files). Most projects serve one audience poorly or both terribly. |

aDNA solves these by giving projects a **deliberate knowledge architecture** — structured knowledge that both humans and agents can read, navigate, and build on.

---

## Who Is This For?

aDNA works for any project that manages knowledge. Here are four common use cases:

**Startup founder** — You're tracking investors, customers, product roadmap, hiring, and fundraising. Your `who/` leg holds investor profiles, customer records, and team members. Your `what/` leg holds product decisions, market research, and competitive analysis. Your `how/` leg holds fundraising campaigns, sprint plans, and onboarding processes. One vault replaces scattered Notion pages, Google Docs, and Slack threads.

**Researcher** — You're managing papers, datasets, experiments, and collaborations. Your `what/` leg holds literature reviews, dataset documentation, and methodology decisions. Your `who/` leg holds collaborators, lab members, and funding contacts. Your `how/` leg holds research missions, publication pipelines, and grant application workflows.

**Creative professional** — You're juggling clients, projects, creative assets, and revision cycles. Your `who/` leg holds client profiles and collaborator contacts. Your `what/` leg holds project briefs, asset libraries, and style guides. Your `how/` leg holds project workflows, revision pipelines, and delivery checklists.

**Personal knowledge manager** — You're organizing learning goals, reading notes, skills development, and personal projects. Your `what/` leg holds course notes, book summaries, and topic deep-dives. Your `how/` leg holds learning paths, habit tracking, and project plans. Your `who/` leg holds mentors, communities, and professional contacts.

Each of these users starts with the same base structure and extends it with domain-specific directories. See [Extending the Ontology](#extending-the-ontology) for how.

---

## The aDNA Paradigm

### The Triad

Every piece of project knowledge answers exactly one of three questions:

```
who/     →  Who is involved?                 (people, teams, organizations)
what/    →  What does this project know?     (knowledge, decisions, artifacts)
how/     →  How does this project work?      (processes, plans, operations)
```

This is the **triad** — three directories, three questions, complete coverage.

**The Question Test** classifies any content:

| Content | Question | Triad leg |
|---------|----------|-----------|
| A research paper summary | What do we know about X? | `what/` |
| The deployment checklist | How do we deploy? | `how/` |
| The engineering team roster | Who works on this? | `who/` |
| An architecture decision record | What did we decide? | `what/decisions/` |
| A campaign plan for Q2 launch | How do we execute Q2? | `how/campaigns/` |

Why three? Two legs create sorting ambiguity — people get conflated into either knowledge or process. Four or more legs were found unnecessary in practice; candidates like TOOLS, WHERE, and WHEN all decompose cleanly into the existing three. Three is the minimum that separates knowledge, process, and people without overlap.

### Two Deployment Forms

| Form | Structure | Use case |
|------|-----------|----------|
| **Bare triad** | `what/` · `how/` · `who/` at project root | Knowledge bases, Obsidian vaults, standalone projects |
| **Embedded triad** | `.agentic/what/` · `.agentic/how/` · `.agentic/who/` | Git repos where aDNA lives alongside source code |

This repo is a **bare triad** — the full aDNA experience. When adding aDNA to an existing codebase, use the embedded form so it doesn't collide with your source tree.

### Governance Files

Five files provide the structural skeleton:

| File | Audience | Purpose |
|------|----------|---------|
| **CLAUDE.md** | Agents | Master context — auto-loaded on session start. Project structure, safety rules, domain knowledge, agent protocol. |
| **MANIFEST.md** | Both | Project identity — what this project is, its architecture, entry points, active builds. |
| **STATE.md** | Both | Operational state — what's happening now, blockers, next steps. The live pulse of the project. |
| **AGENTS.md** | Agents | Per-directory guide — one in every directory telling agents what lives here and how to work with it. |
| **README.md** | Humans | Standard README — GitHub rendering, onboarding, external audience. |

Agents read `CLAUDE.md` → `STATE.md` → directory-level `AGENTS.md` files. Humans read `README.md` → `MANIFEST.md` → browse the triad. Same knowledge, two entry paths.

---

## Glossary

Key terms used throughout this documentation:

| Term | Definition |
|------|-----------|
| **aDNA** | Agentic DNA — a knowledge architecture standard that organizes project knowledge into three directories (who/what/how) with governance files for both human and AI agent navigation. |
| **Triad** | The three-directory structure (`who/`, `what/`, `how/`) at the heart of aDNA. Every piece of project knowledge belongs in exactly one leg. |
| **Ontology** | The set of entity types your project uses (e.g., customers, decisions, sessions). aDNA ships 14 base types; you extend with your own. |
| **Lattice** | A YAML-defined directed graph that models a workflow, pipeline, or reasoning process. Lattices are optional — the triad works without them. |
| **FAIR** | Findable, Accessible, Interoperable, Reusable — metadata principles applied to lattices so they can be shared and discovered. |
| **Convergence model** | The way aDNA's execution hierarchy (Campaign → Mission → Objective) progressively narrows context, reducing token count while increasing signal density. |
| **AGENTS.md** | A per-directory guide file that tells AI agents what lives in a directory and how to work with it. Every directory in an aDNA project has one. |

---

## An Ontology for Building Your Ontology

Base aDNA ships **14 entity types** across the triad — the minimal operational ontology any project needs:

| Triad leg | Entities | Purpose |
|-----------|----------|---------|
| **WHO** (3) | `governance`, `team`, `coordination` | Who decides, who works, how they sync |
| **WHAT** (4) | `context`, `decisions`, `modules`, `lattices` | What you know, what you've decided, what you build, how you compose |
| **HOW** (7) | `campaigns`, `missions`, `sessions`, `templates`, `skills`, `pipelines`, `backlog` | Plan → decompose → execute → track → automate → ideate |

**You extend by adding domain-specific entities under the appropriate triad leg.** The base gives you operational infrastructure — sessions, missions, coordination — that works from day one. Your extensions add the domain knowledge that makes the project yours.

Here are some examples of how different teams extend the base:

| Domain | Example extensions | Triad leg |
|--------|-------------------|-----------|
| **Startup / Business** | `investors`, `customers`, `partners`, `fundraising_pipeline` | `who/` |
| **Research / Science** | `experiments`, `datasets`, `hypotheses`, `protocols` | `what/` |
| **Software team** | `services`, `incidents`, `deployments` | `how/` |
| **Creative agency** | `clients`, `creative_assets`, `revision_cycles` | `who/` + `what/` |
| **Personal learning** | `courses`, `books`, `learning_goals` | `what/` |

The triad doesn't prescribe your domain — it gives you the scaffold to build your domain ontology on.

### The Discriminator Pattern

When related entities share a directory, use a frontmatter field to distinguish them:

```yaml
# what/modules/tool_lattice_validate.md
type: module
module_type: tool        # ← discriminator

# what/modules/model_protein_binder.md
type: module
module_type: model       # ← discriminator
```

This keeps the directory structure flat while preserving semantic precision. The base ontology uses three discriminators: `module_type` (tool, model, preprocessor, ...), `lattice_type` (pipeline, agent, context_graph, workflow), and `skill_type` (agent, process).

---

## What's Inside

```
adna/
├── CLAUDE.md                           # Agent master context
├── MANIFEST.md                         # Project identity (customize this)
├── STATE.md                            # Operational state (customize this)
├── what/                               # Knowledge
│   ├── context/                        #   Context library
│   ├── decisions/                      #   Architecture Decision Records
│   ├── docs/                           #   aDNA specification documents
│   │   ├── adna_standard.md            #     Full normative spec (v2.1)
│   │   ├── adna_design.md              #     Architecture rationale
│   │   └── adna_bridge_patterns.md     #     Multi-instance composition
│   └── lattices/                       #   Lattice definitions
│       ├── lattice_yaml_schema.json    #     JSON Schema
│       ├── canvas_yaml_interop.md      #     Canvas ↔ YAML spec
│       ├── examples/                   #     13 example lattices
│       └── tools/                      #     validate, convert, interop
├── how/                                # Operations
│   ├── templates/                      #   10 auto-triggering templates
│   ├── pipelines/prd_rfc/              #   R&D → PRD → RFC pipeline
│   ├── sessions/                       #   Session tracking
│   ├── campaigns/                      #   Multi-mission initiatives
│   ├── missions/                       #   Task decomposition
│   ├── skills/                         #   Agent recipes & procedures
│   └── backlog/                        #   Ideas & improvements
├── who/                                # Organization
│   ├── coordination/                   #   Cross-agent notes
│   └── governance/                     #   Project governance
└── .obsidian/                          # Visual config (theme, snippets, plugins)
```

| Component | Location | What it does |
|-----------|----------|-------------|
| **Lattice tools** | `what/lattices/tools/` | Validate `.lattice.yaml` files, convert to/from Obsidian canvas |
| **JSON Schema** | `what/lattices/lattice_yaml_schema.json` | Formal schema for lattice definitions |
| **13 example lattices** | `what/lattices/examples/` | Business, research, creative, personal, and biotech examples |
| **10 templates** | `how/templates/` | Session, mission, campaign, ADR, context, coordination, backlog, skill, PRD, RFC |
| **PRD/RFC pipeline** | `how/pipelines/prd_rfc/` | 4-stage content-as-code planning workflow |
| **aDNA spec docs** | `what/docs/` | Normative standard, design rationale, bridge patterns |
| **Obsidian config** | `.obsidian/` | Tokyo Night theme, CSS snippets, 11 pre-configured plugins (run `setup.sh`) |

---

## Quick Start

**Clone and open: ~5 minutes.** Agent-guided customization: 15-30 minutes. Manual customization: 30-45 minutes.

### Prerequisites

- [Obsidian](https://obsidian.md) 1.0+ (free, all platforms)
- Python 3.6+ (optional — only needed for lattice YAML validation/conversion tools)

### 1. Clone the repo

```bash
git clone https://github.com/LatticeProtocol/adna.git
cd adna
```

### 2. Install plugins and theme

```bash
./setup.sh
```

Downloads and installs 11 community plugins and the Tokyo Night theme. Run with `--force` to re-download everything.

Optionally install [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) font — the vault falls back to system fonts without it.

### 3. Open in Obsidian

Open `adna/` as a vault in [Obsidian](https://obsidian.md). Enable community plugins when prompted. The accent color (Rebecca Purple `#663399`) and CSS snippets activate automatically.

> **Visual learner?** Open `what/lattices/examples/hello_world.canvas` in Obsidian to see a lattice as an interactive node graph — no YAML knowledge needed. Drag nodes, follow edges, and explore how datasets, modules, and processes connect.

### 4. Choose your setup path

**Option A: Agent-guided setup (recommended, ~15-30 min)**

Open a terminal in the vault directory and start Claude Code:

```bash
claude
```

**Berthier** — the vault's built-in agent personality — will detect this is a fresh vault and walk you through interactive onboarding:

- Explains the aDNA architecture and triad structure
- Asks about your project, domain, and goals
- Suggests domain-specific ontology extensions
- Customizes governance files with your project identity
- Offers personality customization for your agent

This is the fastest way to go from clone to productive vault. Everything Berthier does is inspectable — see `how/skills/skill_onboarding.md`.

**Option B: Manual setup (~30-45 min)**

See [Using Without AI Agents](#using-without-ai-agents) below.

---

## Using Without AI Agents

aDNA works perfectly well as a human-only knowledge management system. The triad structure, templates, and Obsidian integration don't require AI agents at all.

### Manual setup in 5 steps

**1. Clone and install** — Follow Quick Start steps 1-3 above.

**2. Edit `MANIFEST.md`** — Replace the project description with your project's name and purpose. This file is your project's identity card — update it to describe what you're building.

**3. Edit `STATE.md`** — Replace the current phase and next steps with your own. This is your operational dashboard — use it to track what's happening now and what's next.

**4. Edit `CLAUDE.md` (optional)** — If you plan to use AI agents later, update the project description in the `## Identity & Personality` section. If not, you can ignore this file.

**5. Create your first content file** — Pick the triad leg that matches your first piece of knowledge and create a file:
   - A decision? Create `what/decisions/adr_your_decision.md`
   - A team member? Create `who/governance/team_member_name.md`
   - A project plan? Create `how/missions/plan_your_project.md`

Use the templates in `how/templates/` as starting points — each one has the required frontmatter fields pre-filled.

### What you can skip for solo use

- **Session tracking** — designed for multi-agent coordination. For solo human use, it's optional.
- **Collision prevention** — the `updated` / `last_edited_by` fields prevent multi-agent conflicts. Solo users can still use them as an audit trail, but they're not critical.
- **AGENTS.md files** — these are per-directory guides for AI agents. As a human, browse directories directly in Obsidian — the AGENTS.md files won't get in the way.
- **Lattices** — YAML workflow definitions for executable pipelines. Useful for computational work, but the triad handles knowledge management without them.

---

## Working with AI Agents

aDNA is designed for **human-agent collaboration**. The architecture serves both audiences simultaneously.

### Agent Orientation

When an AI agent (Claude Code, Cursor, etc.) opens an aDNA vault, it reads `CLAUDE.md` and immediately understands:

- Project structure and where to find things
- Safety rules and what not to touch
- Active state — what's in progress, what's blocked
- Session protocol — how to track its work
- Domain knowledge — project-specific context

No prompt engineering required. The architecture *is* the prompt.

When an agent first opens a fresh vault, it runs an interactive onboarding flow to help customize the vault for your project.

### Session Tracking

Every agent session creates a file in `how/sessions/active/` before modifying vault files:

| Tier | When | What's tracked |
|------|------|---------------|
| **Tier 1** | Default for most work | Session ID, intent, files touched |
| **Tier 2** | Shared config edits | Adds scope declaration, conflict scan, heartbeat |

On completion, sessions close with a **SITREP** — a structured handoff:

- **Completed** — what got done
- **In progress** — what's started but unfinished
- **Next up** — recommended next actions
- **Blockers** — anything preventing progress
- **Files touched** — full audit trail

The next agent reads the last SITREP and picks up where the previous one left off. Knowledge compounds instead of evaporating.

### The Execution Hierarchy

For work larger than a single session:

```
Campaign  (strategic initiative — weeks to months)
├── Phase  (logical grouping with human gate between phases)
│   ├── Mission  (multi-session task — 1-5 sessions)
│   │   ├── Objective  (session-sized unit of work)
│   │   └── Objective
│   └── Mission
└── Phase
```

- **Campaigns** coordinate multiple missions toward a strategic goal
- **Missions** decompose complex tasks into claimable objectives
- **Objectives** are what actually get done in a session
- **Phases** group missions with human approval gates between them

### Cross-Agent Coordination

When multiple agents work on the same project, they coordinate through `who/coordination/` — structured notes that flag dependencies, handoffs, and blockers. Each agent checks coordination on startup, before starting work.

---

## Extending the Ontology

Adding a new entity type takes three steps:

### 1. Create the directory

Place it under the right triad leg:

```bash
mkdir -p who/customers    # People/orgs → who/
# or
mkdir -p what/datasets    # Knowledge objects → what/
# or
mkdir -p how/runbooks     # Operational processes → how/
```

### 2. Add an AGENTS.md

Every directory needs an `AGENTS.md` telling agents what lives here:

```markdown
# customers/ — Agent Guide

## What's Here
Customer records for active and prospective accounts.

## Working Rules
- One file per customer: `customer_<name>.md`
- Check `updated` field before modifying
- Set `last_edited_by` and `updated` on every edit
- Link to contacts via `[[contacts/contact_name]]`
```

### 3. Create a template

Add a template in `how/templates/` so new entities are consistent:

```markdown
---
type: customer
created: 2026-02-28
updated: 2026-02-28
status: prospect
last_edited_by:
tags: [customer]
---

# customer_<name>
```

Configure Templater to auto-trigger the template when files are created in the new directory.

**Using an AI agent?** The entity scaffolding skill (`how/skills/skill_new_entity_type.md`) automates all three steps — just tell the agent what entity type you need and where it belongs.

---

## Context Lattices

> **Lattices are optional.** The triad structure handles knowledge management without them. Lattices add value when you need to model executable workflows, computational pipelines, or multi-step reasoning processes. If your use case is purely knowledge organization, you can skip this section entirely.

A **lattice** is a directed graph connecting datasets, modules, reasoning nodes, and processes into an executable composition. Lattices bridge human knowledge graphs and machine-executable workflows.

### Four Lattice Types

| Type | What it does | Example |
|------|-------------|---------|
| **pipeline** | Deterministic DAG — modules process data through defined stages | Sales pipeline: lead → qualify → propose → close |
| **agent** | LLM-driven reasoning — nodes contain prompts, models make decisions | Code review agent: diff → analyze → critique → suggest |
| **context_graph** | Knowledge structure — connects datasets and context into navigable maps | Domain knowledge graph linking papers, models, and datasets |
| **workflow** | Operational process — defines human/agent procedures | Product launch: research → build → test → ship |

### Three Execution Modes

| Mode | Behavior |
|------|----------|
| **workflow** | Deterministic — every node executes in topological order |
| **reasoning** | LLM-driven — nodes contain prompts, model decides routing |
| **hybrid** | Mixed — some nodes are deterministic, others use LLM reasoning |

### Built-in FAIR Metadata

Every lattice carries [FAIR](https://www.go-fair.org/fair-principles/) metadata — making it findable, accessible, interoperable, and reusable by default:

```yaml
fair:
  license: "MIT"
  creators: ["Lattice Labs"]
  keywords: [research, context engineering, multi-agent]
  provenance: "Designed for deep research workflows"
```

---

## Your First Lattice

### 1. Pick an example

Example lattices ship in `what/lattices/examples/` across multiple domains:

| Example | Type | What it models |
|---------|------|---------------|
| `hello_world.lattice.yaml` | pipeline (workflow) | Minimal 3-node pipeline — start here |
| `sales_pipeline.lattice.yaml` | pipeline (workflow) | Lead → qualify → propose → negotiate → close |
| `product_launch.lattice.yaml` | pipeline (hybrid) | Market research → engineering → QA → launch |
| `learning_path.lattice.yaml` | workflow | Course selection → study → practice → assessment |
| `creative_brief.lattice.yaml` | pipeline (hybrid) | Brief intake → design → revision → delivery |
| `deep_research.lattice.yaml` | pipeline (hybrid) | Research query → validated context object |
| `knowledge_base.lattice.yaml` | context_graph (reasoning) | Knowledge retrieval + LLM reasoning |
| `research_orchestrator.lattice.yaml` | agent (hybrid) | Multi-source research coordination |
| `protein_binder_design.lattice.yaml` | pipeline (workflow) | Computational biology pipeline |

Copy one to start customizing:

```bash
cp what/lattices/examples/hello_world.lattice.yaml what/lattices/my_first.lattice.yaml
```

### 2. Understand the anatomy

A `.lattice.yaml` has five sections:

```yaml
lattice:
  name: my_first                    # URL-safe identifier
  version: "1.0.0"                  # Semver
  lattice_type: pipeline            # pipeline | agent | context_graph | workflow
  description: "What this lattice does"
  execution:
    mode: hybrid                    # workflow | reasoning | hybrid
    runtime: local                  # local | ray | kubernetes
    tier: L1                        # L1 (edge) | L2 (regional) | L3 (cloud)
  nodes:
    - id: input_data
      type: dataset                 # dataset | module | process | reasoning
      description: "Input description"
    - id: analyze
      type: reasoning
      description: "What this node decides"
      prompt: "Instructions for the LLM"
      model_override: "claude-opus-4-6"  # or any LLM model ID
  edges:
    - from: input_data
      to: analyze
      label: "raw data"
      condition: "optional routing condition"
  fair:
    license: "MIT"
    keywords: [your, domain, tags]
```

**Nodes** are the units of work — datasets hold data, modules execute code, processes coordinate, reasoning nodes use LLMs. **Edges** define the flow between them, with optional conditions for branching and loops.

### 3. Validate

```bash
# From the repo root
cd what/lattices
pip install -r tools/requirements.txt
python -c "
from tools.lattice_validate import validate_lattice_file
result = validate_lattice_file('my_first.lattice.yaml')
print(f'Valid: {result.valid}')
for e in result.errors: print(f'  ERROR: {e}')
for w in result.warnings: print(f'  WARN:  {w}')
"
```

### 4. Visualize

Convert to an Obsidian canvas for visual editing:

```bash
python -c "
from tools.lattice2canvas import lattice_file_to_canvas
lattice_file_to_canvas('my_first.lattice.yaml', 'my_first.canvas')
print('Canvas created — open in Obsidian')
"
```

Edit the canvas visually in Obsidian (drag nodes, reroute edges), then convert back:

```bash
python -c "
from tools.canvas2lattice import canvas_file_to_lattice
canvas_file_to_lattice('my_first.canvas', 'my_first.lattice.yaml')
print('Lattice updated from canvas')
"
```

> **Note**: YAML is the source of truth. Canvas is a derived visualization. Round-tripping preserves node IDs, refs, and edge topology, but canvas-only properties (positions, dimensions, groups) don't survive the return trip. See [`canvas_yaml_interop.md`](what/lattices/canvas_yaml_interop.md) for the full mapping spec.

---

## Context Engineering

Token efficiency is the constraint that shapes everything. Every token in an agent's context window must earn its place.

| Principle | What it means |
|-----------|--------------|
| **Smallest high-signal token set** | Deliver maximum useful signal in the fewest tokens. Ruthlessly cut filler. |
| **Sub-agents as compression** | A sub-agent explores 10K+ tokens of source material and returns 1-2K of structured findings. 5-10x compression. |
| **Progressive disclosure** | Structure output in tiers: Tier 1 is self-contained and always loaded. Tier 2 adds depth on demand. Tier 3 provides source verification. |
| **Model-attention-aware formatting** | Tables compress information 40-60% vs. prose and are easier for models to parse. Use structured formats. |
| **Just-in-time retrieval** | Load only the context needed for the current operation. Don't front-load the entire knowledge base. |

The `what/context/` directory implements these principles as a **context library** — structured knowledge files with token estimates, tiered information, and progressive loading. Agents load only the subtopics they need for the current task.

---

## Architecture Reference

### Governance Files

| File | Loaded by | Scope | Update frequency |
|------|-----------|-------|-----------------|
| `CLAUDE.md` | Agents (auto) | Global — project structure, safety, protocol | Rarely (version-gated) |
| `MANIFEST.md` | Both | Global — project identity, architecture | On structural changes |
| `STATE.md` | Both | Global — current plans, blockers, next steps | Every session |
| `AGENTS.md` | Agents | Per-directory — what lives here, how to work with it | When directory contents change |
| `README.md` | Humans | Global — external onboarding, GitHub rendering | On major updates |

### Content-as-Code Pipelines

Files flow through stage directories. A file's location *is* its processing state:

```
how/pipelines/prd_rfc/
├── 01_research/        # Problem exploration
├── 02_requirements/    # PRD authoring (human gate)
├── 03_design/          # RFC authoring
└── 04_review/          # Final approval (human gate)
```

Each stage has an `AGENTS.md` with instructions. Drop a file into stage 1, and agents (or humans) advance it through the pipeline by moving it to the next stage directory.

### Compute Tiers

Lattices specify which compute tier they target:

| Tier | Scope | Example |
|------|-------|---------|
| **L1** (Edge) | Local/edge compute, lightweight inference | Laptop GPU, edge device |
| **L2** (Regional) | Institutional clusters, moderate training | University HPC, on-prem cluster |
| **L3** (Cloud/HPC) | Large-scale data centers, heavy training | Cloud GPU fleet |

### Collision Prevention

Three rules keep multi-agent work safe:

1. **Read before write** — always read current content immediately before writing
2. **Check `updated`** — if another agent edited today, confirm before overwriting
3. **Set `last_edited_by`** — every edit updates the frontmatter audit trail

---

## Further Reading

| Document | What it covers |
|----------|---------------|
| [`what/docs/adna_standard.md`](what/docs/adna_standard.md) | Full normative specification (v2.1) — RFC 2119 keywords, all MUST/SHOULD/MAY rules |
| [`what/docs/adna_design.md`](what/docs/adna_design.md) | Architecture rationale — why three legs, why these governance files, design tradeoffs |
| [`what/docs/adna_bridge_patterns.md`](what/docs/adna_bridge_patterns.md) | Multi-instance composition — nesting, sibling, monorepo patterns |
| [`what/lattices/canvas_yaml_interop.md`](what/lattices/canvas_yaml_interop.md) | Canvas ↔ YAML bidirectional mapping specification |
| [`what/lattices/lattice_yaml_schema.json`](what/lattices/lattice_yaml_schema.json) | JSON Schema for `.lattice.yaml` validation |

---

## License

[MIT](LICENSE)

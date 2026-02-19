# lattice-adna

**The knowledge genome for AI-native projects.**

aDNA gives any project a persistent knowledge architecture that AI agents can orient in, operate on, and coordinate across — alongside humans. It's an ontology for building your ontology: a minimal scaffold that bootstraps domain-specific knowledge structures from day one. Clone this repo to build your own.

> *aDNA originated from the [Lattice Protocol](https://github.com/lat-labs/lattice-protocol) for federated AI compute, but the knowledge architecture is universal — any team running AI agents benefits from structured project DNA.*

---

## The Problem

AI agents start every session cold. No memory of past work, no awareness of other agents, no understanding of the project they're operating in. This creates four compounding failures:

| Failure | What happens |
|---------|-------------|
| **Orientation overhead** | Every session begins with the agent re-discovering project structure, conventions, and current state. Context windows fill with exploration instead of execution. |
| **Coordination failure** | Multiple agents (or humans + agents) work on the same project without shared protocol. They overwrite each other's work, duplicate effort, and produce inconsistent outputs. |
| **Knowledge fragmentation** | Insights discovered in one session are lost by the next. There's no persistent medium that accumulates project understanding across sessions and agents. |
| **Audience divergence** | Humans browse knowledge visually (folders, links, graphs). Agents parse knowledge programmatically (frontmatter, structured files, protocol files). Most projects serve one audience poorly or both terribly. |

aDNA solves these by giving projects a **deliberate knowledge architecture** — a genome that both humans and agents can read, navigate, and build on.

---

## The aDNA Paradigm

**Agentic DNA (aDNA)** is a knowledge architecture standard. Think of it as the genome of a project: structured knowledge that agents read on startup to orient, operate, and coordinate — while humans browse the same structure through tools like Obsidian.

### The Triad

Every piece of project knowledge answers exactly one of three questions:

```
what/    →  What does this project know?     (knowledge)
how/     →  How does this project work?      (operations)
who/     →  Who is involved?                 (organization)
```

This is the **triad** — the universal ontology at the heart of aDNA. Three directories. Three questions. Complete coverage.

**The Question Test** classifies any content:

| Content | Question | Triad leg |
|---------|----------|-----------|
| A research paper summary | What do we know about X? | `what/` |
| The deployment checklist | How do we deploy? | `how/` |
| The engineering team roster | Who works on this? | `who/` |
| An architecture decision record | What did we decide? | `what/decisions/` |
| A campaign plan for Q2 launch | How do we execute Q2? | `how/campaigns/` |

Why three? Two legs create sorting ambiguity — people get conflated into either knowledge or process. Four or more legs proved unnecessary in practice; candidates like TOOLS, WHERE, and WHEN all decompose cleanly into the existing three. Three is the minimum that separates knowledge, process, and people without overlap.

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

## Context Lattices

A **lattice** is a directed graph connecting datasets, modules, reasoning nodes, and processes into an executable composition. Lattices are the bridge between human knowledge graphs and machine-executable workflows.

### Four Lattice Types

| Type | What it does | Example |
|------|-------------|---------|
| **pipeline** | Deterministic DAG — modules process data through defined stages | Deep research: query → plan → workers → merge → validate → output |
| **agent** | LLM-driven reasoning — nodes contain prompts, models make decisions | Code review agent: diff → analyze → critique → suggest |
| **context_graph** | Knowledge structure — connects datasets and context into navigable maps | Domain knowledge graph linking papers, models, and datasets |
| **workflow** | Operational process — defines human/agent procedures | Sprint planning: backlog → prioritize → assign → track |

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
  creators: ["Your Name"]
  keywords: [research, context engineering, multi-agent]
  provenance: "Designed for deep research workflows"
```

### Example: Deep Research Lattice

The included `deep_research.lattice.yaml` shows a hybrid pipeline that transforms a research query into an optimized context object:

```
research_query → planning → source_dispatch → raw_findings
                                                    ↓
context_output ← review_gate ← standardize ← optimize ← validate ← merge_dedup
                                                              ↑______|
                                                           (validation loop)
```

10 nodes across 4 types (dataset, reasoning, process), 11 edges including a conditional validation loop and effort-scaled routing. The planning node uses `claude-opus-4-6` for query classification, the validate node self-loops until quality thresholds pass, and the review gate conditionally routes to human review for high-effort research.

---

## An Ontology for Building Your Ontology

Base aDNA ships **14 entity types** across the triad — the minimal operational ontology any project needs:

| Triad leg | Entities | Purpose |
|-----------|----------|---------|
| **WHO** (3) | `governance`, `team`, `coordination` | Who decides, who works, how they sync |
| **WHAT** (4) | `context`, `decisions`, `modules`, `lattices` | What you know, what you've decided, what you build, how you compose |
| **HOW** (7) | `campaigns`, `missions`, `sessions`, `templates`, `skills`, `pipelines`, `backlog` | Plan → decompose → execute → track → automate → ideate |

**You extend by adding domain-specific entities under the appropriate triad leg.** The base gives you operational infrastructure — sessions, missions, coordination — that works from day one. Your extensions add the domain knowledge that makes the project yours.

For example, Lattice Labs extended the base with 8 domain entities:

| Extension | Added entities | Triad leg |
|-----------|---------------|-----------|
| **CRM** | `customers`, `partners`, `contacts`, `projects`, `communications`, `roadmap`, `data` | `who/` |
| **Science** | `hardware`, `datasets`, `targets` | `what/` |

A biotech lab might add `experiments/`, `compounds/`, `protocols/` under `what/`. A software team might add `services/`, `incidents/`, `deployments/` under `how/`. The triad doesn't prescribe your domain — it gives you the scaffold to build your domain ontology on.

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
lattice-adna/
├── CLAUDE.md                           # Agent master context
├── MANIFEST.md                         # Project identity
├── STATE.md                            # Operational state
├── what/                               # Knowledge
│   ├── context/                        #   Context library
│   ├── decisions/                      #   Architecture Decision Records
│   ├── docs/                           #   aDNA specification documents
│   │   ├── adna_standard.md            #     Full normative spec (v2.0)
│   │   ├── adna_design.md              #     Architecture rationale
│   │   └── adna_bridge_patterns.md     #     Multi-instance composition
│   └── lattices/                       #   Lattice definitions
│       ├── lattice_yaml_schema.json    #     JSON Schema
│       ├── canvas_yaml_interop.md      #     Canvas ↔ YAML spec
│       ├── examples/                   #     3 ready-to-use lattices
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
| **3 example lattices** | `what/lattices/examples/` | Deep research, protein binder design, research orchestrator |
| **10 templates** | `how/templates/` | Session, mission, campaign, ADR, context, coordination, backlog, skill, PRD, RFC |
| **PRD/RFC pipeline** | `how/pipelines/prd_rfc/` | 4-stage content-as-code planning workflow |
| **aDNA spec docs** | `what/docs/` | Normative standard, design rationale, bridge patterns |
| **Obsidian config** | `.obsidian/` | Tokyo Night theme, 9 CSS snippets, 12 pre-configured plugins (run `setup.sh`) |

---

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/lat-labs/lattice-adna.git
cd lattice-adna
```

### 2. Install plugins and theme

```bash
./setup.sh
```

Downloads and installs 12 community plugins and the Tokyo Night theme. Run with `--force` to re-download everything.

Optionally install [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) font — the vault falls back to system fonts without it.

### 3. Open in Obsidian

Open `lattice-adna/` as a vault in [Obsidian](https://obsidian.md). Enable community plugins when prompted. The accent color (Rebecca Purple `#663399`) and CSS snippets activate automatically.

### 4. Start an agent session (recommended)

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

> **Prefer reading?** Skip this step and customize the vault manually.
> The governance files (`CLAUDE.md`, `MANIFEST.md`, `STATE.md`) are
> well-documented and ready to edit.

---

## Your First Lattice

### 1. Pick an example

Three example lattices ship in `what/lattices/examples/`:

| Example | Type | What it models |
|---------|------|---------------|
| `deep_research.lattice.yaml` | pipeline (hybrid) | Research query → validated context object |
| `protein_binder_design.lattice.yaml` | pipeline | Protein design → docking → optimization |
| `research_orchestrator.lattice.yaml` | agent | Multi-source research coordination |

Copy one to start customizing:

```bash
cp what/lattices/examples/deep_research.lattice.yaml what/lattices/my_first.lattice.yaml
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
      model_override: "claude-opus-4-6"
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

## Working with AI Agents

aDNA is designed for **human-agent collaboration**. The architecture serves both audiences simultaneously.

### Agent Orientation

When an agent first opens a fresh lattice-adna vault, it runs an interactive onboarding flow to help customize the vault for your project.

When an AI agent (Claude Code, Cursor, etc.) opens an aDNA vault, it reads `CLAUDE.md` and immediately understands:

- Project structure and where to find things
- Safety rules and what not to touch
- Active state — what's in progress, what's blocked
- Session protocol — how to track its work
- Domain knowledge — project-specific context

No prompt engineering required. The architecture *is* the prompt.

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
mkdir -p who/customers    # CRM entity → who/
# or
mkdir -p what/datasets    # Knowledge entity → what/
# or
mkdir -p how/runbooks     # Operational entity → how/
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
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
status: prospect
last_edited_by:
tags: [customer]
---

# customer_<name>
```

Configure Templater to auto-trigger the template when files are created in the new directory.

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
| [`what/docs/adna_standard.md`](what/docs/adna_standard.md) | Full normative specification (v2.0) — RFC 2119 keywords, all MUST/SHOULD/MAY rules |
| [`what/docs/adna_design.md`](what/docs/adna_design.md) | Architecture rationale — why three legs, why these governance files, design tradeoffs |
| [`what/docs/adna_bridge_patterns.md`](what/docs/adna_bridge_patterns.md) | Multi-instance composition — nesting, sibling, monorepo patterns |
| [`what/lattices/canvas_yaml_interop.md`](what/lattices/canvas_yaml_interop.md) | Canvas ↔ YAML bidirectional mapping specification |
| [`what/lattices/lattice_yaml_schema.json`](what/lattices/lattice_yaml_schema.json) | JSON Schema for `.lattice.yaml` validation |

---

## License

[MIT](LICENSE)

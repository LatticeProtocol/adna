# lattice-adna

A quick-start Obsidian vault for building and managing Lattice YAML definitions with AI agents. Clone, open in Obsidian, and start designing lattices immediately.

## What is lattice-adna?

**lattice-adna** is a self-contained project template built on the [aDNA (Agentic DNA)](what/docs/adna_standard.md) knowledge architecture. It provides:

- **Lattice YAML tools** — validate, visualize, and convert lattice definitions
- **Canvas interop** — bidirectional conversion between `.lattice.yaml` and Obsidian `.canvas` for visual editing
- **AI agent integration** — structured `CLAUDE.md` context so AI agents can navigate and operate in the vault
- **Planning pipeline** — R&D → PRD → RFC workflow for product planning
- **9 templates** — sessions, missions, campaigns, ADRs, PRDs, RFCs, and more
- **Example lattices** — 3 ready-to-customize lattice definitions

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/YOUR_ORG/lattice-adna.git
cd lattice-adna
```

### 2. Open in Obsidian

Open the `lattice-adna/` directory as a vault in Obsidian.

### 3. Install community plugins

Go to **Settings → Community plugins → Browse** and install:

| Plugin | Purpose |
|--------|---------|
| Dataview | Query and display structured data |
| Templater | Template engine with folder-based auto-triggers |
| Meta Bind | Inline metadata editing |
| Homepage | Set a start page |
| Tasks | Task tracking |
| Style Settings | Customize CSS snippet options |
| Table Editor | Better table editing |
| Omnisearch | Vault-wide search |
| Terminal | Terminal access from Obsidian |
| Buttons | Clickable action buttons |
| Folder Notes | Index files for folders |

### 4. Install theme and font

- **Theme**: Install "Tokyo Night" from **Settings → Appearance → Themes**
- **Font**: Download [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) and install on your system

The accent color (Rebecca Purple `#663399`) and CSS snippets will activate automatically.

## What's Inside

```
lattice-adna/
├── what/           # Knowledge — lattice tools, schema, examples, docs
├── how/            # Operations — templates, sessions, pipelines
└── who/            # Organization — coordination, governance
```

| Layer | Question | Key Contents |
|-------|----------|-------------|
| **what/** | What does this project know? | Lattice YAML tools, JSON Schema, 3 examples, aDNA docs |
| **how/** | How does this project work? | 9 templates, PRD/RFC pipeline, session tracking |
| **who/** | Who is involved? | Coordination notes, governance |

## Your First Lattice

1. Copy an example from `what/lattices/examples/`
2. Customize the nodes, edges, and metadata
3. Validate:

```bash
cd what/lattices
pip install -r tools/requirements.txt
python -c "
from tools.lattice_validate import validate_lattice_file
result = validate_lattice_file('examples/deep_research.lattice.yaml')
print(f'Valid: {result.valid}')
for e in result.errors: print(f'  {e}')
for w in result.warnings: print(f'  [warn] {w}')
"
```

4. Convert to canvas for visual editing:

```bash
python -c "
from tools.lattice2canvas import lattice_file_to_canvas
lattice_file_to_canvas('examples/deep_research.lattice.yaml', 'deep_research.canvas')
print('Canvas created — open in Obsidian')
"
```

## AI Agent Integration

This vault includes a `CLAUDE.md` that provides structured context for AI agents (Claude Code, Cursor, etc.). When an AI agent opens this vault, it automatically understands:

- Project structure and navigation
- Safety rules and collision prevention
- Session tracking protocol
- Available tools and templates
- Domain knowledge (lattice types, compute tiers, FAIR metadata)

## Templates

| Template | Auto-triggers in | Purpose |
|----------|-----------------|---------|
| Session | `how/sessions/active/` | Track agent work sessions |
| Mission | `how/missions/` | Multi-session task decomposition |
| Context | `what/context/` | Agent knowledge files |
| ADR | `what/decisions/` | Architecture Decision Records |
| Coordination | `who/coordination/` | Cross-agent notes |
| Backlog | `how/backlog/` | Ideas and improvements |
| Campaign | `how/campaigns/` | Multi-mission initiatives |
| PRD | Pipeline stage 2 | Product Requirements Documents |
| RFC | Pipeline stage 3 | Request for Comments / Design |

## Pipeline

The R&D → PRD → RFC pipeline in `how/pipelines/prd_rfc/` provides a structured product planning workflow:

```
01_research/ → 02_requirements/ → 03_design/ → 04_review/ → deliverables/
```

Drop a problem description into `01_research/` and follow the stage `AGENTS.md` instructions to produce approved PRD + RFC documents.

## License

MIT

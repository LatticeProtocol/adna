# CLAUDE.md — lattice-adna
<!-- v1.0 | 2026-02-17 -->

## Identity & Mission

**lattice-adna** — a quick-start Obsidian vault for building and managing Lattice YAML definitions with AI agents. Clone this repo, open it in Obsidian, and start designing lattices immediately.

This project uses the **aDNA (Agentic DNA)** knowledge architecture — a universal structure for AI-native projects that both humans and AI agents can navigate, operate, and coordinate within.

---

## Project Map

```
lattice-adna/
├── CLAUDE.md                    # Agent master context (this file)
├── AGENTS.md                    # Root agent guide
├── MANIFEST.md                  # Project overview, architecture, entry points
├── STATE.md                     # Operational state — current phase, blockers, next steps
├── README.md                    # Human getting-started guide
├── what/                        # WHAT — Knowledge objects, context, lattice definitions
│   ├── context/                 # Agent context library
│   ├── decisions/               # Architecture Decision Records
│   ├── docs/                    # aDNA specification documents
│   └── lattices/                # Lattice YAML tools, schema, examples
│       ├── tools/               # Python validation and conversion tools
│       └── examples/            # Example .lattice.yaml files
├── how/                         # HOW — Operations, sessions, templates
│   ├── templates/               # 9 reusable templates
│   ├── sessions/                # Session tracking (active/ + history/)
│   ├── missions/                # Multi-session plans
│   ├── backlog/                 # Ideation and improvement tracking
│   ├── campaigns/               # Multi-mission strategic initiatives
│   ├── pipelines/               # Content-as-code workflows
│   │   └── prd_rfc/             # R&D → PRD → RFC planning pipeline
│   └── deliverables/            # Pipeline output artifacts
└── who/                         # WHO — People, coordination, governance
    ├── coordination/            # Cross-agent ephemeral notes
    └── governance/              # Roles, policies
```

---

## Safety Rules

### File Safety

| Risk | Rule |
|------|------|
| Low (content files) | Check `updated` before overwriting. Set `last_edited_by` and `updated`. |
| Medium (shared configs) | Read before write. One config at a time. |
| None (new files) | Creating new files has no collision risk. |

### Collision Prevention Rules

1. **Read before write.** Always read current content immediately before writing.
2. **Check `updated` field.** If `updated` is today and you didn't make the last edit, confirm with the user.
3. **Set `last_edited_by` and `updated`.** When modifying any content file, update frontmatter:
   ```yaml
   updated: 2026-02-17
   last_edited_by: agent_{username}
   ```
4. **One shared config at a time.** Edit one config, verify the write, then move to the next.
5. **New files are safe.** Creating a new file has no collision risk.

### Escalation

- Stop if uncertain about destructive or irreversible actions
- Flag blockers with `#needs-human`
- Do not proceed with ambiguous scope — ask the user

### Priority Hierarchy

1. **Data integrity** — never corrupt or lose existing data
2. **User-requested tasks** — explicit instructions from current user
3. **Operational maintenance** — session tracking, plan updates
4. **Exploration** — research, audits, improvements

---

## Agent Protocol

### Startup Checklist

Every session, in order:
1. **CLAUDE.md** — auto-loaded; confirms project structure and rules
2. **STATE.md** — operational snapshot: current phase, blockers, next steps
3. **`how/sessions/active/`** — check for conflicting sessions
4. **`who/coordination/`** — read any urgent cross-agent notes
5. **`how/backlog/`** — quick scan for ideas relevant to current session
6. **`how/campaigns/`** — check for active campaigns
7. **`how/missions/`** — check for active missions
8. **Create session file** in `how/sessions/active/` and begin work

### Session Tracking

Every session creates a file in `how/sessions/active/` before modifying project files. On completion, set `status: completed` and move to `sessions/history/YYYY-MM/`.

- **Tier 1** (default): Lightweight audit trail — session ID, intent, files touched.
- **Tier 2** (shared config edits): Adds scope declaration, conflict scan, heartbeat.

Full protocol: `how/sessions/AGENTS.md`

### Session Closure (SITREP)

Every session ends with a structured status report:

- **Completed** — tasks finished this session
- **In progress** — work started but not finished (with handoff notes)
- **Next up** — recommended next actions or plan tasks
- **Blockers** — anything preventing progress (tag `#needs-human` if applicable)
- **Files touched** — created, modified, or moved

Every session MUST include a **Next Session Prompt** — a self-contained paragraph enabling a fresh agent to continue the work.

### Campaigns

Multi-mission strategic initiatives in `how/campaigns/`. Campaigns coordinate multiple missions toward a strategic goal, with phased execution and user gates between phases. Protocol: `how/campaigns/AGENTS.md`

### Missions

Tasks too large for one session are decomposed into missions in `how/missions/`. Agents claim objectives by session, track progress, and hand off. Protocol: `how/missions/AGENTS.md`

### Content-as-Code Pipelines

Folder-based workflows where files flow through stage directories. Each stage folder contains an `AGENTS.md` with agent instructions; a file's location is its state. Pipeline index: `how/pipelines/AGENTS.md`

### Context Library

Before domain tasks, check `what/context/` for relevant topics. Read the topic `AGENTS.md`, load only the subtopics you need. Protocol: `what/context/AGENTS.md`

### Backlog

Durable ideation and improvement tracking in `how/backlog/`. Scan on session start for relevant ideas, create new idea files when improvements surface during work. Protocol: `how/backlog/AGENTS.md`

---

## Domain Knowledge

### Lattice Types

| Type | Description | Execution Mode |
|------|-------------|---------------|
| `pipeline` | Deterministic DAG of modules | `workflow` |
| `agent` | LLM-driven reasoning | `reasoning` |
| `context_graph` | Knowledge structure | varies |
| `workflow` | Operational process | `workflow` |

### Execution Modes

| Mode | Description |
|------|-------------|
| `workflow` | Deterministic DAG — fixed sequence of steps |
| `reasoning` | LLM-driven — model decides next steps |
| `hybrid` | Mixed — workflow structure with reasoning at decision points |

### Compute Tiers

| Tier | Scope | Example |
|------|-------|---------|
| **L1** (Edge) | Local/edge compute, lightweight inference | Laptop GPU, edge device |
| **L2** (Regional) | Institutional clusters, moderate training | University cluster, on-prem HPC |
| **L3** (Cloud/HPC) | Large-scale data centers, heavy training | Cloud GPU fleet |

### FAIR Metadata

Every `.lattice.yaml` includes a `fair` block with:
- `license` — SPDX identifier (e.g., `MIT`, `Apache-2.0`)
- `creators` — list of creator names
- `keywords` — semantic tags for findability
- `identifier` — optional DOI or persistent ID
- `provenance` — origin and methodology description

---

## Working with Content

### Naming

**Always underscores, never hyphens.** Pattern: `type_descriptive_name.md`

### Metadata

All content files require YAML frontmatter:
```yaml
---
type: {entity_type}
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active
last_edited_by: agent_{username}
tags: []
---
```

### Linking

Use bidirectional wikilinks when adding relationships between entities.

---

## Quickstart

### Agent Quickstart (5 steps)
1. Read CLAUDE.md — understand project structure, safety rules
2. Read STATE.md — understand current phase, blockers, recent decisions
3. Check `how/sessions/active/` — identify any conflicting sessions
4. Check `who/coordination/` — read urgent cross-agent notes
5. Create session file in `how/sessions/active/` and begin work

### Human Quickstart (4 steps)
1. Read README.md — understand what this project is and how to navigate
2. Read MANIFEST.md — understand architecture and entry points
3. Browse the triad (`what/`, `how/`, `who/`) — explore the knowledge structure
4. Open STATE.md — see current operational status and next steps

---
<!-- v1.0 | 2026-02-17 -->

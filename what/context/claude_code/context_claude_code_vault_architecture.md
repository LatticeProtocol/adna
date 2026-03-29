---
type: context_guide
topic: claude_code
subtopic: vault_architecture
created: 2026-03-27
updated: 2026-03-28
sources: ["aDNA Standard v2.2", "Obsidian vault patterns", "Claude Code project scoping"]
context_version: "1.0"
token_estimate: ~2500
last_edited_by: agent_aria
runtime: claude_code
tags: [context, claude_code, vault_architecture, multi_vault, organization]
---

# System Configuration: Vault Architecture

> **Runtime scope:** This file documents the Claude Code implementation. The underlying pattern applies to any runtime; only the implementation details are Claude Code-specific.

Multi-vault organization patterns for AI-native projects where humans browse in Obsidian and agents operate via Claude Code.

---

## Key Principles

| # | Principle | Rule |
|---|-----------|------|
| 1 | **Multi-vault separation** | Split knowledge domains into separate vaults by purpose. Each vault is simultaneously an Obsidian vault and a Claude Code project scope. Code projects, research, and operations each get their own vault. This gives agents narrower context windows and gives humans cleaner navigation. |
| 2 | **`_meta/` as operational layer** | Every vault and project gets a `_meta/` directory for all generated output: plans, reports, logs, context, agent memory. Source directories stay clean. Generated artifacts never pollute source content. |
| 3 | **`.claude/` is READ-ONLY** | The `.claude/` directory contains configuration only: `CLAUDE.md`, settings, rules, hooks. Agents never write generated content there. All output routes to `_meta/`. This is a non-negotiable invariant. |
| 4 | **Output routing discipline** | Every generated artifact has a canonical destination. Consistent routing prevents file sprawl and makes cleanup predictable. See the routing table below. |
| 5 | **Workspace convention** | A parent directory acts as the workspace root, containing the base template repo and all forked projects. A workspace-level `CLAUDE.md` governs shared rules that cascade down to all vaults and projects. |
| 6 | **Vault-per-domain, project-per-repo** | Vaults organize by work domain (coding, research, collaboration). Within a vault, each repo or project is a subdirectory with its own `_meta/` and optionally its own `.claude/CLAUDE.md` for project-specific overrides. |
| 7 | **Gitignore `_meta/`** | The `_meta/` directory is gitignored in upstream repos. It contains local operational state: session files, agent memory, cost logs. Each clone generates its own. Shared knowledge lives in source directories, not `_meta/`. |

---

## Output Routing Table

| Artifact Type | Destination | Examples |
|--------------|-------------|----------|
| Plans | `_meta/plans/` | Implementation plans, migration plans, campaign plans |
| Documentation | `_meta/docs/` | Generated guides, summaries, analysis docs |
| Session state | `_meta/context/` | Active session context, working memory |
| Cost tracking | `_meta/logs/` | Token usage, API costs, routing logs |
| Reports | `_meta/reports/` | Audits, compliance checks, reviews |
| Agent memory | `_meta/agentdb/` | Persistent agent state (SQLite) |
| Research cache | `_meta/research/` | Cached findings, search results |
| Reference | `_meta/reference/` | On-demand verbose rules, loaded when needed |

---

## Workspace Layout

Generic workspace structure showing the governance cascade:

```
~/lattice/                       # Workspace root
├── CLAUDE.md                    # Workspace governance (inherits from ~/.claude/)
├── .claude/                     # Workspace Claude config
│   └── rules/                   # Shared rules (invariants, patterns)
├── .mcp.json                    # MCP server configs
├── _meta/                       # Workspace operational layer
│   ├── agentdb/                 # Cross-project agent memory
│   ├── logs/                    # Activity and cost logs
│   └── reports/                 # Cross-project reports
├── coding_vault/                # Domain: code projects
│   ├── .claude/CLAUDE.md        # Vault-level rules
│   ├── _meta/                   # Vault operational layer
│   ├── project_alpha/           # Individual project
│   │   ├── .claude/CLAUDE.md    # Project-level overrides (optional)
│   │   └── _meta/              # Project operational layer
│   └── project_beta/
│       └── _meta/
└── research_vault/              # Domain: research & collaboration
    ├── .claude/CLAUDE.md
    ├── _meta/
    └── study_one/
        └── _meta/
```

### Governance Cascade

Configuration inherits top-down. Each level can add rules but should not contradict parent levels.

```
~/.claude/CLAUDE.md              # Global: agent identity, core obligations
  └── ~/lattice/CLAUDE.md        # Workspace: shared invariants, session protocol
      └── coding_vault/.claude/  # Vault: domain conventions, quality gates
          └── project/.claude/   # Project: specific rules, entry points
```

---

## `_meta/` Structure

Standard layout for the operational layer at any level (workspace, vault, or project):

```
_meta/
├── agentdb/          # Persistent agent memory (SQLite)
│   └── agent.db      # AgentDB instance
├── context/          # Active session state
│   └── active.md     # Current session context
├── plans/            # Generated plans
├── docs/             # Generated documentation
├── reports/          # Analysis and audit reports
├── logs/             # Activity, cost, and routing logs
│   └── costs.jsonl   # Token and API cost tracking
├── research/         # Cached research findings
└── reference/        # On-demand verbose rules
```

**Absolute paths required.** When reading or writing to `_meta/`, always use absolute paths. Sub-repos with their own `_meta/` will silently route to the wrong database if you rely on `$PWD` resolution.

---

## Vault Root Requirements

Every vault root must contain these files:

| File | Purpose | Required |
|------|---------|----------|
| `.claude/CLAUDE.md` | Agent configuration and project rules | Yes |
| `.gitignore` | Must exclude `_meta/` from version control | Yes |
| `_meta/` | Operational layer directory | Yes |
| `CLAUDE.md` | Project-level agent context (if also a project root) | Conditional |
| `MANIFEST.md` | Project overview and architecture | Conditional |
| `STATE.md` | Operational state snapshot | Conditional |

---

## Recommendations

| Area | Recommendation |
|------|---------------|
| **Starting scale** | Begin with 2 vaults maximum: one for code projects, one for everything else. Split further only when separation reduces context noise. |
| **Vault root setup** | Every vault root needs: `.claude/CLAUDE.md`, `.gitignore` (excluding `_meta/`), and a `_meta/` directory. |
| **Workspace shortcuts** | Use symlinks at workspace root for quick access to key configs (e.g., `claude-config -> .claude`, `mcp-config.json -> .mcp.json`). |
| **Workspace CLAUDE.md** | Keep it minimal. It inherits from the global `~/.claude/CLAUDE.md` and adds workspace-level rules only. |
| **Context inheritance** | Projects within a vault inherit the vault's `.claude/CLAUDE.md` via Claude Code's project scoping. Only add project-level overrides when the project genuinely diverges. |
| **Template separation** | Keep the base template repo (e.g., `adna/`) as a read-only upstream. Fork into new project directories for actual work. This allows `git pull` updates from upstream. |
| **Agent memory isolation** | Each project gets its own `_meta/agentdb/agent.db`. Cross-project memory lives at the workspace level `_meta/agentdb/`. |

---

## Anti-Patterns

| Anti-Pattern | Problem | Correction |
|-------------|---------|------------|
| Writing generated content to `.claude/` | Violates READ-ONLY invariant. Pollutes configuration with ephemeral artifacts. | Route all generated output to `_meta/`. |
| Single monolithic vault | Everything in one directory loses context scoping. Agents load irrelevant context. Obsidian sidebar becomes unnavigable. | Split by domain. Two vaults is enough for most setups. |
| No `_meta/` separation | Mixing generated artifacts with source content. Git history polluted with session files. Unclear what is authoritative. | Create `_meta/` at every level. Gitignore it. |
| Hardcoded absolute paths in configs | Breaks portability across machines and collaborators. | Use relative paths or environment variables. |
| Skipping workspace `CLAUDE.md` | Loses the governance cascade. Each vault reinvents shared rules. Inconsistent agent behavior across projects. | Create a minimal workspace `CLAUDE.md` that inherits from global and adds workspace-level rules. |
| Deep nesting beyond 3 levels | Workspace > vault > project is sufficient. Deeper nesting adds cognitive overhead without proportional benefit. | Flatten. Use naming conventions instead of directory depth. |
| Sharing `_meta/` via git | Operational state is local. Sharing it causes conflicts and leaks session data. | Add `_meta/` to `.gitignore` at every repo root. |

---

## Decision Checklist: When to Create a New Vault

| Question | If Yes |
|----------|--------|
| Does this domain have fundamentally different file types? | New vault. |
| Would agents benefit from a narrower context window? | New vault. |
| Do collaborators need different access? | New vault. |
| Is it just a new project in the same domain? | Same vault, new subdirectory. |
| Is it a temporary experiment? | Same vault, `_meta/research/` or a scratch project. |

---

## Sources

- **aDNA Standard v2.2** -- triad structure (who/what/how), governance files (CLAUDE.md, MANIFEST.md, STATE.md), entity types
- **Obsidian vault patterns** -- multi-vault organization, sidebar navigation, link resolution boundaries
- **Claude Code project scoping** -- how `.claude/` directories scope agent context, inheritance chain from global to project level

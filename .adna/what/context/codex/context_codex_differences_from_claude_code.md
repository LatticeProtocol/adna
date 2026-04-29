---
type: context_core
topic: codex
subtopic: differences_from_claude_code
created: 2026-04-28
updated: 2026-04-28
sources: ["campaign_codex_compatibility M00", "claude_code/ topic in this same context library"]
context_version: "1.0"
token_estimate: ~4000
last_edited_by: agent_herb
runtime: codex
tags: [context, codex, claude_code, comparison, parity]
freshness_category: developing
last_evaluated: 2026-04-28
---

# Codex vs Claude Code — Differences That Matter

> **Runtime scope:** codex CLI 0.125.0 vs Claude Code (current). Both runtimes evolve; re-evaluate at major version bumps on either side.

aDNA targets both runtimes as first-class. Most operational doctrine (Berthier, OODA, war ontology, sync rules) is universal. The differences below are the surfaces that diverge — these inform when to write runtime-specific guidance vs. runtime-neutral guidance.

## Quick comparison

| Surface | Claude Code | Codex CLI | Impact |
|---------|-------------|-----------|--------|
| Governance file | `CLAUDE.md` | `AGENTS.md` | aDNA repos ship both; AGENTS.md points at CLAUDE.md |
| Auto-load doctrine | CLAUDE.md (always) | AGENTS.md cascade (git-aware) | Root AGENTS.md required; CLAUDE.md not seen by codex unless referenced |
| Config dir | `.claude/` (per-machine bits in `.local`) | `.codex/` (per-machine bits in `.local`) | Mirror schema; can't share files |
| MCP servers | `.claude/settings.json` or `.mcp.json` | `~/.codex/config.toml` or `.codex/config.toml` | Different format (JSON vs TOML); same server bins |
| Plan mode | First-class (`/plan`) | None as a CLI mode | Replicate plan-vs-execute via prompt structure |
| Hooks | 6 lifecycle events (config-driven) | None | Don't rely on hooks for codex compatibility |
| Slash commands | First-class | Skills serve overlapping role (loaded, not invoked) | Don't ship `/commands` as the only entry point |
| Memory | Auto-loaded `MEMORY.md` files | No analog | Memory injection has to be explicit in AGENTS.md or skill |
| Skill loading | Skills surfaced as slash commands | `~/.codex/skills/` + `.agents/skills/` auto-load | Different invocation; same content possible |
| Session state | `.claude/settings.local.json` workspace | None native | Use `_meta/` per aDNA convention; runtime-neutral |
| Auth | API key via env or `.claude/auth` | OAuth (ChatGPT) or API key | Onboarding diverges — document both paths |
| Status line | First-class custom status | None | Don't rely on status line for cross-runtime UX |

## Surfaces that DON'T differ (operational doctrine — write runtime-neutral)

- aDNA triad structure (`who/`, `what/`, `how/`)
- AGENTS.md *content* conventions inside aDNA (every directory has one, frontmatter rules, etc.)
- Templates, skills (as markdown documents), pipelines (folder-based)
- Sessions, missions, campaigns, wars (operational hierarchy)
- OODA cadence, AAR, SITREP
- Git as the coordination bus (sync rules, commit-then-push)
- Standing Orders 1-7 in lattice-labs CLAUDE.md
- Berthier persona, doctrine, conduct

## Surfaces where you SHOULD write dual-track or runtime-specific guidance

### 1. Onboarding skills

Steps that mention "open Claude Code with `claude`" should either:
- (a) say "open Claude Code with `claude` OR codex with `codex`", or
- (b) split into `skill_claude_code_onboarding.md` and `skill_codex_onboarding.md` and have a parent skill route based on the user's tool choice.

Default: dual-track in one skill. Split only when divergence > 30% of skill content.

### 2. Plan mode workflows

Claude Code's `/plan` is not directly portable. Codex equivalent: prompt-engineer the plan/execute distinction explicitly:

```
codex exec "Before making changes, write a plan to /tmp/plan.md describing what you'll change and why. Wait for me to confirm before executing."
```

Document this pattern in `how/skills/skill_codex_plan_mode_emulation.md` (NOT YET SHIPPED — file as backlog).

### 3. Hooks-based automation

Anything implemented as a Claude Code hook (auto-formatting on save, auto-commit, etc.) has no codex equivalent. Two options:
- (a) Move the automation to a git pre-commit hook (runs regardless of agent)
- (b) Document the gap in onboarding; let codex users invoke manually

### 4. MCP server enablement

Each runtime has its own config file. Skills that say "enable the foo MCP server" should give both:
- Claude Code: edit `.claude/settings.local.json` `mcpServers.foo` entry
- Codex: add `[mcp.foo]` table to `.codex/config.toml`

### 5. Per-machine workspace

Claude Code's `.claude/settings.local.json` and codex's `.codex/config.local.toml` should both be gitignored. Document in `skill_machine_setup.md` to add **both** to `.gitignore` even if only one runtime is currently in use.

## Migration: Claude-only project → multi-runtime

Steps to add codex support to a Claude-Code-only aDNA project:

1. Add root `AGENTS.md` referencing existing `CLAUDE.md`. (~50 lines, see template.)
2. Audit `how/skills/` for Claude-Code-specific invocation strings; rewrite as dual-track or runtime-neutral.
3. If `.claude/` has MCP servers, mirror to `.codex/config.toml`.
4. Add `.codex/config.local.toml` to `.gitignore`.
5. Verify with `codex exec "orient yourself to this repo"`. Capture transcript.
6. Document the multi-runtime contract in CLAUDE.md (e.g., one line: *"This repo supports Claude Code and codex; AGENTS.md is the codex entry point pointing here."*).

## When to maintain runtime parity vs. accept divergence

**Maintain parity** when:
- The feature is core to aDNA's value proposition (orientation, session tracking, governance)
- A user might switch tools mid-project and expect the same operational reality
- The feature is a documented Standing Order

**Accept divergence** when:
- The feature is a runtime-specific UX nicety (status line, slash commands as primary UI)
- Replicating it costs more than the value it provides on the second runtime
- The runtime simply doesn't expose the primitive (hooks, plan mode as a built-in)

Document the divergence in this file (or a successor) and in the affected skill — don't let it become tribal knowledge.

## See also

- `claude_code/AGENTS.md` — peer topic for the Claude Code runtime
- `context_codex_overview.md`, `context_codex_agents_md_spec.md`, `context_codex_mcp_setup.md` — codex-specific deep dives
- `who/governance/governance_agent_protocol.md` — runtime-neutral agent protocol (the part that does NOT diverge)

---
type: governance
runtime: codex
version: "1.0"
token_estimate: ~600
created: 2026-04-28
updated: 2026-04-28
last_edited_by: agent_herb
tags: [agents_md, codex, governance, root, workspace]
---

# AGENTS.md — Agentic-DNA

This is the **codex-native entry point** for this repo. The canonical operational doctrine lives in [`CLAUDE.md`](./CLAUDE.md) (this file's sibling) and [`.adna/CLAUDE.md`](./.adna/CLAUDE.md) (template doctrine). Both runtimes — Claude Code and OpenAI Codex CLI — are first-class citizens; they read different filenames but converge on the same behavior.

## Why two governance files

- `CLAUDE.md` is the canonical doctrine for this workspace. Claude Code auto-loads it. Codex does **not** auto-load `CLAUDE.md` — that is why this `AGENTS.md` exists, to point codex at it.
- `AGENTS.md` (this file) is what codex auto-loads. It is intentionally short. It hands off to `CLAUDE.md` for the operational rules and to `.adna/AGENTS.md` for the template-level guide.

If both files conflict, treat `CLAUDE.md` as authoritative — `AGENTS.md` is a runtime adapter, not a parallel rulebook.

## Quick start for codex users

1. **Read `CLAUDE.md`** in this directory — workspace-level doctrine (Berthier persona, project routing, lattice/aDNA introduction).
2. **Read `.adna/CLAUDE.md`** — template doctrine that gets copied into every forked project (`<name>.aDNA/`).
3. **If creating a new project**: load `.adna/how/skills/skill_project_fork.md`. The fork target is `<project_name>.aDNA/` at this directory level.
4. **If onboarding**: load `.adna/how/skills/skill_codex_onboarding.md` for codex-specific install + auth + MCP setup notes.

Sub-directory `AGENTS.md` files at `.adna/AGENTS.md`, `.adna/what/AGENTS.md`, `.adna/how/AGENTS.md`, `.adna/who/AGENTS.md` (and deeper) load automatically as you `cd` into those subtrees — codex walks **git-root → cwd**, root-first concat, leaf-overrides-root.

## Codex-specific notes

- **Auth**: ChatGPT Plus or API key (`codex login`).
- **Install**: `npm i -g @openai/codex`. Verify with `codex --version`.
- **MCP**: configured at `~/.codex/` (global) or repo `.codex/` (project, gitignored per-machine bits). The `.adna/` template does not ship an MCP config; project forks may add `.codex/` mirroring `.claude/` semantics.
- **Skills**: codex auto-loads `~/.codex/skills/.system/*/SKILL.md` (global) and `.agents/skills/*/SKILL.md` (project-level).
- **Differences from Claude Code**: see `.adna/what/context/codex/context_codex_differences_from_claude_code.md`.

## Standing rules

1. **CLAUDE.md is authoritative** — if AGENTS.md and CLAUDE.md disagree, follow CLAUDE.md and flag the AGENTS.md drift as a fix to surface upstream.
2. **Never edit `.adna/`** — it's the base template, kept clean for `git pull` updates. Customization happens in forked `<name>.aDNA/` directories.
3. **Onboarding lives in `.adna/`** — first-run detection is in `.adna/CLAUDE.md`, not here.

## See also

- [`CLAUDE.md`](./CLAUDE.md) — workspace doctrine (Claude Code auto-loaded)
- [`.adna/CLAUDE.md`](./.adna/CLAUDE.md) — template doctrine
- [`.adna/AGENTS.md`](./.adna/AGENTS.md) — template root agent guide
- [`.adna/what/context/codex/`](./.adna/what/context/codex/) — codex runtime context library
- [`.adna/how/skills/skill_codex_onboarding.md`](./.adna/how/skills/skill_codex_onboarding.md) — codex install + first-orient
- [`README.md`](./README.md) — human-facing onboarding

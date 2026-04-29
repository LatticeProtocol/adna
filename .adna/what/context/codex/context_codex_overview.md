---
type: context_guide
topic: codex
subtopic: overview
created: 2026-04-28
updated: 2026-04-28
sources: ["campaign_codex_compatibility M00 empirical verification", "OpenAI codex CLI public docs"]
context_version: "1.0"
token_estimate: ~1500
last_edited_by: agent_herb
runtime: codex
tags: [context, codex, overview, install, auth]
freshness_category: stable
last_evaluated: 2026-04-28
---

# Codex CLI — Overview

> **Runtime scope:** OpenAI Codex CLI 0.125.0 on macOS. Linux/Windows behavior assumed similar; verify at adoption.

OpenAI's terminal-based agentic coding tool. Analogous to Claude Code: starts in a working directory, loads governance from local files, executes tasks via tool calls. Different filename convention (`AGENTS.md` instead of `CLAUDE.md`), different config location (`~/.codex/` instead of `~/.claude/`), different auth model (ChatGPT account or API key).

## Install

```bash
npm i -g @openai/codex
codex --version   # 0.125.0 or later
```

Requires Node.js (npm). Available on macOS, Linux, Windows.

## Auth

Two options:

| Method | When | How |
|--------|------|-----|
| **ChatGPT Plus / Pro** | Personal use, included in subscription | `codex login` → browser OAuth flow |
| **API key** | Programmatic, billing per-call | Set `OPENAI_API_KEY` env var (or `.codex/` config) |

Both authenticate the same `codex` binary; switch by editing `~/.codex/auth.json` (managed automatically by `codex login`/`codex logout`).

## What codex reads on startup

1. **`~/.codex/AGENTS.md`** — global doctrine (optional, often absent)
2. **`AGENTS.md` files along the path from `git-root` to `cwd`** — root-first, concat into system prompt, leaf overrides root (see `context_codex_agents_md_spec.md`)
3. **`~/.codex/skills/.system/*/SKILL.md`** — global skills (auto-loaded)
4. **`.agents/skills/*/SKILL.md`** in cwd or ancestors — project skills (auto-loaded)
5. **MCP servers** — defined in `~/.codex/` config or repo `.codex/`

What codex does **NOT** read automatically:
- `CLAUDE.md` (any location) — must be referenced explicitly from AGENTS.md
- `.claude/settings.json` or `.claude/settings.local.json` — codex ignores `.claude/`
- Sub-tree AGENTS.md files outside the cwd path — only ancestors of cwd within the git repo

## Operating model

Codex is **terminal-first**. The primary surface is `codex` (interactive REPL) and `codex exec "<prompt>"` (one-shot). It does not have:
- A built-in plan mode equivalent (you compose plan-vs-execute manually via prompt)
- Hooks or lifecycle events (no pre-/post-tool-call interceptors)
- Slash commands (skills serve a similar role but are loaded, not invoked)
- Project-level memory beyond AGENTS.md + `.agents/skills/`

It does have:
- MCP server support (parity with Claude Code)
- Configurable model selection (default routes through OpenAI)
- Permission policies (whitelist/blacklist tool calls)
- Skill auto-loading (similar to Claude Code skills)

## Recommended workflow

1. Install codex once globally (`npm i -g @openai/codex`).
2. Create `~/.codex/AGENTS.md` if you have personal cross-project rules (optional).
3. In an aDNA project: ensure repo root has `AGENTS.md` pointing at `CLAUDE.md`.
4. `cd <project> && codex` — codex orients via AGENTS.md cascade.
5. Configure MCP servers in `~/.codex/config.toml` (or per-repo `.codex/config.toml`) — see `context_codex_mcp_setup.md`.

## Version expectations

aDNA's empirical AGENTS.md spec verification was done on **codex CLI 0.125.0** (campaign_codex_compatibility M00, 2026-04-28). Behavior for newer versions should be re-verified periodically — codex is in active development and CLI surface may shift. The cascade and skill-loading primitives have been stable across the 0.12x series.

## See also

- `context_codex_agents_md_spec.md` — empirical findings on cascade behavior
- `context_codex_mcp_setup.md` — MCP server configuration
- `context_codex_differences_from_claude_code.md` — feature-parity gaps and contrasts
- `how/skills/skill_codex_onboarding.md` — operational install + first-orient walkthrough

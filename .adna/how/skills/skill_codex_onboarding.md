---
type: skill
skill_type: process
created: 2026-04-28
updated: 2026-04-28
status: active
category: onboarding
trigger: "Manual — invoked by user (or AGENTS.md pointer) to install codex CLI and orient to an aDNA project"
last_edited_by: agent_herb
runtime: codex
tags: [skill, onboarding, codex, install, runtime]

requirements:
  tools: [npm, node, codex]
  context: ["AGENTS.md (this repo)", "CLAUDE.md (this repo)", ".adna/what/context/codex/"]
  permissions: ["install global npm packages", "create files in ~/.codex/", "edit .gitignore"]
---

# Skill: Codex CLI Onboarding

## Overview

Install OpenAI Codex CLI and orient it to an aDNA project. Mirror skill of `skill_machine_setup.md` for the codex runtime. Companion to `skill_onboarding.md` (project first-run) — codex onboarding is **runtime install**, not project customization.

This skill is runtime-specific. If you're setting up Claude Code instead, see the Claude-Code-side onboarding (typically embedded in `skill_machine_setup.md`).

## When to run

- First time using codex on a new machine
- After a major codex version bump (re-verify behavior)
- When adding codex support to a project that was previously Claude-Code-only

Not needed if `codex --version` already returns a current version AND `~/.codex/auth.json` shows valid credentials.

## Prerequisites

- Node.js (npm) installed. Verify: `node --version` (Node 18+).
- Either a ChatGPT Plus/Pro subscription OR an OpenAI API key.
- Git installed (codex AGENTS.md cascade depends on git-aware path resolution).

## Steps

### 1. Install codex CLI

```bash
npm i -g @openai/codex
codex --version
```

Expected: `codex-cli 0.125.0` or later. If install fails, check Node version; npm permissions issues usually resolve with a Node version manager (nvm/asdf) instead of `sudo npm`.

### 2. Authenticate

**ChatGPT subscription path:**

```bash
codex login
```

Opens a browser tab, walks you through OAuth, and writes `~/.codex/auth.json`. No further config needed for personal use.

**API key path:**

```bash
export OPENAI_API_KEY="sk-..."   # add to your shell rc for persistence
```

Or write `~/.codex/config.toml`:

```toml
[auth]
api_key = "sk-..."
```

Verify: `codex exec "what model are you running?"` should return without an auth error.

### 3. Verify AGENTS.md cascade in this project

```bash
cd <repo-root>
codex exec "report which AGENTS.md files you have loaded and which directories they came from"
```

Expected output: lists the repo-root AGENTS.md and any sub-directory AGENTS.md files between repo root and your `cwd`. If only `~/.codex/AGENTS.md` (or nothing) is reported, check that:
- The repo root has an `AGENTS.md` file (not just `.adna/AGENTS.md`)
- The repo is git-initialized (`git status` works)
- You're inside the repo, not above it

### 4. Configure MCP servers (if the project uses them)

See `what/context/codex/context_codex_mcp_setup.md`. Quick version:

- Global servers (used across projects): `~/.codex/config.toml`
- Project-only servers: `<repo>/.codex/config.toml`
- Per-machine secrets/paths: `<repo>/.codex/config.local.toml` — **add to `.gitignore`**

Smoke-test:

```bash
codex exec "list available MCP tools"
```

### 5. Verify `.gitignore` excludes per-machine config

If the project doesn't already exclude codex per-machine files:

```bash
echo ".codex/config.local.toml" >> .gitignore
git add .gitignore && git commit -m "gitignore: codex per-machine config"
```

Mirror the existing `.claude/settings.local.json` exclusion pattern if present.

### 6. (Optional) Set up global doctrine in `~/.codex/AGENTS.md`

If you have personal cross-project rules (preferred coding style, default branch behavior, communication tone), put them in `~/.codex/AGENTS.md`. Codex auto-loads this on every session, regardless of repo.

Keep it short (~30 lines). Per-project AGENTS.md and CLAUDE.md handle project-specific rules.

### 7. Read project doctrine

In an aDNA project, codex auto-loads `AGENTS.md` cascade — but **CLAUDE.md is not auto-loaded**. The root AGENTS.md should point you at CLAUDE.md; read it manually:

```bash
codex exec "read CLAUDE.md and summarize the agent protocol"
```

This is the codex equivalent of Claude Code's automatic CLAUDE.md load. Make it a habit on first orient.

## Verification

You're done when:

- `codex --version` returns a 0.125.0+ version
- `codex login` completed successfully OR API key works
- `codex exec "..."` runs without auth errors
- AGENTS.md cascade test (Step 3) reports the expected files
- MCP servers (if any) appear in `codex exec "list available MCP tools"`

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `command not found: codex` after install | npm global bin not in PATH | Add `$(npm prefix -g)/bin` to PATH |
| Auth error on every `codex exec` | Stale or missing `~/.codex/auth.json` | Re-run `codex login` |
| AGENTS.md not loading | Repo not git-initialized OR no root `AGENTS.md` | `git init` in repo OR add root AGENTS.md |
| MCP tool not appearing | Bad config syntax OR relative path in `command` | `python -c "import tomllib; tomllib.loads(open('.codex/config.toml').read())"` to validate; use absolute paths |
| Codex behaves differently from documented | Version drift (codex evolves quickly) | `codex --version` and re-run empirical tests in `what/context/codex/context_codex_agents_md_spec.md` |

## See also

- `what/context/codex/AGENTS.md` — full codex context library index
- `what/context/codex/context_codex_overview.md` — what codex is, version expectations
- `what/context/codex/context_codex_agents_md_spec.md` — empirical cascade behavior (the source of truth)
- `what/context/codex/context_codex_mcp_setup.md` — MCP setup deep-dive
- `what/context/codex/context_codex_differences_from_claude_code.md` — feature-parity gaps

## Provenance

Authored 2026-04-28 from `campaign_codex_compatibility` M01 (Agentic-DNA repo PR). Empirical findings sourced from M00 (codex CLI 0.125.0 on macOS, ChatGPT Plus OAuth path).

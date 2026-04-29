---
type: context_guide
topic: codex
subtopic: mcp_setup
created: 2026-04-28
updated: 2026-04-28
sources: ["campaign_codex_compatibility M00", "OpenAI codex docs (codex CLI 0.125.0)"]
context_version: "1.0"
token_estimate: ~2500
last_edited_by: agent_herb
runtime: codex
tags: [context, codex, mcp, config]
freshness_category: developing
last_evaluated: 2026-04-28
---

# Codex CLI — MCP Setup

> **Runtime scope:** codex CLI 0.125.0. MCP support is GA in codex; config schema is more recent than the cascade behavior and may shift between minor versions.

## What MCP buys you in codex

Same as in Claude Code: external tools (local executables, HTTP servers, network services) exposed as callable tool calls. Common MCP servers used in aDNA contexts:

- `gemini-image` — image generation
- `playwright` — browser automation
- Custom domain servers (e.g., `latlab/mcp/` for protocol-specific tools)

Both runtimes consume the same MCP server *implementations*; the difference is in how each runtime is *configured to launch* those servers.

## Where codex looks for MCP config

```
~/.codex/config.toml         # global, applies to all repos
<repo>/.codex/config.toml    # project-level, layered on top of global
```

Project-level config wins on collision. Per-machine overrides go in `<repo>/.codex/config.local.toml` (gitignore this — contains paths/credentials).

## Suggested config layout

```toml
# ~/.codex/config.toml or <repo>/.codex/config.toml

[mcp.gemini-image]
command = "node"
args = ["/absolute/path/to/gemini-image-mcp/dist/index.js"]
env = { GOOGLE_API_KEY = "..." }

[mcp.playwright]
command = "npx"
args = ["-y", "@playwright/mcp"]
```

## Sharing servers with `.claude/`

If a repo already has Claude Code MCP servers in `.claude/settings.json` or `.mcp.json`, you have three options:

| Strategy | When | Tradeoff |
|----------|------|----------|
| **Mirror** — duplicate server defs into `.codex/config.toml` | Stable, infrequently-changing servers | Two configs to keep in sync |
| **Single source via .mcp.json** — both runtimes read the standard `.mcp.json` (if codex supports it) | Fast-iteration MCP work | Verify codex `.mcp.json` support empirically per version; fall back to mirror |
| **Codex-only config** — `.codex/config.toml` is the canonical, `.claude/` references it | Codex-first projects | Forces Claude Code to learn the codex layout |

Default for aDNA-style multi-runtime repos: **mirror**, accept the duplication, document the canonical source-of-truth in the repo's CLAUDE.md.

## Smoke test

After configuring an MCP server, verify:

```bash
cd <repo>
codex exec "list available MCP tools and their descriptions"
```

Expected: codex returns a list including the configured server's tool surface. If a tool doesn't appear, troubleshoot in this order:
1. `codex --version` matches what you tested against
2. Config file path is correct (`~/.codex/config.toml` vs `.codex/config.toml`)
3. TOML syntax valid (`python -c "import tomllib; tomllib.loads(open('.codex/config.toml').read())"`)
4. Absolute paths in `command` / `args` (not relative)
5. Server itself runs standalone (`node /path/to/server.js` outputs MCP protocol on stdout)

## Permissions

Codex has its own permissions/allowlist system, conceptually parallel to Claude Code's `.claude/settings.json` permissions. Configure in `~/.codex/config.toml`:

```toml
[permissions]
# Allowlist commands that auto-approve (no per-call prompt)
allow = [
  "git status",
  "git diff",
  "ls",
  "rg *",
]

# Blocklist commands that always require explicit approval
deny = [
  "rm -rf",
  "git push --force",
]
```

Codex permissions are coarser than Claude Code's pattern matching — verify against current codex docs before trusting any specific syntax.

## Per-machine config

Anything machine-specific (absolute paths to executables, API keys via env vars, local-only servers) should go in `.codex/config.local.toml`, gitignored:

```
# .gitignore
.codex/config.local.toml
```

Mirror the `.claude/settings.local.json` exclusion pattern.

## See also

- `context_codex_overview.md` — install, auth
- `context_codex_agents_md_spec.md` — what auto-loads (MCP is separate from AGENTS.md cascade)
- `context_codex_differences_from_claude_code.md` — Claude vs codex MCP feature comparison
- `how/skills/skill_codex_onboarding.md` — operational walkthrough

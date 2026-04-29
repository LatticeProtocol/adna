---
type: directory_index
created: 2026-04-28
updated: 2026-04-28
token_estimate: 600
last_edited_by: agent_herb
runtime: codex
tags: [directory_index, context, codex]
---

# Codex — Context Index

## Overview

Technical knowledge about the **OpenAI Codex CLI** operating environment for aDNA projects. Mirror of `what/context/claude_code/` for the codex runtime. This topic is **runtime-specific** — it documents how codex implements (or differs from) the patterns aDNA relies on: AGENTS.md cascade, MCP, skills, and config layout.

This topic is part of a **cross-triad domain** (see ADR-003). Governance and operational components live in their triad-correct locations; this directory holds the WHAT (technical knowledge) portion:

| Component | Triad | Location |
|-----------|-------|----------|
| Agent protocol | WHO | `who/governance/governance_agent_protocol.md` |
| Codex onboarding | HOW | `how/skills/skill_codex_onboarding.md` |
| AGENTS.md cascade behavior | WHAT | this directory |
| MCP setup (codex side) | WHAT | this directory |
| Differences from Claude Code | WHAT | this directory |

## Subtopics

| # | Subtopic | File | ~Tokens | Subtype | Key Content |
|---|----------|------|---------|---------|-------------|
| 1 | Overview | `context_codex_overview.md` | ~1,500 | context_guide | What codex is, install, auth, version expectations |
| 2 | AGENTS.md spec | `context_codex_agents_md_spec.md` | ~3,000 | context_core | Empirical findings F-01–F-07: cascade walk, concat order, git-awareness, precedence |
| 3 | MCP setup | `context_codex_mcp_setup.md` | ~2,500 | context_guide | `.codex/` config, MCP server sharing with `.claude/`, smoke test |
| 4 | Differences from Claude Code | `context_codex_differences_from_claude_code.md` | ~4,000 | context_core | Plan mode, hooks, slash commands, skills loading, parity gaps |

## Total Token Budget

~11,000 tokens to load all subtopics. Typical session loads 1-2 subtopics (~3K-5K tokens).

## Usage by Task

| Task | Load |
|------|------|
| First-time codex install | overview, `how/skills/skill_codex_onboarding.md` |
| Writing a root AGENTS.md | agents_md_spec, overview |
| Setting up `.codex/` config | mcp_setup, overview |
| Migrating Claude-Code skills to codex | differences_from_claude_code, agents_md_spec |
| Debugging "why didn't codex load X?" | agents_md_spec |
| Comparing runtime parity for ADR-011 | differences_from_claude_code |

## Dependency Notes

- **agents_md_spec** is the empirical-behavior reference — load before any AGENTS.md authoring decision. Source data lives in the originating campaign's M00 artifacts (`campaign_codex_compatibility/artifacts/m00_smoke/`).
- **differences_from_claude_code** depends on `claude_code/` for the comparison side. Load both for full picture.
- **mcp_setup** is independent of the others — load only when configuring `.codex/` or `.mcp.json`.

## Load/Skip Decision

**Load when**:
- Configuring or onboarding a codex CLI environment
- Writing or reviewing AGENTS.md content (root or sub-directory)
- Debugging codex orientation, skill-loading, or MCP issues
- Drafting an ADR/RFC about runtime parity

**Skip when**:
- Working in a Claude-Code-only project — load `claude_code/` instead
- Performing routine domain work where runtime details don't apply

**Skip entirely** when running aDNA on a non-codex runtime — this topic documents codex specifics.

**Token cost**: ~600 tokens (this AGENTS.md)

## Provenance

Established 2026-04-28 from `campaign_codex_compatibility` M00 empirical findings (codex CLI 0.125.0 on macOS via `npm i -g @openai/codex`, ChatGPT Plus OAuth). Re-verify behavior at major codex version bumps.

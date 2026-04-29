---
type: context_core
topic: codex
subtopic: agents_md_spec
created: 2026-04-28
updated: 2026-04-28
sources: ["campaign_codex_compatibility M00 — 6 raw codex transcripts on Herb's Mac Pro M1 Max, codex CLI 0.125.0"]
context_version: "1.0"
token_estimate: ~3000
last_edited_by: agent_herb
runtime: codex
tags: [context, codex, agents_md, cascade, empirical]
freshness_category: stable
last_evaluated: 2026-04-28
---

# Codex AGENTS.md — Cascade Behavior

> **Runtime scope:** Empirical behavior on codex CLI 0.125.0. Re-verify at major version bumps (0.13x+, 1.0+). Source: 6 marker-string smoke tests in `campaign_codex_compatibility/artifacts/m00_smoke/` (2026-04-28).

## Summary

Codex auto-loads **AGENTS.md files along the path from `git-root` to `cwd`** at session start, concatenates them root-first into the system prompt, and treats leaf rules as overriding root rules where they collide. CLAUDE.md is **not** auto-loaded. Sibling subtrees and descendants of `cwd` are **not** loaded.

## Findings (F-01 through F-07)

### F-01 — Walk direction is git-root → cwd, not cwd → root

When codex starts in `<git-repo>/sub/leaf/`, it loads:
1. `<git-repo>/AGENTS.md` (if present)
2. `<git-repo>/sub/AGENTS.md` (if present)
3. `<git-repo>/sub/leaf/AGENTS.md` (if present)

It does **not** load `<git-repo>/sibling/AGENTS.md` or `<git-repo>/sub/leaf/deeper/AGENTS.md`.

**Implication for design**: a root AGENTS.md per repo is **necessary** for orientation when codex starts at the repo root. Existing sub-directory AGENTS.md trees (e.g., `.adna/what/AGENTS.md`) load only when codex is started in those subtrees — they do not surface when codex is started at the repo root.

### F-02 — Concat order is root-first; leaf overrides root

The system prompt receives the AGENTS.md files in root-to-leaf order. When two files declare conflicting rules on the same topic, the leaf (deepest) takes precedence. This is consistent with model attention bias toward later context.

**Implication for content**: root AGENTS.md should set defaults and pointers (to CLAUDE.md, to onboarding skill); deeper AGENTS.md should carry directory-specific overrides.

### F-03 — CLAUDE.md is NOT auto-loaded

Codex makes no attempt to load any file named `CLAUDE.md` automatically. Doctrine in CLAUDE.md is invisible to codex unless an AGENTS.md explicitly tells it to read CLAUDE.md or duplicates the content.

**Implication for aDNA**: every aDNA repo that wants codex parity needs an `AGENTS.md` referencing `CLAUDE.md` as canonical doctrine. AGENTS.md should not duplicate CLAUDE.md (per aDNA Standing Order #3 in campaign CLAUDE.md), but should hand off explicitly: *"Read CLAUDE.md for canonical doctrine."*

### F-04 — Cascade is git-aware

If `cwd` is **not** inside a git repo, codex loads only `~/.codex/AGENTS.md` (global) and any `AGENTS.md` literally in `cwd`. The "root → cwd" walk depends on a `.git/` directory marking the repo root.

**Implication for fixtures and tests**: AGENTS.md cascade tests must run inside a git-initialized directory tree to exercise the realistic loading behavior.

### F-05 — Skills load from two paths

Codex auto-loads:
- **Global**: `~/.codex/skills/.system/<name>/SKILL.md`
- **Project**: `.agents/skills/<name>/SKILL.md` in cwd or ancestors

Both load at session start; both inject SKILL.md content into the available skill set.

**Implication for aDNA**: aDNA-shipped skills can live at `.agents/skills/` for project-level codex visibility (separate from `how/skills/` which is the human-readable skill library). Lattice-labs already uses this pattern with `termy-obsidian-context`.

### F-06 — `.agents/skills/` documentation status

The `.agents/skills/` path was confirmed working empirically. Public codex docs as of 0.125.0 list `~/.codex/skills/` but do not explicitly document `.agents/skills/` as a project-level convention. Treat the project-level path as **descriptive** behavior, not contractual — recheck at codex version bumps.

### F-07 — Token economy

Empirical token costs:
- Small AGENTS.md (~50 lines, ~500 tokens): cheap, suitable for root entry points
- 3-file nested cascade (root + sub + leaf, ~50 lines each): ~1,500 tokens
- 5 global skills + 3 nested AGENTS.md combined: ~7,200 tokens

Plenty of headroom in modern context windows. Keep root AGENTS.md ≤ ~50 lines for cleanliness, not budget reasons.

## Untested behavior (open follow-ups)

| # | Question | Status |
|---|----------|--------|
| FU-01 | Truncation behavior on AGENTS.md > N lines (does codex truncate, refuse, or warn?) | Untested as of 2026-04-28 |
| FU-02 | Multi-AGENTS.md merge with structured rules (lists, tables, numbered standing orders) — does precedence apply field-by-field or whole-file? | Untested |
| FU-03 | `~/.codex/AGENTS.md` global slot — present in docs, not yet exercised on a real machine | Pending personal-doctrine slot use |

These are flagged as `COULD` requirements in `campaign_codex_compatibility/planning/02_prd.md` (R15-R17). Resolution opportunistic.

## Source data

Raw transcripts: `campaign_codex_compatibility/artifacts/m00_smoke/01-06_*.txt` — six marker-string tests using `ROOT_MARKER`, `SUB_A_MARKER`, `SUB_B_MARKER`, and `PRECEDENCE_KEY` to disambiguate which files loaded and in what order.

## See also

- `context_codex_overview.md` — what codex is, install, auth
- `context_codex_differences_from_claude_code.md` — how this cascade differs from Claude Code's
- `how/skills/skill_codex_onboarding.md` — operational install
- `campaign_codex_compatibility/context/codex_agents_md_behavior.md` (lattice-labs vault) — original campaign-internal capture of the same findings, includes raw transcript references

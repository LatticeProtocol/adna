---
type: governance
subtype: campaign_agents
created: YYYY-MM-DD
updated: YYYY-MM-DD
last_edited_by: agent_{username}
runtime: codex
tags: [governance, campaign, agents_md]
---

# AGENTS.md — Campaign: {campaign_name}

This is the **codex-native entry point** for this campaign. Canonical doctrine lives in `CLAUDE.md` (sibling file). Read it for the full operating protocol; this file is a thin pointer.

## Quick Start

1. Read `CLAUDE.md` — full campaign doctrine (Standing Orders, Quick Start, Key Files)
2. Read `campaign_{name}.md` — master campaign document
3. Check current mission status in the phase table
4. Claim next unclaimed objective and create a session file

## Codex-specific notes

- Codex does **not** auto-load `CLAUDE.md`. Read it explicitly per Quick Start step 1.
- Sub-directory `AGENTS.md` files in this campaign (if any) load automatically as you `cd` into them — codex walks git-root → cwd, root-first concat.
- For codex install / auth / MCP setup, see `.adna/how/skills/skill_codex_onboarding.md`.

## Standing Orders

Same as `CLAUDE.md`. If they conflict, CLAUDE.md wins — `AGENTS.md` is a runtime adapter, not a parallel rulebook.

## Delegation Notes

Same as `CLAUDE.md`. Read the campaign-level CLAUDE.md for what's been done, what's in progress, and any non-obvious decisions.

## See also

- `CLAUDE.md` — canonical campaign doctrine
- `campaign_{name}.md` — master document
- `missions/` — mission files
- `artifacts/` — campaign deliverables

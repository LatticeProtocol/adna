---
type: migration
source_version: "X.Y"
target_version: "X.Z"
scope: governance|content|structure|mixed
risk_level: low|medium|high
affected_files:
  - CLAUDE.md
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: draft
last_edited_by: agent_<username>
tags: [migration]
---

# Migration: vX.Y → vX.Z

<!-- Template guidance (remove these comments when filling):
  - Always use pattern matching, never line numbers — user files may differ
  - Describe both the "find" pattern and the "replace" pattern for each change
  - One file per step section — makes rollback granular
  - Include micro-verification after each step (a quick check the change applied)
  - Pre-flight and post-flight are mandatory — they prevent partial migrations
-->

## Summary

**What changed**: [Brief description of what's different between versions]
**Why**: [Motivation — what problem does this version solve or what capability does it add]
**Impact**: [What the user will notice after migrating — new features, changed behavior, etc.]

## Pre-Flight Checks

Before starting, verify all of the following:

1. **Version verification**: Read `CLAUDE.md` frontmatter. Confirm `version: "X.Y"`. If the version does not match, STOP — this migration does not apply.
2. **Clean working state**: Run `git status`. If there are uncommitted changes, ask the user to commit or stash first. Migrations should be their own atomic commit.
3. **Read affected files**: Read each file listed in `affected_files` frontmatter. Confirm the expected patterns exist before attempting to modify them.

<!-- Add additional pre-flight checks specific to this migration -->

## Changes

### File: CLAUDE.md

<!-- Describe each change as a find→replace pattern:
  - **Find**: The exact text pattern to locate (use surrounding context for uniqueness)
  - **Replace**: The new text to substitute
  - **Why**: Brief rationale for this specific change
-->

| # | Find | Replace | Why |
|---|------|---------|-----|
| 1 | `version: "X.Y"` | `version: "X.Z"` | Version bump |
| 2 | `<!-- vX.Y | ... -->` | `<!-- vX.Z | YYYY-MM-DD -->` | Version comment |

<!-- Repeat for each affected file -->

## Migration Steps

<!-- Number each step. One atomic operation per step. Micro-verify after each. -->

### Step 1: Update CLAUDE.md

1. Open `CLAUDE.md`
2. In frontmatter, find `version: "X.Y"` and replace with `version: "X.Z"`
3. Find the version comment `<!-- vX.Y |` and replace with `<!-- vX.Z | YYYY-MM-DD -->`
4. **Verify**: Read CLAUDE.md frontmatter. Confirm `version: "X.Z"`.

<!-- Add steps for each affected file -->

## Post-Flight Validation

After all steps are complete, verify:

- [ ] `CLAUDE.md` frontmatter shows `version: "X.Z"`
- [ ] Version comment at top of CLAUDE.md shows `vX.Z`
- [ ] Version comment at bottom of CLAUDE.md shows `vX.Z` (if present)

<!-- Add validation checks specific to this migration -->

All checks passing = migration complete. Commit the changes:
```
git add -A && git commit -m "chore: migrate aDNA vault from vX.Y to vX.Z"
```

## Rollback

If the migration fails or produces unexpected results, revert per-file:

**CLAUDE.md**: Reverse each find/replace from the Changes table (swap Find and Replace columns).

**Git shortcut** (if you committed before discovering the issue):
```
git revert HEAD
```

**Git shortcut** (if you haven't committed yet):
```
git checkout -- CLAUDE.md
```

## Related

- [Migration Registry](../migrations/AGENTS.md) — All available migrations
- [Version Migration Skill](../skills/skill_version_migration.md) — Guided upgrade workflow
- [Version Migration Guide](../../what/docs/version_migration_guide.md) — How to create migration prompts

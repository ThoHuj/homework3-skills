---
name: harmonize-project-skills
description: >-
  Audits project skills under .cursor/skills/ for structure, quality, overlap,
  and integration. Validates README and PROJECT-BRIEF alignment, reports issues
  by severity, and fixes approved items. Use when the user invokes
  harmonize-project-skills, asks to audit, harmonize, or streamline skills,
  check skill file structure, reduce skill friction, or verify skills work well
  together.
disable-model-invocation: true
---

# Harmonize Project Skills

Audit and tune the project's skill stack so skills are reliable, distinct, and
work together without friction.

**Scope:** `.cursor/skills/`, plus README and `.cursor/PROJECT-BRIEF.md` only
where they describe skills.

## When to run

Only when the user explicitly invokes this skill or asks to audit, harmonize,
validate, or streamline project skills. Do not auto-run on unrelated tasks.

Run after adding or changing skills, before submission, or when skills feel
inconsistent or unreliable.

## Phase 1: Inventory

1. List every directory under `.cursor/skills/`
2. For each skill, note:
   - Files present (`SKILL.md` required; supporting files optional)
   - Frontmatter fields (`name`, `description`, `disable-model-invocation`)
   - Approximate `SKILL.md` line count
   - Pattern type (interview, checklist, auto-read, auto-write, etc.)
   - Whether README and brief mention it

Use Glob or shell listing — do not rely on memory or chat history alone.

## Phase 2: Audit

Work through [checklist.md](checklist.md) for every skill and for the stack as a
whole. Record each finding with:

- **Severity:** Critical / Warning / Suggestion
- **Location:** skill name and file path
- **Issue:** what is wrong or risky
- **Fix:** concrete change (or "none — informational")

### Cross-skill checks (required)

These catch friction that single-skill review misses:

| Check | What to look for |
|-------|------------------|
| Overlap | Two skills with the same primary job or duplicate triggers |
| Gaps | Brief/README reference a skill that does not exist, or a skill is undocumented |
| Invocation model | Explicit skills missing `disable-model-invocation: true`; ambient skills that should not be user-only |
| Handoff chain | `discover-project-goal` → brief → `maintain-project-docs` references intact |
| Terminology | Same concept named differently across skills (e.g. "brief" vs "project doc") |
| File references | Broken relative links; references more than one level deep |
| Doc drift | README layout tree, skills sections, and brief architecture snapshot out of sync |

### Standards (quick reference)

- Skill dir: `.cursor/skills/<name>/` with `SKILL.md`
- `name` in frontmatter matches directory name; lowercase, hyphens, ≤64 chars
- `description`: third person, WHAT + WHEN, ≤1024 chars, includes trigger terms
- `SKILL.md` body: concise, ≤500 lines; progressive disclosure for detail
- Explicit user skills: `disable-model-invocation: true`
- Ambient context skills: omit that field (auto-invoke when relevant)
- Each skill has a "Relationship to other skills" (or equivalent) when others exist
- No secrets; no `~/.cursor/skills-cursor/` paths

Full criteria: [checklist.md](checklist.md).

## Phase 3: Report

Present findings using [report-template.md](report-template.md).

Rules:

- Lead with a one-paragraph **summary** (skill count, critical count, overall health)
- Group by severity; Critical first
- If no issues: say so explicitly and note any Suggestions worth optional polish
- Do not dump full skill bodies into chat

Ask the user how to proceed:

1. **Report only** — stop after the audit
2. **Fix approved** — apply fixes for Critical and Warning items the user approves
3. **Fix all safe** — apply Critical, Warning, and non-controversial Suggestions without per-item approval

Default to option 2 if the user did not specify.

## Phase 4: Fix (when approved)

Apply fixes in this order:

1. **Structure** — missing files, wrong directory names, broken frontmatter
2. **Skill content** — descriptions, relationship sections, broken links, invocation flags
3. **Cross-skill** — dedupe overlap, align terminology, clarify boundaries in Relationship tables
4. **Docs** — follow `maintain-project-docs` write rules: patch README layout, skill sections,
   brief architecture snapshot, key decisions, and next steps

After edits:

1. Re-run the checklist mentally on changed files — confirm Critical/Warning items resolved
2. Tell the user what changed in a short bullet list (files touched, issues fixed)
3. If structural or skill-list changes were made, note that docs were synced

Do not rewrite skills for style preference alone unless the user asked for polish.

## Phase 5: Re-audit (optional)

If many fixes were applied, offer a quick second pass on the checklist items that
failed before. One pass is usually enough.

## Relationship to other skills

| Skill | Role |
|-------|------|
| `harmonize-project-skills` | Audits skill quality and cohesion (this skill) |
| `discover-project-goal` | Creates project brief — not a substitute for skill audit |
| `maintain-project-docs` | Reads brief before work and keeps brief/README current; run its write rules after harmonize fixes docs |
| `grill-me` | Topic-scoped interview loop; distinct from full discovery |
| `finish-ticket` | Per-ticket completion checklist; distinct from stack-wide audit |

When harmonize finds doc staleness, fix docs using `maintain-project-docs` write workflow
and [doc-map.md](../maintain-project-docs/doc-map.md) — do not run a full discovery interview.

## Examples

**User:** `Use harmonize-project-skills`

**Agent:** Inventories skills → runs checklist → reports 1 Critical (README missing
new skill), 2 Warnings → asks fix scope → user approves Critical + Warnings → fixes
skill section and layout tree → confirms re-audit clean.

**User:** `Audit our skills but don't change anything yet`

**Agent:** Full audit and report only; stops at Phase 3.

**User:** `Streamline skills — fix everything that's safe`

**Agent:** Audit → Fix all safe → sync docs → summarize changes.

---
name: maintain-project-docs
description: >-
  Reads .cursor/PROJECT-BRIEF.md before non-trivial work to align with project
  goals, constraints, and priorities, and writes back durable facts — new
  decisions, scope changes, new skills — into the brief and README as they
  emerge. Use when starting features, refactors, or multi-file changes; when
  scope is unclear; when a request may conflict with stated goals; or when the
  user shares or confirms project direction, a decision, or a skill change.
---

# Maintain Project Docs

Keeps the agent aligned with project direction and keeps the record accurate,
in both directions: read before acting, write when you learn something durable.

**File:** `.cursor/PROJECT-BRIEF.md` (produced by `discover-project-goal`),
plus `README.md` for human-facing skill documentation.

## Part 1 — Read before work

Read `.cursor/PROJECT-BRIEF.md` when project context would change your approach:

- Starting a non-trivial task (feature, refactor, multi-file change)
- Before suggesting approaches, features, or architecture
- Goals, scope, or priorities are unclear
- The user's request may conflict with stated goals, non-goals, or constraints

Do **not** read on every message. Read on demand when triggers above apply, and
skip entirely for trivial tasks (typo, simple factual question, single-line
fix) or when you already read the brief this session and it has not changed.

Apply these sections when relevant: **TL;DR/Vision** (direction), **Current
phase** (what matters now), **Priorities** (P0 before P1 before P2),
**Constraints** (hard boundaries), **Recommended next steps**. Don't dump the
brief into chat — use it to guide action and name it briefly when it informs a
decision, e.g. "The brief lists application code as out of scope."

### Conflict handling

If the request contradicts the brief, flag it before proceeding: name the
specific conflict, then ask whether to adjust the request, update the brief, or
override for this task only. Never silently ignore the brief.

### Missing brief

If `.cursor/PROJECT-BRIEF.md` does not exist: say so, suggest
`discover-project-goal`, and proceed without it only for trivial tasks.

## Part 2 — Write when facts change

**Do not wait** for the user to ask — update `.cursor/PROJECT-BRIEF.md` and
`README.md` when you learn or confirm **durable** facts:

- Goals, scope, non-goals, or current phase change
- Priorities shift (P0/P1/P2 reorder or new items)
- A key decision is made or superseded
- A skill is added, renamed, merged, or materially changed
- Repo layout under `.cursor/skills/` changes
- Recommended next steps are completed or replaced
- README or brief content is stale relative to the repo

Skip for one-off task-only instructions, facts already captured correctly,
unconfirmed guesses (ask first), trivial chat, or anything touching secrets or
credentials (never write those anywhere).

Use [doc-map.md](doc-map.md) to decide which file and section to edit.
Rule of thumb: **brief** = why, what matters now, constraints, decisions, next
agent actions; **README** = how to use the repo (skill list, patterns,
invocation, layout). Overlap is fine in TL;DR/skills table; avoid duplicating
full skill write-ups in the brief.

### Update workflow

1. Read both files (and list `.cursor/skills/` if skills may have changed)
2. Identify stale or missing content from the new fact
3. Edit minimally — only affected sections, preserving structure and wording
4. Bump `Last updated` in the brief header
5. Tell the user in one line what synced, e.g. "Updated brief (phase, next
   steps) and README (added `finish-ticket`)."

If a new fact contradicts existing brief/README content: flag it, prefer the
user's latest explicit statement, update both files if both are affected. For
large direction pivots, suggest `discover-project-goal` for a full refresh, but
still patch obvious staleness in the same session if confirmed.

## Relationship to other skills

| Skill / file | Role |
|--------------|------|
| `discover-project-goal` | Full interview → initial or major brief rewrite |
| `maintain-project-docs` | Reads brief before work; writes brief + README incrementally (this skill) |
| `harmonize-project-skills` | Audits skill stack; after fixes, use this skill's write rules to sync docs |
| `grill-me` | Topic interview loop; applies this skill's write rules when decisions are durable |
| `finish-ticket` | May trigger a doc sync (e.g. ticket-tracking decisions) via this skill's write rules |
| `.cursor/PROJECT-BRIEF.md` | Authoritative source for goals and direction |

## Examples

**User:** "Add a Flask API to this project."
**Action:** Read brief — application code is out of scope. Flag the conflict,
ask how to proceed.

**User:** "Let's defer the git workflow skill and focus on `finish-ticket`
first."
**Action:** Update brief priorities and next steps; update README planned
table. Report what changed in one line.

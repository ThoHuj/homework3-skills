---
name: discover-project-goal
description: >-
  Interviews the user to discover and document a project's main goal, vision,
  constraints, and direction. Writes a detailed handoff brief another agent can
  read to understand where the project is heading. Use when the user invokes
  discover-project-goal, asks to define project goals, capture project vision,
  or create a project brief for agent handoff.
disable-model-invocation: true
---

# Discover Project Goal

Run a structured discovery session to learn what this project is for, where it
is heading, and what matters most. Output is a durable brief for the next agent.

**Output file:** `.cursor/PROJECT-BRIEF.md`

## When to run

Only when the user explicitly invokes this skill or asks for project-goal
discovery. Do not auto-run on unrelated tasks.

## Phase 1: Reconnaissance (agent work)

Before asking anything, gather facts from the repo and environment:

- README, docs, issue trackers, comments, config files
- Directory layout, languages, frameworks, dependencies
- Existing `.cursor/PROJECT.md` or `.cursor/PROJECT-BRIEF.md` if present
- Git history or commits when useful for intent signals

Extract what you can infer: stack, maturity, apparent domain, gaps in docs.
Do **not** ask the user for facts you can look up.

Summarize findings internally. Use them to ask sharper questions, not to
guess goals the user has not stated.

## Phase 2: Interview (user decisions)

Interview in **rounds** until the frontier of open questions is empty.

### The frontier

Treat discovery as a tree. Each answered question unlocks follow-ups. The
**frontier** is every question whose prerequisites are already settled.

Typical branches (adapt to what reconnaissance revealed):

| Branch | Example questions |
|--------|-------------------|
| Purpose | What problem does this solve? Who is it for? |
| Vision | What does success look like in 3–6 months? |
| Scope | In scope vs out of scope for the current phase |
| Priorities | Must-haves vs nice-to-haves; what to build first |
| Constraints | Stack, budget, timeline, compliance, non-negotiables |
| Quality bar | Performance, reliability, UX, testing expectations |
| Risks | Known unknowns, dependencies, blockers |
| Handoff | What should the next agent do first? |

Skip branches already clear from repo facts or prior answers.

### AskQuestion rules

Use the **AskQuestion** tool for each round:

- One `AskQuestion` call per round
- One question object per frontier item (batch related questions)
- Do **not** duplicate the same choices as a numbered list in chat

For each question:

| Field | Guidance |
|-------|----------|
| `prompt` | `<Topic>: <question>` — concise, direct |
| `options` | Use when natural choices exist. Put recommended answer **first** with `(Recommended)`. Give the reason in the prompt, not the label. |
| `allow_multiple` | `true` only when several options legitimately apply together |

Include `Something else (I will type it)` when fixed choices may not fit.

When AskQuestion is unavailable, ask in prose — one block per frontier item,
each with a recommended answer and brief reason.

**Facts vs decisions:** only decisions go to the user. Look up codebase facts
yourself.

After each round, wait for answers, update the tree, recompute the frontier,
and continue until nothing meaningful remains open.

### Minimum coverage

Before finishing, confirm you have explicit user input (not inference alone)
on at least:

1. Primary goal and intended audience
2. Current phase / near-term direction
3. Success criteria
4. Hard constraints or non-goals
5. Recommended first step for the next agent

If any are missing, ask one more round.

## Phase 3: Write the brief

When the interview is complete, write or overwrite `.cursor/PROJECT-BRIEF.md`
using [template.md](template.md).

### Writing rules

- Write for a **cold-start agent** with no chat history
- Use the user's words where possible; paraphrase only for clarity
- Separate **confirmed facts** (from repo) from **stated goals** (from user)
- Mark inferred items as `[Inferred — confirm with user]` sparingly; prefer
  asking over guessing
- Be specific: names, metrics, deadlines, and concrete next steps beat vague prose
- Do not store secrets, API keys, or credentials
- Set `Last updated` to today's date
- Keep sections scannable: short paragraphs, bullets, tables where helpful

### After writing

Tell the user briefly:

1. Where the file was written
2. One-sentence summary of the project's direction
3. Whether they should commit the brief for team handoff (optional but useful)

Offer to refine the brief if anything is wrong. Do not start implementation
unless the user asks.

## Relationship to other skills

| Skill / file | Role |
|--------------|------|
| `.cursor/PROJECT-BRIEF.md` | Output of this skill — comprehensive goal/direction handoff |
| `maintain-project-docs` | Reads the brief before non-trivial work; keeps brief and README current when durable facts change |
| `harmonize-project-skills` | Audits skill quality after adding skills; not a substitute for discovery |
| `grill-me` | Topic-scoped interview loop; may trigger incremental doc sync |
| `finish-ticket` | Completion checklist run per ticket; independent of project-wide discovery |
| `.cursor/PROJECT.md` | Incremental session memory (project-memory skill); may overlap |

If both exist, PROJECT-BRIEF is the authoritative source for goals and
direction; PROJECT.md holds ongoing tactical context. When writing the brief,
incorporate relevant facts from PROJECT.md without duplicating stale content.

## Re-running

When invoked again, read the existing brief first. Interview only on what
changed, is unclear, or conflicts with the repo. Merge updates; do not erase
still-valid content without user confirmation.

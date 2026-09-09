---
name: grill-me
description: >-
  Interview the user in rounds about a plan or decision until every branch of
  the design tree is settled, using AskQuestion for structured choices. Use when
  the user invokes grill-me, asks to be grilled, or wants to remove open
  questions about a topic before building starts.
disable-model-invocation: true
---

# Grill Me

Stress-tests a plan or decision by interviewing its owner until nothing is
left silently assumed. Works on any topic — a feature plan, architecture choice,
scope change, or skill design. The user names or implies the topic when invoking;
read conversation context to bound what is in scope.

## When to run

Only when the user explicitly invokes this skill or asks to be grilled. Do not
auto-run on unrelated tasks. Do not offer a grill unprompted — wait for the
user to request it.

## Phase 1: Scope the topic

Before asking anything:

1. **Identify the topic** from the user's message and recent conversation —
   what plan, decision, or open question are we settling?
2. **Gather facts** the agent can answer (codebase, repo, environment). Use a
   subagent when useful. Do not ask the user for lookup-able facts.
3. **Read project context** when relevant — `.cursor/PROJECT-BRIEF.md`, README,
   related skills — so questions respect existing decisions and constraints.
4. **State the scope** in one sentence in chat before the first round, e.g.
   "Grilling: auth approach for the API." Let the user correct scope if wrong.

If no topic is clear, ask one short clarification question before starting the
loop.

## Phase 2: The interview loop

Run rounds until the frontier is empty. Each round is: **scan → ask → wait →
repeat**.

### The tree

Map the topic as a design tree: every decision branches into the decisions
that hang off it. The **frontier** is every question whose prerequisites
are already settled — askable now without guessing at answers not yet heard.

Include both explicit unknowns and implicit assumptions that could change the
outcome. Drop questions already answered in this conversation or settled in
the project brief unless the user is revisiting them.

### AskQuestion rules

Each round asks the whole frontier via the **AskQuestion** tool:

- One `AskQuestion` call per round
- One question object per frontier item (batch when the frontier has several)
- Do **not** repeat the same choices as a numbered list in chat — the
  AskQuestion UI is the question surface

For each question:

| Field | Guidance |
|-------|----------|
| `prompt` | `<title>: <question>` — concise, in the user's language |
| `options` | Use when there are natural choices. Put the recommended answer **first**, with `(Recommended)` at the end of the label. Give the reason in the prompt, not the label. |
| `allow_multiple` | `true` only when several options can legitimately apply together |

When fixed choices might not cover the situation, include
`Something else (I will type it)` as an option.

For decisions without obvious fixed choices, propose the most likely options
inferred from context, mark the best guess `(Recommended)`, and include the
freeform escape. If you truly cannot offer two meaningful options, ask that
one item in prose and keep the rest of the round on AskQuestion.

If AskQuestion is unavailable, fall back to numbered prose — one block per
frontier item, each with a recommended answer:

    **Q1** — **<title>**: <question, with options when there are natural ones>

    **Recommended:** <recommended answer + reason in a sentence or two>

Grill in the user's language.

### Facts vs decisions

Facts are the agent's job; decisions are the user's. Only decisions go to the
user via AskQuestion.

### Loop control

After each round:

1. Wait for answers
2. Update the tree — settled decisions unblock new questions
3. Recompute the frontier
4. If frontier non-empty → next round
5. If frontier empty → Phase 3

A question whose answer depends on another question still open this round
belongs to a later round.

## Phase 3: Summary

When the frontier is empty, confirm shared understanding with the user. Write
a compact summary — in English, in chat, no file:

- **Decided** — each decision, one line
- **Rejected / parked** — what was deliberately not chosen or postponed

Keep the rejected/parked list concrete — it feeds out-of-scope sections in
later planning.

If anything still feels ambiguous, run one more round before closing.

## Phase 4: Sync project docs

If the session produced **durable project facts** — goals, scope, priorities,
key decisions, new or changed skills, resolved open questions — apply the
`maintain-project-docs` write workflow in the same turn:

1. Read [maintain-project-docs/SKILL.md](../maintain-project-docs/SKILL.md) and
   [doc-map.md](../maintain-project-docs/doc-map.md)
2. Patch `.cursor/PROJECT-BRIEF.md` and `README.md` minimally
3. Tell the user in one line what synced

Skip doc updates when the topic was purely local to the current task with no
project-wide impact (e.g. naming one variable, a one-off refactor approach).

When in doubt whether a fact is durable, ask one short confirmation before
writing.

## Boundary

The skill ends after the summary and optional doc sync. If the outcome is a
plan for work, offer the next step — the user's yes starts it, never the offer
itself.

Never start building and never create tickets, plan files, or implementation
artifacts yourself unless the user explicitly asks after the summary.

## Relationship to other skills

| Skill | Role |
|-------|------|
| `discover-project-goal` | Full project discovery → writes or rewrites the brief from scratch |
| `grill-me` | Topic-scoped interview loop → summary; may trigger incremental doc sync |
| `maintain-project-docs` | Reads brief before other work; write rules used in Phase 4 |
| `harmonize-project-skills` | Audits skill quality; not a substitute for settling decisions |
| `finish-ticket` | Runs after building, once the plan from a grill session is implemented |

Use `discover-project-goal` when the whole project direction needs capture or
a major refresh. Use `grill-me` when a specific plan or decision in the
current conversation still has open questions.

# homework3-skills

A Cursor Agent Skills homework project. This repo contains project-scoped skills
under `.cursor/skills/` — reusable instructions that teach the Cursor agent how
to perform specific workflows.

**Goal:** Build 3–5 skills with varied patterns, each documented here with clear
invocation examples. No application code — skills and documentation only.

## Prerequisites

- [Cursor](https://cursor.com) with Agent Skills enabled
- Open this repo as your workspace so project skills are discovered from
  `.cursor/skills/`

## Project layout

```
homework3-skills/
├── README.md                          # This file
├── .gitignore                         # Standard Python template (no app code)
├── .cursor/
│   ├── PROJECT-BRIEF.md               # Generated project handoff doc
│   └── skills/
│       ├── maintain-project-docs/
│       │   ├── SKILL.md               # Read brief before work; keep brief and README current
│       │   └── doc-map.md             # Which facts go in which file
│       ├── discover-project-goal/
│       │   ├── SKILL.md               # Skill instructions
│       │   └── template.md            # Output template for the brief
│       ├── harmonize-project-skills/
│       │   ├── SKILL.md               # Audit and tune the skill stack
│       │   ├── checklist.md           # Per-skill and stack-wide audit criteria
│       │   └── report-template.md     # Structured audit report format
│       ├── grill-me/
│       │   └── SKILL.md               # Interview loop until a topic is settled
│       ├── finish-ticket/
│       │   └── SKILL.md               # Completion checklist: review, test, PR, ticket update
│       └── write-jira-tickets/
│           └── SKILL.md               # Ticket authoring + commit/branch linkage convention
```

## How to invoke a skill

Skills in `.cursor/skills/` are available to the Cursor agent when this project
is open. To run a skill, mention it explicitly in the Agent chat:

```
Use discover-project-goal
```

You can also describe the intent in natural language — the agent matches your
request to the skill's description:

```
Run project goal discovery
Create a project brief for agent handoff
Define what this project is for
```

Some skills run only when you ask for them (`disable-model-invocation: true`);
others apply automatically when relevant (e.g. `maintain-project-docs`, `finish-ticket`).

---

## Skills

### `discover-project-goal`

**Pattern:** Interview + template

**Purpose:** Runs a structured discovery session to learn the project's main
goal, vision, constraints, and direction. The agent explores the repo first,
asks you targeted questions in rounds, then writes a detailed handoff document
another agent can read without prior chat history.

**When to use:**

- Starting a new project and need to capture direction
- Onboarding a new agent or collaborator
- Re-aligning after scope or priorities change

**How to invoke:**

```
Use discover-project-goal
```

Or:

```
Let's discover this project's goal
Create a project brief for agent handoff
```

**What happens:**

1. **Reconnaissance** — The agent reads the repo (README, structure, configs,
   existing docs) before asking questions it could answer itself.
2. **Interview** — Structured question rounds about purpose, audience, scope,
   priorities, constraints, and next steps.
3. **Write brief** — Produces `.cursor/PROJECT-BRIEF.md` from the interview
   answers.

**Output:** `.cursor/PROJECT-BRIEF.md`

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/discover-project-goal/SKILL.md` | Skill workflow |
| `.cursor/skills/discover-project-goal/template.md` | Brief structure template |
| `.cursor/PROJECT-BRIEF.md` | Generated handoff document |

**Example session:**

```
You:  Use discover-project-goal
Agent: [Explores repo, asks structured questions across rounds]
You:  [Answer questions]
Agent: [Writes .cursor/PROJECT-BRIEF.md and summarizes the project's direction]
```

### `maintain-project-docs`

**Pattern:** Conditional auto-read + auto-write workflow

**Purpose:** Keeps the agent aligned with project goals by reading
`.cursor/PROJECT-BRIEF.md` before non-trivial work, and keeps
`.cursor/PROJECT-BRIEF.md` / `README.md` accurate by writing back durable
facts — decisions, scope changes, new skills — as they emerge. One skill, two
directions: read before acting, write when you learn something.

**When it applies (automatic):**

Read side:
- Starting a non-trivial task (feature, refactor, multi-file change)
- Suggesting approaches, features, or architecture
- Scope or priorities are unclear
- A request may conflict with stated goals or non-goals

Write side:
- The user shares or confirms project direction, constraints, or decisions
- A skill is added, renamed, or materially changed
- Priorities or phase focus shift
- Docs drift from the repo (missing skills, wrong layout tree, outdated next steps)

**How to invoke:**

No explicit invocation needed — the agent applies this skill automatically when
triggers match. To force either pass:

```
Re-read the project brief before we continue
Sync the project docs
```

**What happens:**

1. **Read** — Agent reads `.cursor/PROJECT-BRIEF.md` when starting meaningful work; applies goals, constraints, priorities, and phase focus.
2. **Flag** — If your request conflicts with the brief, the agent names the conflict and asks how to proceed.
3. **Write** — When durable facts emerge, maps them to a file/section via `doc-map.md`, patches minimally, and bumps `Last updated`.
4. **Report** — One-line summary of what synced.

**If no brief exists:** The agent suggests running `discover-project-goal` first.

**Pair with:** `discover-project-goal` (creates the brief this skill reads and updates)

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/maintain-project-docs/SKILL.md` | Read + write workflow |
| `.cursor/skills/maintain-project-docs/doc-map.md` | Fact → file/section mapping |
| `.cursor/PROJECT-BRIEF.md` | Read before work; updated incrementally |
| `README.md` | Updated when skills or layout change |

**Example behavior:**

```
You:  Add a Flask API to this project
Agent: The brief lists application code as out of scope — this project is
       skills + docs only. Want to override, or should we stay focused on skills?

You:  Let's skip the git skill and build code review next.
Agent: [Updates brief priorities and next steps; updates README planned table]
       Updated brief (priorities, next steps) and README (planned skills).
```

### `grill-me`

**Pattern:** Interview loop (topic-scoped)

**Purpose:** Runs a structured interview loop on a plan or decision until every
open question is settled. Maps the topic as a design tree, asks the current
frontier of questions via AskQuestion each round, then recomputes and repeats
until nothing is left ambiguous. Summarizes decisions and syncs project docs
when durable facts emerge.

**When to use:**

- A plan or decision still has unanswered questions before building starts
- You want assumptions stress-tested on a specific topic (not full project discovery)
- You need a clear decision summary and updated brief/README after grilling

**How to invoke:**

```
Use grill-me
```

Or:

```
Grill me on the auth approach
Remove the question marks about our deployment plan
```

**What happens:**

1. **Scope** — Agent bounds the topic from conversation context and gathers
   lookup-able facts from the repo.
2. **Loop** — Each round: compute the frontier of askable questions →
   AskQuestion → wait for answers → repeat until the frontier is empty.
3. **Summary** — Decided and rejected/parked items in chat.
4. **Doc sync** — If durable project decisions emerged, updates the brief and
   README via `maintain-project-docs` write rules.

**Output:** Summary in chat; optional updates to `.cursor/PROJECT-BRIEF.md` and
`README.md`

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/grill-me/SKILL.md` | Interview loop workflow |
| `.cursor/PROJECT-BRIEF.md` | Updated when grilling settles project-wide decisions |
| `README.md` | Updated when grilling affects documented skills or layout |

**Example session:**

```
You:  Use grill-me on whether we add a fifth skill or submit with four
Agent: [Scopes topic, runs AskQuestion rounds until frontier is empty]
You:  [Answer questions across rounds]
Agent: [Summary: Decided / Rejected. Updated brief (key decision, next steps).]
```

**Pair with:** `maintain-project-docs` (doc updates after grilling), `discover-project-goal`
(full project discovery when the whole direction needs capture)

### `finish-ticket`

**Pattern:** Conditional auto-run checklist workflow

**Purpose:** Defines what "done" means for a ticket, task, or feature and runs
the completion sequence instead of just stopping after the last edit —
self-review, tests and coverage, integration check, PR preparation, and ticket
status update.

**When it applies (automatic):**

- The agent's own last planned step for a ticket/task/feature is complete
- The user says "done," "ship it," "wrap this up," or similar
- Right before opening a pull request

**How to invoke:**

No explicit invocation needed — the agent runs this checklist when it believes
work is complete. To force it:

```
Run the finish-ticket checklist
Are we actually done here?
```

**What happens:**

1. **Self-review** — Reads the full diff against the ticket's stated goal; flags scope creep and leftover debug code.
2. **Tests** — Runs the test suite; adds coverage for new behavior if the repo has a test convention.
3. **Coverage/edge cases** — Checks new branches and error paths are exercised, not just the happy path.
4. **Integration check** — Confirms build/lint passes and checks callers/contracts the change touches.
5. **Ticket linkage** — Confirms commit messages reference the ticket ID per repo convention.
6. **PR prep** — Drafts a PR description (what/why/how tested); does not open it without a go-ahead.
7. **Ticket status** — Updates ticket status if it has the means and the user confirmed, otherwise tells the user what to change.

**Output:** Short pass/skip/attention summary in chat; optional PR draft; optional ticket status update.

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/finish-ticket/SKILL.md` | Completion checklist workflow |

**Example behavior:**

```
Agent: [Finishes implementing the requested change]
Agent: Tests pass (12/12, 2 new). Lint clean. No coverage tool in this repo —
       flagging as a gap. Commit has a `Refs: TICKET-42` trailer. Ready for PR — want me
       to open it?
```

**Pair with:** `maintain-project-docs` (sync docs if completion reveals a durable decision)

### `write-jira-tickets`

**Pattern:** Template / checklist (ambient)

**Purpose:** Teaches how to write clear, testable Jira tickets (summary,
description, acceptance criteria) and link them to git commits and branches
via the Conventional Commits + trailer convention (`Refs: PROJ-123`). Uses the
Atlassian MCP tools to create or update real tickets when invoked.

**When it applies (automatic):**

- Drafting a Jira ticket, or writing a commit message/branch name that should
  reference one
- The user asks how to connect commits to tickets

**How to invoke:**

No explicit invocation needed — the agent applies this automatically when
drafting a ticket or commit. To force it:

```
Write a Jira ticket for this
How should I link this commit to the ticket?
```

**What happens:**

1. **Structure the ticket** — Summary, description, testable acceptance criteria, type/labels, links; checks the target project's actual conventions via Atlassian MCP metadata calls before inventing fields.
2. **Create/update** — Files or edits the real ticket via `createJiraIssue`/`editJiraIssue` after confirming project key and issue type.
3. **Link commits** — Recommends Conventional Commits subject + `Refs: PROJ-123` footer trailer, and a matching `PROJ-123-short-desc` branch name; defers to the repo's existing convention if one is already in use.

**Output:** Drafted or created ticket; recommended branch name and commit trailer convention.

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/write-jira-tickets/SKILL.md` | Ticket authoring + linkage workflow |

**Example behavior:**

```
You:  Write a Jira ticket for the login timeout bug and give me a branch name.
Agent: [Checks project issue-type metadata, drafts summary/description/AC,
       creates PROJ-456 via MCP]
       Created PROJ-456. Branch: PROJ-456-fix-login-timeout.
       Commit footer: Refs: PROJ-456.
```

**Pair with:** `finish-ticket` (verifies at completion time that commits actually followed this convention)

### `harmonize-project-skills`

**Pattern:** Checklist audit + report + optional fix workflow

**Purpose:** Audits all project skills for correct structure, quality, overlap, and
integration. Checks that README and the project brief stay aligned with the repo.
Reports issues by severity and fixes approved items so the skill stack is reliable
and low-friction.

**When to use:**

- After adding or changing skills
- Before submission or when skills feel inconsistent
- When unsure skills invoke correctly or work well together

**How to invoke:**

```
Use harmonize-project-skills
```

Or:

```
Audit our project skills
Streamline the skill stack
Check skill file structure
```

**What happens:**

1. **Inventory** — Lists every skill under `.cursor/skills/` and its files.
2. **Audit** — Runs the checklist (structure, content, cohesion, doc alignment).
3. **Report** — Presents findings as Critical / Warning / Suggestion.
4. **Fix** — Applies approved fixes to skills and docs; re-checks resolved items.

**Output:** Audit report in chat; optional file edits when fixes are approved.

**Files:**

| File | Role |
|------|------|
| `.cursor/skills/harmonize-project-skills/SKILL.md` | Audit workflow |
| `.cursor/skills/harmonize-project-skills/checklist.md` | Audit criteria |
| `.cursor/skills/harmonize-project-skills/report-template.md` | Report format |

**Example session:**

```
You:  Use harmonize-project-skills
Agent: [Inventories skills, runs checklist, reports 2 warnings]
       Want report only, or should I fix Critical and Warning items?
You:  Fix the warnings
Agent: [Patches skills and README, summarizes changes]
```

---

## Planned skills

Six skills are done — above the assignment minimum (3–5).

| Skill | Pattern | Status |
|-------|---------|--------|
| `discover-project-goal` | Interview + template | Done |
| `maintain-project-docs` | Conditional auto-read + auto-write workflow | Done |
| `grill-me` | Interview loop (topic-scoped) | Done |
| `finish-ticket` | Conditional auto-run checklist | Done |
| `harmonize-project-skills` | Checklist audit + fix workflow | Done |
| `write-jira-tickets` | Template / checklist (ambient) | Done |
| Mindset skill (e.g. confirmational → adversarial → concluding) | Reasoning workflow | Considered, not built |
| Session-summary-as-tree | Template | Considered, not built |

---

## For instructors

This submission demonstrates:

- **Project-scoped skills** stored under `.cursor/skills/`
- **Explicit invocation** via `disable-model-invocation: true` where appropriate
- **Automatic invocation** for ambient context skills (`maintain-project-docs`,
  `finish-ticket`)
- **Structured workflows** with phased steps (reconnaissance, interview, output)
- **Interview loop** for topic-scoped decision settling (`grill-me`)
- **Checklist audit workflow** with severity-ranked findings (`harmonize-project-skills`)
- **Completion/definition-of-done workflow** covering review, test, PR, and
  ticket-tracking hygiene (`finish-ticket`)
- **Progressive disclosure** via supporting files (`template.md`, `checklist.md`, `doc-map.md`)
- **Agent handoff** via a generated brief (`.cursor/PROJECT-BRIEF.md`)

To evaluate the skill set:

1. **`discover-project-goal`** — Invoke in Agent chat, answer the interview,
   inspect `.cursor/PROJECT-BRIEF.md`.
2. **`maintain-project-docs`** — Ask the agent to do something that conflicts with
   the brief (e.g. "add application code") and confirm it flags the conflict; then
   add or change a skill and confirm the agent updates README and the brief
   without a full rediscovery interview.
3. **`grill-me`** — Invoke on a plan or decision with open questions; answer
   rounds until the agent summarizes and optionally syncs docs.
4. **`finish-ticket`** — Finish a piece of work and confirm the agent runs the
   completion checklist (review, tests, PR prep, ticket status) before calling
   it done.
5. **`harmonize-project-skills`** — Invoke to audit the skill stack; review the
   report and optional fixes.
6. **`write-jira-tickets`** — Ask the agent to write a ticket for some work; confirm
   it drafts summary/AC, creates it via Atlassian MCP, and gives a branch name and
   commit trailer convention.

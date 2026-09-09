# Project Brief

> Agent handoff document. Read this before starting work on this project.
> Produced by the discover-project-goal skill.
> Last updated: 2026-09-09 (added write-jira-tickets, harmonized docs)

## TL;DR

This is a graded homework project for learning Cursor Agent Skills. The goal is
to build 3–5 project-scoped skills with varied patterns, plus a README that
documents each skill. Six skills exist (`discover-project-goal`,
`maintain-project-docs`, `grill-me`, `finish-ticket`,
`harmonize-project-skills`, `write-jira-tickets`). Ready for final submission
review. No application code is in scope.

## Problem and audience

- **Problem:** The project needs to demonstrate practical understanding of how
  Cursor Agent Skills work — their structure, triggers, workflows, and patterns.
- **Audience:** A course instructor evaluating a graded submission. The README
  must be clear enough that the instructor can understand and invoke each skill.
- **Why now:** Assignment is due within a few days; five skills are built and
  documented; final polish complete.

## Vision and success criteria

- **Vision:** A polished repo containing 3–5 well-crafted, distinct skills and
  documentation that shows mastery of the Cursor skills system.
- **Success looks like:**
  - 3–5 skills live under `.cursor/skills/`, each serving a different purpose
  - Skills demonstrate **variety in patterns** — e.g. interview-style,
    template-based, workflow/checklist, documentation generation, git workflow
  - Each skill has clear YAML frontmatter, a specific description with trigger
    terms, and actionable step-by-step instructions
  - A README explains every skill and how to invoke it
- **Not success:** Building a full application, overlapping skills that do the
  same thing, or skills so vague they cannot be invoked reliably.

## Current phase

- **Phase name:** Pre-submission — six skills in place, rebalanced for
  category coverage against the assignment's example list
- **Focus now:** Final instructor review; optional git init and commit if desired.
- **Explicitly deferred:** Application code, deployment, tests, CI/CD — none of
  these are in scope for this assignment.

## Priorities

| Priority | Item | Rationale |
|----------|------|-----------|
| P0 | 3–5 distinct project skills | Core deliverable — 6 of 3–5 done |
| P0 | README documenting all skills | Done; keep current via `maintain-project-docs` |
| P1 | Pattern + category variety across skills | Primary grading criterion; assignment lists distinct workflow categories |
| P1 | `discover-project-goal` skill polish | Done; harmonize pass confirmed submission-ready |
| P2 | Optional utility scripts in skills | Only if they add real value to a skill |

## Constraints

- **Tech stack:** Markdown skill files (`.cursor/skills/<name>/SKILL.md`).
  Standard Python `.gitignore` is present but no Python code is planned.
- **Environment:** Local Cursor IDE; project-scoped skills only
  (`.cursor/skills/`, not `~/.cursor/skills/`).
- **Non-negotiables:**
  - README must explain each skill and how to invoke it
  - Skills must live in this repo under `.cursor/skills/`
  - No secrets or credentials in skill files
  - No full application — skills + docs only
- **Resources:** Solo student project; deadline within a few days.

## Quality bar

- Each skill should follow Cursor skill conventions: YAML frontmatter with
  `name` and `description`, concise body, progressive disclosure via templates
  or reference files when needed.
- Skills intended for explicit invocation should set
  `disable-model-invocation: true`.
- Descriptions must be third-person and include both WHAT and WHEN.
- Prefer focused, single-purpose skills over one mega-skill.
- README should be scannable: skill name, one-line purpose, invocation example
  for each.

## Architecture snapshot

Skills-only repo. Current layout:

- `.cursor/skills/discover-project-goal/SKILL.md` — interview-based project
  goal discovery; writes `.cursor/PROJECT-BRIEF.md`
- `.cursor/skills/discover-project-goal/template.md` — output template for the
  brief
- `.cursor/skills/maintain-project-docs/SKILL.md` — merged read+write doc
  skill: auto-reads brief before non-trivial work, auto-writes brief/README
  when durable facts emerge; `doc-map.md` maps facts to sections
  (merged from former `align-with-brief` + `sync-project-docs`)
- `.cursor/skills/grill-me/SKILL.md` — user-invoked topic-scoped interview loop;
  settles open questions via AskQuestion rounds; may trigger doc sync
- `.cursor/skills/finish-ticket/SKILL.md` — auto-run completion checklist
  (review, tests, coverage, integration, PR prep, ticket status) once the
  agent believes a ticket/task is done
- `.cursor/skills/harmonize-project-skills/SKILL.md` — user-invoked audit of skill
   structure, cohesion, and doc alignment; `checklist.md`, `report-template.md`
- `.cursor/skills/write-jira-tickets/SKILL.md` — ambient skill for drafting
  testable Jira tickets and creating/updating them via Atlassian MCP; teaches
  Conventional Commits + trailer (`Refs: PROJ-123`) for commit/branch linkage;
  upstream of `finish-ticket`'s linkage check
- `.cursor/PROJECT-BRIEF.md` — this file
- `README.md` — instructor-facing skill catalog and invocation guide
- `.gitignore` — standard Python template (no application code expected)

No application source. Git may or may not be initialized.

## Conventions

- Skill names: lowercase, hyphens, max 64 characters.
- Project-scoped skills only — store under `.cursor/skills/<skill-name>/`.
- Use `AskQuestion` for structured interview-style skills when available.
- Use `disable-model-invocation: true` for skills the user calls explicitly.
- Supporting files (templates, examples) live alongside `SKILL.md` in the skill
  directory.

## Key decisions

| Date | Decision | Rationale | Status |
|------|----------|-----------|--------|
| 2026-09-08 | Build 3–5 skills with varied patterns | Assignment success criterion is variety | Active |
| 2026-09-08 | No application code | Explicit non-goal; skills + docs only | Active |
| 2026-09-08 | README is P0 after first skill | Required by assignment constraints | Active |
| 2026-09-08 | First skill: `discover-project-goal` | Interview + template pattern; already built | Active |
| 2026-09-08 | `sync-project-docs` for incremental doc maintenance | Keeps brief and README aligned without full rediscovery | Active |
| 2026-09-08 | `harmonize-project-skills` for skill stack audit | Reduces overlap, doc drift, and invocation friction before submission | Active |
| 2026-09-08 | `grill-me` for topic-scoped decision interviews | Settles open questions on a plan before building; syncs docs when decisions are durable | Active |
| 2026-09-09 | Merged `align-with-brief` + `sync-project-docs` into `maintain-project-docs` | Assignment weights category breadth; reduced doc-cluster from 3 skills to 1 to make room | Active |
| 2026-09-09 | Added `finish-ticket` (review/test/coverage/PR/ticket-status checklist) | Directly named in assignment as a workflow category; was the most central gap | Active |
| 2026-09-09 | Added `write-jira-tickets` (ticket authoring + commit/branch linkage, ambient, uses Atlassian MCP) | Deeper coverage of ticket writing / git-commit linkage than `finish-ticket` alone provided; grilled via `grill-me` | Active |

## Risks and open questions

- **Risks:**
  - Tight timeline (due within a few days) — five skills meet the 3–5 requirement
  - Skill ideas may overlap — need distinct purposes and patterns
- **Open questions:**
  - Exact assignment rubric or required skill topics (not stated)
  - Which specific patterns the instructor weights most heavily

### Skill roadmap (candidate ideas)

Remaining assignment categories not yet built, lowest priority since 6 of 5
minimum are done:

1. **Mindset skill** (e.g. `dual_pass`: confirmational → adversarial →
   concluding) — pure reasoning-pattern skill, no file I/O
2. **Session-summary-as-tree** — structured chat-session summary for future
   agents reading the transcript
3. Infra usage, parallel-agent communication, local knowledge-base — flagged
   by the assignment itself as advanced/bonus ("överkurs")

`discover-project-goal` covers interview + template; `grill-me` covers
interview loop; `finish-ticket` covers completion/checklist;
`maintain-project-docs` covers conditional read+write; `harmonize-project-skills`
covers audit; `write-jira-tickets` covers template/checklist for ticket
authoring and git linkage. New skills should keep covering distinct categories
rather than adding more doc-lifecycle variants.

## Recommended next steps

1. **Final submission pass** — confirm all skills are invocation-ready, descriptions
   are discovery-friendly, and README is complete for instructor review.
2. **Optional git init + commit** — if submitting via git, initialize and push when ready.

## Context sources

- Repo paths read: `.gitignore`, `README.md`, all `.cursor/skills/*/SKILL.md`
  and supporting files (`template.md`, `doc-map.md`, `checklist.md`,
  `report-template.md`)
- User interview date: 2026-09-08
- Last harmonize audit: 2026-09-09 (post `write-jira-tickets` addition — 1 Critical doc-drift item fixed, 0 remaining)
- Related docs: `README.md` (skill catalog and invocation guide); no
  `.cursor/PROJECT.md`

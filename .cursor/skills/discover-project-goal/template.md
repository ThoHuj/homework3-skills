# Project Brief

> Agent handoff document. Read this before starting work on this project.
> Produced by the discover-project-goal skill.
> Last updated: YYYY-MM-DD

## TL;DR

Two to three sentences: what this project is, who it is for, and where it is
heading right now. A cold-start agent should know the main goal after reading
only this section.

## Problem and audience

- **Problem:** What pain or need does this project address?
- **Audience:** Who uses or benefits from it?
- **Why now:** Why is this project worth doing?

## Vision and success criteria

- **Vision:** Where should this project be in 3–6 months?
- **Success looks like:**
  - [Measurable or observable outcome 1]
  - [Measurable or observable outcome 2]
- **Not success:** Common misreadings or anti-goals

## Current phase

- **Phase name:** e.g. prototype, MVP, hardening, migration
- **Focus now:** What matters in the current sprint or milestone
- **Explicitly deferred:** What is intentionally out of scope for now

## Priorities

| Priority | Item | Rationale |
|----------|------|-----------|
| P0 | | Must-have |
| P1 | | Important, not blocking |
| P2 | | Nice-to-have |

## Constraints

- **Tech stack:** Languages, frameworks, versions
- **Environment:** Deployment targets, hosting, CI/CD
- **Non-negotiables:** Things the project must always do or never do
- **Resources:** Timeline, team size, budget, or tooling limits (if stated)

## Quality bar

- Performance, reliability, security, accessibility, or UX expectations
- Testing or review expectations
- Acceptable tradeoffs (speed vs polish, etc.)

## Architecture snapshot

Brief description of how the system is organized today. Link key paths:

- `path/to/module` — responsibility
- External services, APIs, databases

Skip this section if the repo is empty or too early to describe.

## Conventions

Naming, patterns, and preferences the user stated. Only include what affects
how an agent should write code or make decisions.

## Key decisions

| Date | Decision | Rationale | Status |
|------|----------|-----------|--------|
| YYYY-MM-DD | | | Active / Superseded |

## Risks and open questions

- **Risks:** Known dependencies, blockers, or technical debt
- **Open questions:** Unresolved items the next agent should not assume

## Recommended next steps

Numbered list of concrete actions for the next agent, in priority order:

1. [First action — specific file, feature, or investigation]
2. [Second action]
3. [Third action]

## Context sources

Where this brief came from. Helps the next agent verify or dig deeper.

- Repo paths read: e.g. `README.md`, `src/`, `package.json`
- User interview date: YYYY-MM-DD
- Related docs: e.g. `.cursor/PROJECT.md`, design specs, tickets

---
name: finish-ticket
description: >-
  Defines what "done" means for a ticket, task, or feature and runs the
  completion sequence — self-review, tests and coverage, integration check, PR
  preparation, and ticket status update. Use when the agent believes
  implementation work is complete, before opening a pull request, or when the
  user says they are done, asks to wrap up, close out, or ship a ticket/task.
---

# Finish Ticket

Completion is a checklist, not a feeling. Before declaring a ticket, task, or
feature "done," run this sequence instead of just stopping after the last edit.

## When to run

- The agent's own last planned step for a ticket/task/feature is complete
- The user says "I think that's it," "done," "ship it," "wrap this up," or
  similar
- Right before opening a pull request
- Do **not** run for trivial edits (typo, one-line config change) unrelated to
  a tracked ticket — use judgment

## The completion sequence

Skip steps that don't apply to the repo (e.g. no test suite exists) and say so
explicitly rather than silently skipping.

### 1. Self-review the diff

- Read the full diff, not just the files you remember touching
- Check it matches the ticket's stated goal — no scope creep, nothing missed
- Look for leftover debug code, commented-out blocks, TODOs you meant to
  resolve, and stray print/log statements

### 2. Tests

- Run the existing test suite; fix or explain any failures
- Add tests for new behavior if the repo has a test convention — match its
  existing style and location
- If no test infrastructure exists, say so and ask whether adding minimal
  coverage is in scope

### 3. Coverage and edge cases

- Check that new branches/error paths are exercised, not just the happy path
- If a coverage tool is configured, run it and flag any drop for the changed
  files
- Note untested risky paths explicitly rather than presenting silently as safe

### 4. Integration check

- Confirm the change builds/lints cleanly in the project's normal toolchain
- If the change touches an interface other code depends on, check or note
  callers that might break
- For multi-service or multi-package repos, confirm the change doesn't break
  the boundary contract (API shape, schema, config keys)

### 5. Ticket linkage

- Ensure commit messages reference the ticket ID (e.g. a `Refs: PROJ-123`
  trailer footer per `write-jira-tickets`) per the repo's convention — check
  recent commit history if unsure of the format
- If no ticket ID convention exists, ask the user how tickets are tracked
  before inventing one

### 6. Pull request

- Prepare a PR description: what changed, why, how it was tested, and the
  ticket link
- Do not open the PR without the user's go-ahead unless they've asked you to
  do so proactively for this task

### 7. Ticket status update

- Update the ticket/issue status (e.g. move to "In Review" or "Done") only if
  you have the means to do so (CLI, MCP, API) and the user has confirmed that's
  expected
- If you cannot update it directly, tell the user exactly what to change and
  where

## Reporting back

Summarize the sequence result in a short list — what passed, what was skipped
and why, what still needs the user's attention. Do not claim "done" if tests
fail or a required step was skipped without explanation.

## Relationship to other skills

| Skill | Role |
|-------|------|
| `finish-ticket` | Completion sequence before calling work done (this skill) |
| `maintain-project-docs` | If completion reveals a durable decision or scope change, sync brief/README via its write rules |
| `harmonize-project-skills` | Skill-stack audit; not a substitute for per-ticket completion checks |
| `grill-me` | Use before building if the ticket's goal was ambiguous; this skill runs after building |
| `write-jira-tickets` | Draft-time ticket authoring and commit/branch linkage convention; this skill checks that convention was followed |

## Example

**Agent:** Just finished implementing the requested change.
**Action:** Runs self-review of the diff, runs the test suite (2 new tests
added), confirms lint passes, notes no coverage tool is configured, checks the
commit message includes a `Refs: TICKET-42` trailer, drafts a PR description, and reports:
"Tests pass (12/12, 2 new). Lint clean. No coverage tool in this repo — flagging
as a gap. Ready for PR — want me to open it?"

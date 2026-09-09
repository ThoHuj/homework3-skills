---
name: write-jira-tickets
description: >-
  Teaches how to write clear, testable Jira tickets and link them to git
  commits and branches so work stays traceable. Covers ticket structure
  (summary, description, acceptance criteria), the Atlassian MCP tools for
  creating/updating real tickets, and the Conventional Commits + trailer
  convention for linking commits to a ticket key. Use when the user is
  drafting a Jira ticket, writing a commit message or branch name that should
  reference a ticket, or asks how to connect commits to tickets.
---

# Write Jira Tickets

Good tickets are testable specs, not vague notes. Good commits point back to
the ticket that justified them. This skill covers both, upstream of
`finish-ticket`'s completion-time linkage check.

## Writing the ticket

Structure every ticket with these fields — skip only what the tracker doesn't
support:

- **Summary**: imperative, specific, under ~80 chars (`Fix null pointer on empty cart checkout`, not `Checkout bug`)
- **Description**: problem/context in 2-4 sentences — what's wrong or needed, and why it matters now
- **Acceptance criteria**: a bullet list of testable conditions, each phrased so "done" is unambiguous:
  ```
  - Checkout with an empty cart shows a validation message, not a 500
  - Existing checkout flow with items in cart is unaffected
  - Covered by a new test case
  ```
- **Type/labels**: Bug / Story / Task as the tracker defines them; add component or team labels if the project uses them
- **Links**: parent epic, blocking/blocked-by tickets, related PRs (added once they exist)

Before writing, check the target project's existing tickets for its actual
conventions (issue type names, required fields, label taxonomy) rather than
inventing your own — use `getJiraProjectIssueTypesMetadata` or
`getJiraIssueTypeMetaWithFields` (Atlassian MCP) to confirm required fields,
and `searchJiraIssuesUsingJql` to see a few recent tickets in the same project.

### Creating or updating the real ticket

Use the Atlassian MCP tools directly — don't just describe the ticket in chat
when the user wants it tracked:

- `createJiraIssue` to file it, after confirming project key and issue type
- `editJiraIssue` to update fields on an existing ticket
- `lookupJiraAccountId` before setting an assignee by name
- `getJiraIssue` to fetch current state before editing

Confirm the project key and issue type with the user if not already clear from
context — don't guess which Jira project a ticket belongs in.

## Linking commits to the ticket

Once a ticket key exists (e.g. `PROJ-123`), reference it via a **Conventional
Commits subject + a trailer footer**:

```
fix(checkout): handle empty cart on submit

Validate cart contents before the checkout API call instead of
relying on the backend to reject it.

Refs: PROJ-123
```

- Subject line: `<type>(<scope>): <imperative summary>` — no ticket key in the subject
- Footer trailer: `Refs: PROJ-123` (use `Closes: PROJ-123` only if the commit fully resolves the ticket)
- One trailer per referenced ticket; multiple tickets get multiple `Refs:` lines
- Branch names: `PROJ-123-short-description` — keeps the key visible in `git branch` and PR titles without touching commit subjects

If the repo's git history already uses a different convention (e.g. ticket key
as a subject prefix), match the existing convention instead of introducing a
second one — check `git log --oneline -20` first.

## Relationship to other skills

| Skill | Role |
|-------|------|
| `write-jira-tickets` | Draft-time ticket authoring and commit/branch linkage convention (this skill) |
| `finish-ticket` | Completion-time check that commits actually reference the ticket per the repo's chosen convention |
| `grill-me` | Use first if the ticket's own scope or acceptance criteria are still ambiguous |

## Example

**User:** "Write a Jira ticket for the login timeout bug and give me a branch name."

**Agent:** Checks the project's issue type metadata, drafts summary/description/
acceptance criteria, creates the ticket via `createJiraIssue`, returns
`PROJ-456`, and suggests branch `PROJ-456-fix-login-timeout` plus the commit
footer convention (`Refs: PROJ-456`) for the fix commit.

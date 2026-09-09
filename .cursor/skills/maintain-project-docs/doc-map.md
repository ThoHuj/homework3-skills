# Doc map — where facts belong

Quick reference for `maintain-project-docs`. Edit the listed section(s) when the
fact type appears.

| Fact type | PROJECT-BRIEF.md | README.md |
|-----------|------------------|-----------|
| Primary goal / audience | TL;DR, Problem and audience | Intro paragraph (brief) |
| Vision / success criteria | Vision and success criteria | For instructors (if demo-relevant) |
| Current phase / focus | Current phase | — |
| Priority change | Priorities table | Planned skills table (if skill-related) |
| Constraint / non-goal | Constraints | Intro or skill notes if invocation-relevant |
| Quality bar | Quality bar | — |
| New or changed skill | Architecture snapshot, Key decisions, Recommended next steps | Skills section, Project layout, Planned skills table |
| Skill renamed / removed | Architecture snapshot | Remove or rename skill section; fix layout |
| Repo layout change | Architecture snapshot | Project layout tree |
| Decision with rationale | Key decisions | Only if it affects documented behavior |
| Completed next step | Recommended next steps | Planned skills → Done |
| New open question | Risks and open questions | — |
| Resolved question | Remove from open questions | — |
| Risk identified or cleared | Risks and open questions | — |
| How to invoke a skill | — | Skill subsection |
| Pattern type (interview, checklist, etc.) | Conventions (if project-wide) | Skill subsection + Planned table |
| Interview / discovery date | Context sources | — |

## Staleness checks

When syncing for any reason, quickly verify:

- [ ] Brief `Architecture snapshot` lists every directory under `.cursor/skills/`
- [ ] Brief `Recommended next steps` reflects what actually should happen next
- [ ] README layout tree matches the repo
- [ ] README documents every skill in `.cursor/skills/` (except meta WIP the user asked to hide)
- [ ] README Planned skills table matches brief roadmap / priorities
- [ ] No skill is documented in README but missing from layout tree (or vice versa)

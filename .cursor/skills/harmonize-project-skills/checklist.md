# Skill harmonization checklist

Use during Phase 2 of `harmonize-project-skills`. Mark each item pass/fail; note
failures in the audit report.

## Per-skill structure

- [ ] Directory lives under `.cursor/skills/<name>/`
- [ ] `SKILL.md` exists and is the main instruction file
- [ ] Directory name matches frontmatter `name` exactly
- [ ] `name` is lowercase, hyphenated, ≤64 characters, no spaces or underscores
- [ ] YAML frontmatter is valid (`---` delimiters, required fields present)
- [ ] `description` is non-empty, ≤1024 characters, third person
- [ ] `description` states WHAT the skill does and WHEN to use it (trigger terms)
- [ ] `SKILL.md` body is ≤500 lines (split to reference files if over)
- [ ] Supporting files (templates, checklists) live in the same skill directory
- [ ] Relative links from `SKILL.md` work and are at most one level deep
- [ ] No Windows-style paths (`\`)
- [ ] No secrets, tokens, or credentials

## Per-skill content

- [ ] Title (H1) matches skill purpose; not generic ("Helper", "Utils")
- [ ] Clear phased or step-by-step workflow
- [ ] Invocation behavior is explicit (user-only vs automatic)
- [ ] `disable-model-invocation: true` set iff skill is user-invoked only
- [ ] Examples or concrete behavior described (not abstract only)
- [ ] Terminology consistent within the skill
- [ ] "Relationship to other skills" (or equivalent) when other project skills exist
- [ ] Boundaries clear — what this skill does **not** do

## Stack-wide cohesion

- [ ] Each skill has a distinct primary purpose (no duplicate jobs)
- [ ] Trigger terms in descriptions do not cause the wrong skill to win discovery
- [ ] Handoff chain documented: brief creation → read brief → sync docs
- [ ] Cross-references between skills use correct skill names and paths
- [ ] Mix of patterns is intentional (not three copies of the same workflow)
- [ ] Assignment constraints respected (e.g. project-scoped only, no app code)

## Documentation alignment

- [ ] Every skill in `.cursor/skills/` has a README subsection (unless user asked to hide WIP)
- [ ] README project layout tree matches actual directories and files
- [ ] README planned-skills table reflects brief roadmap / current status
- [ ] Brief architecture snapshot lists every skill directory
- [ ] Brief skill count and "next steps" match repo reality
- [ ] Invocation examples in README match skill `disable-model-invocation` behavior

## Severity guide

| Severity | Examples |
|----------|----------|
| **Critical** | Missing `SKILL.md`, name/dir mismatch, broken skill discovery, undocumented skill in repo, contradictory invocation flags, secrets in files |
| **Warning** | Weak description (no WHEN), overlap with another skill, missing relationship section, doc drift, >500 lines in SKILL.md |
| **Suggestion** | Optional polish: tighter wording, extra example, split long section to reference file |

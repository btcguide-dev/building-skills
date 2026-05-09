# Universal SKILL.md Template

Copy this template and fill in each section. Delete the instructional comments (lines starting with `#>`) before finalising.

```yaml
---
name: your-skill-name
#> Lowercase, hyphens, max 64 chars. Gerund form preferred: analyzing-data, preparing-meetings
description: >
  Use when the user says "[phrase 1]", "[phrase 2]", "[phrase 3]",
  or asks about [topic]. Also triggers on "[alternative phrase]"
  or "[related request]". [One sentence describing what the skill does.]
  NOT for [what should NOT trigger this skill].
#> THE TRIGGER — most important part. Be LOUD. Lead with trigger phrases.
#> Write in third person. Include edge-case triggers people actually say.
#> Test: say the trigger phrase — does it fire? Say 3 similar phrases — does it fire when it shouldn't?
#> Keep under 250 characters for the description (longer gets truncated in listings).
allowed-tools: Read,Write,Edit
#> Only list tools this skill actually needs. Bash access uses patterns: Bash(python*,ffmpeg*)
#> Optional fields:
#> argument-hint: [slug] or [issue-number] — shown during autocomplete
#> disable-model-invocation: true — prevents auto-triggering, manual /name only
#> user-invocable: false — hides from / menu, background knowledge only
#> context: fork — runs in isolated subagent context
#> agent: Explore — which subagent type to use with context: fork
#> paths: "src/**/*.ts" — only auto-activates when working with matching files
---

# [Skill Name]

#> One paragraph: what this skill does and why it exists.

## Context Required

#> List every file the agent should read before running. Full paths.
#> Don't assume the agent remembers anything from prior sessions.
#> Point to external files for shared context, bundle skill-specific context in the folder.

Read these files before running:
- [file path] — [what it contains and why it's needed]
- [file path] — [what it contains and why it's needed]

## Steps

#> Use numbered steps, not prose. Set degrees of freedom:
#> - Tight steps for fragile operations (deployments, migrations, data writes)
#> - Loose guidance for creative tasks (writing, brainstorming, strategy)
#> Each step should be specific and actionable — not "analyze the situation"

1. [Specific, actionable step with clear inputs and outputs]
2. [Next step]
3. [Continue until the deliverable is complete]

## Output

#> Show the EXACT format. Don't describe — show.
#> If it's a table, show the headers. If it's a document, show the sections.
#> If the template is long, put it in a separate output-template.md and reference it.

[Literal template with headers, structure, and length constraints]

## Gotcha Section

#> HIGHEST-SIGNAL CONTENT. Document every failure pattern you've seen.
#> Format: "I know you'll want to do X — don't. Here's why."
#> Start building from day 1. Add to it every time the skill produces bad results.
#> This is what separates a mediocre skill from an excellent one.

- [Failure pattern 1 — what goes wrong and why]
- [Failure pattern 2]
- [Failure pattern 3]

## Constraints

#> Rules specific to THIS skill. Sharp and specific.
#> Not general behaviour instructions — those belong in CLAUDE.md.

- [Constraint 1]
- [Constraint 2]
```

## Folder Structure Template

```
my-skill/
├── SKILL.md              # Core instructions (required, under 500 lines)
├── references/            # Detailed reference docs (loaded on demand)
│   └── guide.md
├── examples/              # Input/output examples showing expected format
│   └── examples.md
├── output-template.md     # Long output templates (referenced from SKILL.md)
└── scripts/               # Executable scripts (run, not loaded into context)
    └── helper.py
```

## Naming Conventions

- **Skill name:** lowercase, hyphens, max 64 chars
- **Gerund form preferred:** `analyzing-data`, `preparing-meetings`, `building-skills`
- **Directory name matches skill name**

## Placement Guide

| Location | Path | When to Use |
|----------|------|------------|
| Personal (global) | `~/.claude/skills/<name>/` | Applies to all your projects |
| Project | `.claude/skills/<name>/` | This project only |
| Enterprise | Managed settings | All users in your org |

## Pre-Finalisation Checklist

Before considering a skill complete, verify:

- [ ] Description triggers correctly (positive test)
- [ ] Description does NOT trigger for similar but different requests (negative test)
- [ ] Steps are numbered/bulleted, not prose
- [ ] Output format is shown, not described
- [ ] Gotcha section exists (even if sparse initially)
- [ ] SKILL.md is under 500 lines
- [ ] Large references moved to separate files
- [ ] Reviewed against the 5 Skill Killers

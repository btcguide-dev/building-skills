# building-skills

A Claude Code skill that serves as the definitive playbook for building, reviewing, updating, and auditing skills. Replaces the default `skill-creator` with a more opinionated, trigger-rich version distilled from real-world skill-building practice.

Triggers on phrases like "build a skill", "create a skill", "review this skill", "improve this skill", "update a skill", "skill audit", "convert to skill", "skill best practices", and more.

## Install

In Claude Code:

```
/plugin marketplace add btcguide-dev/building-skills
/plugin install building-skills@building-skills
```

## Recommended: disable the default skill-creator

To make sure this skill wins skill-creation triggers cleanly, uninstall the bundled Anthropic `skill-creator`:

- **Claude Desktop:** Customize → Skills → find `skill-creator` → Uninstall
- **Claude Code:** `/plugin uninstall skill-creator` (if installed as a plugin)

The two skills have different names so they coexist, but disabling the default removes ambiguity and ensures this one is invoked.

## What's inside

- `skills/building-skills/SKILL.md` — the main playbook
- `skills/building-skills/references/universal-template.md` — copy-and-fill template
- `skills/building-skills/references/advanced-patterns.md` — patterns for layered skills, references, anti-patterns
- `skills/building-skills/references/example-skills.md` — worked examples
- `skills/building-skills/references/skill-killers.md` — the 5 things that kill a skill

## Manual install (no marketplace)

```bash
git clone https://github.com/btcguide-dev/building-skills ~/.claude/skills/_repo
ln -s ~/.claude/skills/_repo/skills/building-skills ~/.claude/skills/building-skills
```

## License

MIT — see [LICENSE](LICENSE).

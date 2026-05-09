---
name: building-skills
description: >
  Use when creating, reviewing, updating, modifying, or auditing a skill —
  including improving a skill's trigger or gotcha section, or converting commands
  to skills. Triggers on "build a skill", "create a skill", "new skill",
  "skill template", "review this skill", "improve this skill", "update a skill",
  "modify a skill", "edit a skill", "skill audit", "convert to skill",
  "skill best practices", "make a skill for", "write a skill". NOT for using or
  invoking existing skills. NOT for general prompting or instruction writing.
allowed-tools: Read,Write,Edit,Glob,Grep
---

# Building Skills

The definitive playbook for building effective agent skills. Skills are portable, human-readable folders that give AI agents actionable playbooks. This skill ensures every skill you build follows proven patterns and avoids common pitfalls.

For the copy-and-fill template, see [references/universal-template.md](references/universal-template.md).

## When to Build a Skill

Build a skill when ANY of these are true:

1. **You've done it 3+ times** with an AI tool
2. **You keep pasting the same instructions** across sessions
3. **You need consistent, reliable output** every time
4. **You want to standardise** how you or others do this work — the skill becomes the single source of truth
5. **You want to unlock something new** — things you always wanted to do but didn't have the bandwidth or ability

Do NOT build a skill when:
- It's a one-off task you won't repeat
- The task is trivial and doesn't benefit from structure
- An existing skill already handles it (check first)

## Scoping: One Skill or Two?

One clear job per skill. Use these tests:

- Can you describe it in **one sentence**? → One skill
- Does it have **two distinct triggers**? → Probably two skills
- Would two different people **use different halves**? → Two skills
- Is the SKILL.md approaching **500 lines**? → Split it

If you find yourself cramming separate jobs into one file, separate them.

## Skill Types: Where to Invest

| Type | What It Does | Durability |
|------|-------------|-----------|
| **Capability Uplift** | Enables new functions the model can't do well on its own | May become obsolete as models improve |
| **Encoded Preference** | Sequences existing capabilities according to YOUR workflow | Gets more valuable over time |

Spend most of your time on **encoded preference skills**. They capture how YOU work and compound in value. Capability skills fill gaps but may be superseded by model improvements.

## The Anatomy of an Effective Skill

Every skill should have these components. The order matters — this is how the model reads and acts on them.

### 1. Trigger / Description (THE MOST CRITICAL LINE)

The description is a **trigger, not a summary**. It tells the model "when should I fire this skill?" If the trigger is vague or quiet, the skill will never be selected.

Rules:
- **Make it LOUD rather than quiet** — models skip past subdued descriptions
- **Lead with trigger phrases**, not with what the skill does
- **Write in third person** — descriptions get injected into the system prompt, so "I can help you" breaks things
- **Use "Use when..." format** — explicit activation phrases
- **Include edge-case triggers** people actually say ("before I send this", "is this solid")
- **Add NOT clauses** — what should NOT trigger this skill (prevents collisions with similar skills)
- **Keep under 250 characters** — longer descriptions get truncated in skill listings

Test your trigger: say the phrase — does it fire? Say 3 similar phrases — does it fire when it shouldn't?

### 2. Context Required

List every file the agent should read before running. Use full paths. Don't assume the agent remembers anything from prior sessions.

- Point to external files for shared context (CLAUDE.md, project docs, stakeholder info)
- Bundle skill-specific context inside the skill folder (templates, rubrics, examples)
- Rule of thumb: "about the skill" = inside the folder. "About you/org" = point externally.

### 3. Steps (The Playbook)

**Use numbered steps or bulleted lists, not prose.** The models like structured instructions dramatically because structured instructions become the action plan.

Set the right level of freedom:
- **Tight steps** for fragile operations (database migrations, deployments, anything that must be precise) — be very prescriptive
- **Loose guidance** for creative tasks (writing strategy docs, brainstorming, anything open to interpretation) — give guidance but leave room for creativity

If you over-railroad the model, you will not get as good results on creative work.

### 4. Output Format

**Show, don't describe.** Include a literal template or example.

- If you want a table, show the table with headers
- If you want a document, show the section structure
- If the output is long, put the template in a separate file (e.g., `output-template.md`) and reference it

This is the most effective way to communicate the desired format.

### 5. Gotcha Section (HIGHEST-SIGNAL CONTENT)

This is probably the most valuable part of any skill. It captures where the model typically goes wrong — the assumptions it makes that it shouldn't, the patterns it falls into that produce bad results.

Format each gotcha as: **"I know you'll want to do X — don't. Here's why."**

- Start building this section from day 1, even if it's empty
- Add to it every time the skill produces a bad result
- Every failure you've seen should be documented here
- This is what separates a mediocre skill from an excellent one

Examples of gotchas:
- "Don't assume attendee seniority from title alone"
- "Don't fabricate company details — flag unknowns explicitly"
- "Don't prepare generic talking points — every point must reference a specific item"
- "If you can't find data, say so — don't fill the gap with assumptions"

### 6. Constraints

Rules specific to THIS skill. Sharp and specific. Not general behaviour instructions.

### What NOT to Include

- **No persona / identity section** ("Act as a senior analyst...") — this is legacy prompt engineering. Tell the model what YOUR approach does differently, not what persona to adopt. The tools are looking for playbooks.
- **Don't state the obvious** — don't waste tokens on things the model already knows. Challenge every paragraph: "Does the model really need this?"
- **Don't repeat general instructions** from CLAUDE.md or system prompts

## The 5 Skill Killers

Quick-check every skill against these. For detailed before/after examples, see [references/skill-killers.md](references/skill-killers.md).

| # | Killer | Fix |
|---|--------|-----|
| 1 | **Vague/quiet description** — too generic, too narrow, or wrong person | Loud, specific, third-person. "Use when..." format with exact trigger phrases. |
| 2 | **Over-defining the process** — railroading instead of guiding | Match freedom to task fragility. Tight for fragile ops, loose for creative. |
| 3 | **Stating the obvious** — wasting tokens on what the model knows | Challenge every paragraph: "Does Claude really need this?" |
| 4 | **Missing gotcha section** — not capturing failure patterns | Document every failure you've seen. This IS the skill's value. |
| 5 | **Monolithic blob** — everything crammed into one file | SKILL.md under 500 lines. Move references to separate files. |

## Three-Stage Loading

Skills load progressively to manage context efficiently:

| Layer | What Loads | Token Cost | When |
|-------|-----------|-----------|------|
| 1. Description | ~100 tokens in system prompt | Always loaded | Every session |
| 2. SKILL.md body | Full instructions | Only when triggered | On activation |
| 3. Folder contents | Scripts, references, examples | Only when needed | On demand |

This means: keep the description tight (~100 tokens), keep SKILL.md focused (under 500 lines), and move large reference material to separate files that load only when the skill explicitly reads them.

## Folder Structure

```
my-skill/
├── SKILL.md              # Core instructions (required, under 500 lines)
├── references/            # Detailed reference docs (loaded on demand)
│   ├── guide.md
│   └── specs.md
├── examples/              # Input/output examples (loaded on demand)
│   └── examples.md
└── scripts/               # Executable scripts (run, not loaded into context)
    └── helper.py
```

### What Goes In the Folder vs. External

| In the Folder | Point Externally |
|--------------|-----------------|
| Specific to THIS skill | Shared across skills |
| Should travel when copied | Changes independently |
| Rubric, template, examples | CLAUDE.md, project docs, stakeholder list |

## Claude Code-Specific Features

These frontmatter fields are available in Claude Code skills:

| Field | Purpose |
|-------|---------|
| `name` | Lowercase, hyphens, max 64 chars. Gerund form preferred (`analyzing-data`) |
| `description` | The trigger. Front-load key use case. Under 250 chars recommended. |
| `allowed-tools` | Tools the skill can use without per-use approval |
| `disable-model-invocation` | Set `true` to prevent auto-triggering (manual `/name` only) |
| `user-invocable` | Set `false` to hide from `/` menu (background knowledge only) |
| `context: fork` | Run in isolated subagent context |
| `agent` | Which subagent type to use with `context: fork` |
| `argument-hint` | Shown during autocomplete (e.g., `[slug]`, `[issue-number]`) |
| `paths` | Glob patterns limiting when skill auto-activates |
| `model` | Override the model when this skill is active |
| `effort` | Override effort level when this skill is active |

### String Substitutions

- `$ARGUMENTS` — all arguments passed when invoking
- `$ARGUMENTS[N]` or `$N` — specific argument by index
- `${CLAUDE_SKILL_DIR}` — directory containing the SKILL.md
- `${CLAUDE_SESSION_ID}` — current session ID

### Dynamic Context Injection

Use `` !`command` `` in skill content to run shell commands before the prompt is sent. Output replaces the placeholder. Useful for injecting live data (git status, API responses, etc.).

## Process: Building a Skill Step by Step

1. **Identify the task** — 3+ times rule, consistent output need, standardisation opportunity
2. **Scope it** — one sentence test. If it fails, split into multiple skills.
3. **Draft the description/trigger FIRST** — this is the most critical line. Get it right before writing anything else.
4. **List context required** — every file the agent needs, with full paths
5. **Write steps as numbered playbook** — set freedom level (tight vs loose)
6. **Add output format** — literal template or example, not description
7. **Write the gotcha section** — even if sparse initially. Placeholder for future failures.
8. **Add constraints** — rules specific to this skill only
9. **Review against 5 Skill Killers** — see checklist above
10. **Test trigger accuracy** — positive test (does it fire?), negative test (does it NOT fire for similar phrases?)
11. **Iterate based on real usage** — the gotcha section grows over time. If you're iterating on outputs, improve the skill.

## Testing & Maintenance

### The Litmus Test

After the skill runs, do you use the output directly? Or do you edit, correct, and restructure it? **If you're iterating on the output, improve the skill, not the output.**

### When to Re-Evaluate

| Trigger | What to Do |
|---------|-----------|
| New model version | Gotcha section might be solving problems the new model doesn't have |
| Switching tools | Skills are portable but behaviours differ. Validate. |
| Results degrade | Before blaming the model: did YOUR context go stale? |
| Before scaling | About to share with many people? Run proper evals first. |
| Monthly | Even if nothing seems broken. Context and examples may have gone stale. |

### Proportional Rigour

Match testing effort to stakes:
- Personal writing skill → quick spot-check
- Shared team skill → peer review, stress test
- Customer-facing or CRM-updating skill → formal evaluation with edge cases

## Additional Resources

- For the copy-and-fill template: [references/universal-template.md](references/universal-template.md)
- For detailed skill killer examples: [references/skill-killers.md](references/skill-killers.md)
- For advanced patterns (dispatcher, chaining, loops): [references/advanced-patterns.md](references/advanced-patterns.md)
- For annotated example skills: [references/example-skills.md](references/example-skills.md)

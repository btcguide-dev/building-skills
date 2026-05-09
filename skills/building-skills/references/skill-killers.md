# The 5 Skill Killers

These are the most common reasons skills fail to deliver value. Check every skill against this list before considering it complete.

---

## Killer 1: Vague or Quiet Description

**The problem:** The description is too generic, too narrow, or written in the wrong voice. The model never selects the skill because it doesn't recognise when to fire it.

**Signs:**
- The skill never activates when you expect it to
- You always have to manually invoke it with `/name`
- Similar requests trigger a different skill or no skill at all

**Bad example:**
```yaml
description: Helps with meeting preparation
```
- Too vague — "helps with" could mean anything
- No trigger phrases — the model doesn't know WHEN to fire
- No exclusions — could collide with other skills

**Good example:**
```yaml
description: >
  Use when the user says "prep for my meeting", "meeting prep",
  "get ready for [meeting name]", "I have a meeting with...",
  or references any upcoming calendar event they want to prepare for.
  Also triggers on "who am I meeting with" or "what should I know
  before my [meeting]". Researches attendees, pulls correspondence
  history, and prepares a risk-aware brief. NOT for scheduling
  meetings or sending calendar invites.
```
- Leads with exact trigger phrases
- Includes edge-case triggers people actually say
- Describes what it does (one sentence)
- Explicit NOT clause prevents collisions

**Fix:** Loud, specific, third-person. "Use when..." format. Include 5+ trigger phrases. Add NOT clauses. Test both positive and negative triggering.

---

## Killer 2: Over-Defining the Process

**The problem:** The skill railroads the model with excessively prescriptive steps for tasks that benefit from flexibility. The model follows instructions literally and produces rigid, low-quality output.

**Signs:**
- Creative output feels templated and lifeless
- The model follows steps mechanically without judgement
- Output quality is worse than just asking without the skill

**When to be prescriptive:**
- Database migrations
- Deployment pipelines
- Anything with irreversible side effects
- Data transformations with exact format requirements

**When to be loose:**
- Strategy documents
- Creative writing
- Brainstorming and ideation
- Research synthesis

**Fix:** Match freedom to task fragility. For fragile ops, lock down every step. For creative tasks, describe the goal and quality bar, then let the model choose the path.

---

## Killer 3: Stating the Obvious

**The problem:** The skill wastes tokens telling the model things it already knows. Every unnecessary paragraph dilutes the instructions that actually matter.

**Signs:**
- The skill explains basic concepts ("JSON is a data format...")
- Instructions describe model capabilities back to the model
- Sections feel like they were written for a human reader, not an agent

**Test:** For every paragraph, ask: "Does the model really need this?" If the answer is no, delete it.

**Fix:** Only include what the model doesn't already know: YOUR specific requirements, YOUR workflow, YOUR quality bar, YOUR edge cases. The model knows how to write JSON — tell it what YOUR JSON should contain.

---

## Killer 4: Missing Gotcha Section

**The problem:** No documentation of where the model goes wrong. The skill produces sub-optimal results because the model falls into its default patterns — patterns that are wrong for YOUR specific use case.

**Signs:**
- You keep correcting the same mistakes in output
- The model makes assumptions you've told it not to make (but only verbally, not in the skill)
- Output quality is inconsistent — sometimes good, sometimes off

**What belongs in the gotcha section:**
- Assumptions the model makes that it shouldn't for this task
- Default behaviours that are wrong in your context
- Edge cases that trip up the workflow
- Failures you've seen in real usage — document every one

**Format each gotcha as:**
> "I know you'll want to do X — don't. Here's why: [reason]."

**Fix:** Start the gotcha section from day 1, even if empty. Add to it every time the skill produces a bad result. This section is the skill's accumulated wisdom — it's what makes a skill genuinely valuable over time.

---

## Killer 5: Monolithic Blob

**The problem:** Everything is crammed into one massive SKILL.md file. The model loads thousands of tokens of reference material every time the skill fires, even when most of it isn't needed.

**Signs:**
- SKILL.md exceeds 500 lines
- The file includes detailed reference docs, long examples, API specs
- Loading the skill noticeably impacts context window

**Fix:**
- Keep SKILL.md under 500 lines — it's a playbook, not an encyclopedia
- Move detailed reference material to `references/` subdirectory
- Move long input/output examples to `examples/` subdirectory
- Move executable scripts to `scripts/` subdirectory
- Reference these files from SKILL.md so the model knows they exist and when to load them

---

## Quick-Check Table

Run every skill through this before shipping:

| # | Check | Pass? |
|---|-------|-------|
| 1 | Description is loud, specific, third-person with trigger phrases | |
| 2 | Steps match the freedom level the task needs (tight/loose) | |
| 3 | Every paragraph passes the "Does the model need this?" test | |
| 4 | Gotcha section exists and documents known failure patterns | |
| 5 | SKILL.md is under 500 lines with references in separate files | |

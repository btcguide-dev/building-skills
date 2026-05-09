# Advanced Skill Patterns

For skill builders who have the basics down and want to take it further. These patterns unlock more sophisticated workflows.

---

## 1. Dispatcher Skill (Meta-Routing)

**What it does:** A meta-skill that reads all requests and routes them to the relevant skill. It's a traffic controller for your skill library.

**When to use:** Essential when your library grows past 10-15 active skills. Without a dispatcher, you're relying on the agent to read through all available skill descriptions and pick the right one — which becomes unreliable with nuanced, similar skills.

**Why it matters:** Especially important when you have similar skills that should trigger in completely different scenarios. The dispatcher adds explicit routing logic rather than hoping description-matching works.

**Implementation pattern:**
```markdown
---
name: dispatching-requests
description: >
  Routes incoming requests to the correct skill. Use as the default
  entry point when the request could match multiple skills.
---

# Request Dispatcher

Read the user's request and route to the correct skill:

| Request Pattern | Route To |
|----------------|----------|
| Mentions "meeting", "calendar", attendees | /preparing-meetings |
| Mentions "research", "investigate", "find out" | /researching-with-confidence |
| Mentions "challenge", "stress test", "poke holes" | /devils-advocate |
| Mentions "morning", "start my day", "priorities" | /morning-briefing |

If the request doesn't clearly match, ask for clarification.
```

---

## 2. Skill Chaining

**What it does:** Output of one skill becomes input to the next. Either automated (one skill calls another) or manual (you take the output and feed it).

**Example chain:**
1. Research with Confidence → produces structured findings
2. Devil's Advocate → stress-tests the research
3. Executive Summary → packages for stakeholders

**Critical requirement:** Skills that chain need **clean, well-defined input and output**. If a skill's output is unstructured prose, the next skill in the chain can't reliably parse it.

**Design for chainability:**
- Define explicit output format (structured markdown, tables, JSON)
- Include a "Summary" section in output that the next skill can use as input
- Document what the skill produces and what it expects to receive

---

## 3. Loop Skills

**What it does:** Iterative skills that check → act → check again → iterate. They keep running until a condition is met or you stop them.

**Non-technical examples:**
- Marketing campaign optimisation: monitor ad performance → adjust bids → re-check → flag when ROAS drops
- Competitive intelligence: scan competitor sites → compare to baseline → alert on meaningful changes
- Content publishing: draft → review against style guide → revise → check compliance → publish

**Implementation pattern:**
```markdown
## Steps

1. Check current state: [what to measure]
2. Compare against target: [threshold or baseline]
3. If within target: report status and wait for next cycle
4. If outside target: take corrective action [specific steps]
5. Re-check after action
6. Log all changes with timestamp
7. Repeat from step 1

## Exit Conditions
- Target achieved for 3 consecutive checks
- Manual stop from user
- Maximum iterations reached ([number])
```

---

## 4. Sub-Agent Skills

**What it does:** Skills that spawn parallel workers using `context: fork`. Multiple agents research different angles simultaneously, then findings are aggregated.

**Example:** A deep research skill that launches 4 sub-agents:
- Agent 1: News and recent developments
- Agent 2: Academic papers and expert analysis
- Agent 3: Social sentiment and community discussion
- Agent 4: Competitor analysis

Results are aggregated into a unified brief.

**Implementation in Claude Code:**
```yaml
---
name: deep-research
description: Thorough multi-angle research on a topic
context: fork
agent: Explore
---

Research $ARGUMENTS from multiple angles. Launch parallel research across:
1. News and recent developments
2. Expert analysis and academic sources
3. Social sentiment and community discussion
4. Competitive landscape

Aggregate findings, cross-reference, and produce a unified brief.
```

---

## 5. State Management

**What it does:** Skills that maintain context across multiple invocations using intermediate checkpoint files.

**When to use:** Essential for long-running skills (20+ minute research sessions, multi-day workflows) where partial results need to survive session interruptions.

**Pattern:** Write intermediate results to checkpoint files:
```markdown
## State Management

- Save progress to `[project]/checkpoints/[skill-name]-checkpoint.md` after each major step
- On startup, check for existing checkpoint file
- If checkpoint exists: resume from last completed step
- If no checkpoint: start from the beginning
- Delete checkpoint file on successful completion
```

---

## 6. Nested Skills (Sub-Skills)

**What it does:** A skill folder contains another skill within it. The parent skill can invoke the child skill as part of its workflow.

**Example:** Meeting Prep skill contains a Meeting Simulation sub-skill:
```
meeting-prep/
├── SKILL.md              # Main prep workflow
├── stakeholder-context.md
├── output-template.md
├── scenarios.md
└── skills/
    └── meeting-sim/
        └── SKILL.md      # Simulates the meeting with role-play
```

The parent skill does the research and preparation. The nested skill role-plays the meeting from each attendee's perspective to stress-test talking points.

---

## 7. Multi-Tool Orchestration

**What it does:** One skill coordinates multiple connected services via MCP or APIs. A single trigger kicks off a workflow that spans several tools.

**Example:** Research → write audio script → generate voice via ElevenLabs → post to Slack. Three connections, one trigger, zero manual steps.

**Critical safeguard:** Guardrails on every write action are non-negotiable. Any skill that posts, publishes, or modifies external systems must have explicit confirmation steps or dry-run modes.

---

## 8. Meta-Skills (Skills That Build Skills)

**What it does:** Recursive pattern — skills that audit, review, or improve other skills.

**Examples:**
- **Skill Reviewer** — audits any skill file against the 5 Skill Killers checklist
- **Skill Optimiser** — rewrites descriptions for better triggering
- **Memory Hygiene** — reviews context files for staleness

The `building-skills` skill you're reading right now is an example of this pattern.

---

## Pattern Selection Guide

| Your Situation | Pattern |
|---------------|---------|
| 10+ skills and the agent picks the wrong one | Dispatcher |
| Multi-step workflow where output feeds forward | Skill Chaining |
| Ongoing monitoring or optimisation | Loop Skills |
| Research from multiple angles simultaneously | Sub-Agent Skills |
| Long-running task that might be interrupted | State Management |
| Skill has a natural sub-task that's useful independently | Nested Skills |
| Workflow spans multiple external services | Multi-Tool Orchestration |
| You want to improve your other skills | Meta-Skills |

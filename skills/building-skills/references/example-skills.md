# Annotated Example Skills

These examples from the AI Daily Brief masterclass demonstrate the patterns in action. They are **illustrative, not templates to copy wholesale** — every skill needs to be customised to your context, your sources, your quality bar, your terminology.

Study the structure and patterns, then build your own.

---

## Example 1: Meeting Prep (Complex Folder Skill)

This is the masterclass's primary example. It demonstrates: trigger phrases, external context pointers, scenario analysis, nested sub-skills, and structured output.

### What Makes It Good

**Trigger:** Lists 7+ specific phrases including edge cases ("I have a meeting with...", "who am I meeting with"). Covers how people actually ask, not just the obvious.

**Folder structure:**
```
meeting-prep/
├── SKILL.md                 # Core workflow (identify attendees → collect context → analyze → brief)
├── stakeholder-context.md   # External pointer — shared across skills, maintained separately
├── output-template.md       # Structured brief format (exec summary, attendee cards, risk scenarios)
├── scenarios.md             # Common meeting derailment patterns with prepared responses
├── examples.md              # Good vs. mediocre prep side-by-side
└── skills/
    └── meeting-sim/
        └── SKILL.md         # Nested: role-plays the meeting from each attendee's perspective
```

**Key pattern — external vs. bundled context:**
- `stakeholder-context.md` points externally because it's shared across skills and changes independently
- `scenarios.md` is bundled because it's specific to meeting prep and should travel with the skill
- `output-template.md` is bundled because the output format is the skill's core deliverable

**Gotcha section highlights:**
- "Don't assume attendee seniority from title alone" — the model defaults to weighting VPs highest
- "Don't fabricate company details — flag unknowns explicitly" — the model fills gaps with plausible fiction
- "Don't prepare generic talking points — every point must reference a specific agenda item"
- "For new contacts with no history: focus on public presence, not invented backstory"

**Nested sub-skill (meeting-sim):** Runs after the prep brief is complete. Role-plays each attendee based on their known positions and challenges the user's talking points. Its own gotcha: "Don't make all advisors agree — tension is the point."

---

## Example 2: Research with Confidence (Multi-Source with Fact-Checking)

Demonstrates: structured input gathering, parallel research, confidence scoring, and mandatory uncertainty reporting.

### What Makes It Good

**Input confirmation step:** Before researching, confirms topic, time horizon, source preferences, and depth with the user. This prevents the model from guessing and wasting effort.

**Source tiering:**
- Tier 1: McKinsey, HBR, Gartner, peer-reviewed journals
- Tier 2: TechCrunch, industry blogs, company announcements
- Tier 3: Reddit, X/Twitter threads, forums
- Primary: SEC filings, patent databases, government data

**Confidence scoring (the unique value):**
- HIGH: 3+ independent, quality sources corroborate
- MEDIUM: 2 sources, or 1 high-quality source
- LOW: single source, or contradicted by other evidence

**Mandatory "What's NOT clear yet" section:** Forces the model to flag gaps rather than filling them with plausible-sounding content. This alone makes the skill worth building.

**Gotcha section highlights:**
- "Don't present a single source as definitive"
- "Don't conflate opinion pieces with primary data"
- "Check publication dates: AI content recycles outdated facts"
- "If all sources trace to one original, that's echo chamber risk, not consensus"

### Chainability

Clean structured output (findings table with claim/confidence/sources) makes this ideal for chaining into Devil's Advocate or Executive Summary skills.

---

## Example 3: Devil's Advocate (Systematic Stress-Testing)

Demonstrates: structured critical analysis, explicit bias checking (including the model's own biases), and constructive conclusions.

### What Makes It Good

**Dual bias check:** Checks for the presenter's biases (anchoring, confirmation, sunk cost) AND the model's own biases ("I notice I'm defaulting to Y — this may reflect training patterns rather than your specific context"). This self-awareness instruction is rare and valuable.

**Verdict system:** SOLID / SHAKY / RED FLAG — forces a clear assessment rather than hedging with "it depends."

**Constructive ending:** Always ends with mitigation recommendations. "If you proceed, here's how to address these risks." This prevents the skill from being purely negative.

**Gotcha section highlights:**
- "Don't be nihilistic — challenge constructively"
- "Don't challenge for the sake of challenging"
- "Prioritise the 2-3 critiques that could actually sink this"
- "Never end with 'but overall this looks good' unless it genuinely does"
- "Explicitly flag when you notice your own model biases"

---

## Example 4: Board of Advisors (Multi-Perspective Simulation)

Demonstrates: defined advisor personas with explicit biases and blind spots, structured disagreement, and the "question nobody asked" technique.

### What Makes It Good

**Each advisor has documented biases:**
- The Strategist: overvalues optionality
- The Operator: undervalues bold bets
- The Customer Voice: deprioritises internal efficiency
- The Skeptic: status quo preference

This is critical — without documented biases, all advisors converge to the same moderate opinion (the model's default).

**Mandatory disagreement:** "Don't make all advisors agree — tension is the point." The value is in surfacing conflicting perspectives, not building consensus.

**"Question nobody asked" section:** Forces identification of what's missing from the analysis entirely. Often the most valuable output.

---

## Common Patterns Across All Examples

1. **Trigger phrases include how people actually talk**, not just the formal command name
2. **Gotcha sections are specific and experience-based**, not generic warnings
3. **Output is structured with literal templates**, not described in prose
4. **Mandatory sections force completeness** — "What's NOT clear" in research, "Question nobody asked" in board of advisors
5. **NOT clauses in descriptions** prevent collision with related skills
6. **Context is explicitly listed** with file paths, not assumed
7. **Nested skills** break complex workflows into composable pieces

---

## The Good vs. Mediocre Test

From the meeting prep examples file — this contrast illustrates why skills matter:

**Mediocre prep:**
> Attendees: Sarah (VP Ops), Mike (Dir Eng), Lisa (PM). Agenda: Q2 planning. Notes: Discuss priorities for next quarter.

No context on dynamics. No scenario prep. No specific talking points. Generic output.

**Great prep:**
> Executive Summary: Q2 planning with Sarah, Mike, and Lisa. Key dynamic: Sarah wants to expand the platform team; Mike wants to consolidate. Lisa is caught between — her roadmap depends on the outcome. Your objective: align on staffing before the budget discussion next Friday.

Specific dynamics, named tensions, prepared responses, evidence-based attendee cards. This is the difference a well-built skill makes.

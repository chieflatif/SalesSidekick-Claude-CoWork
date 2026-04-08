---
description: "Deep account research — staged intelligence pipeline producing a Platform Document. Four stages: Collect (web search), Extract (four lenses), Prism (five perspectives), Assemble (evidence-graded Platform Document). Output contract: 11-section Platform Document saved to workspace, usable by meeting prep, deal strategy, outreach, business cases, and email."
argument-hint: "[Company name]"
---

# /research — Deep Account Research

## Purpose

Conducts staged, multi-lens research on a target company. Collects raw intelligence via web search, reasons over it through four extraction lenses and five perspective shifts, then assembles a structured Platform Document with evidence grading on every claim. The Platform Document becomes the reusable intelligence layer for all downstream capabilities — meeting prep, deal strategy, outreach, business cases, and contextual email.

This is NOT a single-pass search-and-summarize. It separates **collection** from **reasoning** so the output quality is dramatically higher than raw search results.

## When to Use

- Before first outreach to a new prospect
- Preparing for a high-stakes executive meeting
- When deal strategy or business cases flagged insufficient verified data
- Refreshing intel on a dormant account
- After a self-audit recommends upgrading hypothesis-grade claims
- When any downstream capability would benefit from richer company context

## Inputs

**User provides:** Company name (required). Optionally: key contact name/title, specific research focus, or context ("meeting prep", "first outreach", "proposal").

**System reads:**
- Web search: company website, news, press releases, leadership, funding, tech analysis, job postings, competitors
- Workspace data (if company exists): current company record, deal history, call notes, existing Platform Document

**System reads from skills:**
- skills/company-intel/SKILL.md: product positioning, case studies, competitive context (for pain signal mapping)

## Interactive Intake (Voice-First)

When the user provides only a company name with no additional context, ask:

> "Researching **{Company}**. Quick questions so I can focus the research:
> 1. **Who are we meeting with?** Name and title if you have it.
> 2. **What's the context?** First outreach, meeting prep, proposal, or just getting smart?
> 3. **Anything specific to look for?** Competitors, tech stack, recent changes?"

**Skip the questions if** the user provides enough context inline. Examples:
- "Research Acme, I'm meeting their VP Sales next week" — skip, context is clear
- "Deep dive on Globex, we're competing against {{TOP_COMPETITOR_1}} there" — skip, focus is clear
- "Tell me about Initech" — ask, intent is ambiguous

**Voice-first tolerance:** Accept partial answers, combined answers, or "just get me everything." Adapt, don't demand structure.

## Execution Steps

### STAGE 1: COLLECT

Gather raw intelligence. No reasoning yet — just data collection.

1. **Web search: Company fundamentals**
   - Company website, about page, leadership team, board of directors
   - Company size (employees, revenue if public), founding year, HQ location
   - Mission statement, positioning language, taglines

2. **Web search: Recent activity**
   - Company + recent news, press releases, awards (last 6-12 months)
   - Earnings reports, funding rounds, acquisitions, partnerships
   - Leadership changes, reorgs, layoffs, expansions

3. **Web search: Key contact** (if provided)
   - Contact name + company + title
   - LinkedIn activity, published articles, speaking engagements, quotes
   - Career history, tenure at company, areas of responsibility

4. **Web search: Competitive landscape**
   - Company + competitors + industry landscape
   - Market position, analyst coverage, industry rankings
   - Technology partnerships, vendor relationships

5. **Web search: Hiring and technology signals**
   - Company + job postings (tech stack clues, growth signals, pain signals)
   - Built-with analysis, integration partners, technology mentions
   - Open roles that signal strategic priorities or gaps

6. **Load existing workspace data** (if company exists)
   - Current company record, deal history, call notes
   - Previous Platform Document (if refreshing — note what changed)

### STAGE 2: EXTRACT (Four Lenses)

Sequential reasoning over the collected data. No new searches — only analysis of what Stage 1 gathered.

**Lens 1: Company Profile**
- Synthesize: who they are, what they do, markets served, key metrics
- Org structure: decision-making hierarchy, reporting lines if discoverable
- Business model: how they make money, growth trajectory, strategic priorities

**Lens 2: Voice & Language**
- How the company talks about itself: key phrases, terminology, positioning language
- Executive quotes: how leadership frames challenges and priorities
- Tone markers: formal/casual, technical/business, aspirational/operational
- Industry jargon they use: mirror these in outreach and conversation
- What they emphasize vs. what they avoid saying

**Lens 3: Pain Signals**
- Map discovered pain points to {{PRODUCT_DESCRIPTION}} and {{PRIMARY_PRODUCT}}
- For each pain signal: evidence source, severity (High/Medium/Low), personas affected
- Distinguish between: stated pain (they said it), implied pain (job postings, news), and projected pain (industry pattern)
- If no clear pain signals found, say so honestly — don't manufacture them

**Lens 4: Strategic Intel**
- Timing triggers: contract expirations, budget cycles, fiscal year, seasonal patterns
- Competitive position: who else is in this account, incumbent strengths/weaknesses
- Technology landscape: current stack, gaps, modernization signals
- Budget signals: hiring velocity, investment announcements, cost-cutting indicators

### STAGE 3: THE PRISM (Multi-Perspective Analysis)

Five perspectives that surface non-obvious insights. Runs AFTER extraction — reasons over structured intelligence, not raw search results.

**Stakeholder View**
- What keeps the key contact (or likely buyer persona) up at night?
- What are they measured on? What would make them a hero internally?
- What political dynamics might affect a buying decision?

**Contrarian View**
- What angle is nobody else taking with this company?
- What's the counterintuitive insight that would make them lean forward?
- What assumption is every other vendor making that might be wrong?

**Operator View**
- Monday morning reality: what are the people doing the actual work dealing with?
- What's the gap between executive messaging and operational reality?
- Where is manual work or workaround culture hiding?

**Growth Pressure View**
- What breaks first if this company doubles in size?
- Where are the scaling bottlenecks that haven't hit yet but will?
- What worked at their previous size that won't work at the next level?

**Hidden Connection**
- The thread that connects seemingly unrelated data points into a story
- The insight that makes the AE sound like they've been studying this company for months
- If no hidden connection emerges naturally, skip this — don't force it

### STAGE 4: ASSEMBLE + GRADE

1. Assemble the Platform Document from Stages 2 and 3
2. Evidence-grade every claim using the three-tier system
3. Run quality check:
   - Citation coverage: what percentage of claims have sources?
   - Confidence distribution: ratio of Verified / Estimated / Hypothesis
   - 50% Rule: if >50% of quantitative claims are hypothesis-grade, flag the document
4. Save to workspace (see Data Layer below)

## Output Format — Platform Document

```
PLATFORM DOCUMENT — [Company Name]
Generated: [Date] | Sources: [X sources] | Evidence: [V] Verified [E] Estimated [H] Hypothesis

1. COMPANY SNAPSHOT
[Who, what, where, how big, how long. Key numbers. All evidence-graded.]

2. HOW THEY WIN WORK
[Their BD model. What they compete on. Their own words about their positioning.
Use actual quotes and language from their website/press — this IS the voice data.]

3. KEY CONTACT PROFILE
[Title, tenure, what they own. Career context. What to ask. What NOT to say.
If no key contact provided, profile the most likely buyer persona instead.]

4. VOICE & LANGUAGE GUIDE
[How they talk. Key phrases from their website, press, job postings.
Executive quotes. Terminology to mirror in outreach and conversation.
Tone: [formal/casual/technical/aspirational]. Mirror this.]

5. PAIN SIGNALS
[Mapped to {{PRODUCT_DESCRIPTION}}. For each signal:]
- [Evidence grade] [Pain signal] — Source: [where found]
  Severity: [High/Medium/Low] | Personas affected: [who]
  Type: Stated / Implied / Projected

6. STRATEGIC INTELLIGENCE
[Timing triggers. Budget signals. Technology landscape. Competitive position.
Contract cycles. Hiring velocity. Investment patterns.]

7. THE PRISM — What Nobody Else Is Seeing
[Five perspectives synthesized into actionable insights.
Lead with the strongest insight. Skip any perspective that produced nothing non-obvious.]

8. SELLING ANGLES
Three wedges specific to this company:
- Financial: [specific to their financial situation, evidence-graded]
- Technical: [specific to their tech stack/needs, evidence-graded]
- Strategic: [specific to their business trends/pain, evidence-graded]

9. QUALIFICATION
[Hell Yes / Evaluating / Hell No — based on ICP fit ({{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}})
and Hell Yes/Hell No signal detection. Show which signals matched.]

10. GAPS & OPEN QUESTIONS
[What we don't know. What to ask in the first meeting.
Specific questions tied to MEDDPICC gaps if a deal exists.]

11. RECOMMENDED NEXT STEP
[One specific action. Not generic "schedule a call" — tied to the research findings.
Example: "The VP Engineering just posted about scaling challenges on LinkedIn — reach out with the Growth Pressure angle."]
```

After the Platform Document, always show:

```
EVIDENCE SUMMARY
- Verified: [X claims] | Estimated: [X claims] | Hypothesis: [X claims]
- Citation coverage: [X]% of claims have sources
- 50% Rule: [PASS / FLAG — with explanation if flagged]

FILES UPDATED
| File | Action | Path |
|------|--------|------|
| Company: [slug] | Created/Updated | data/companies/[slug].md |
| Platform Doc: [slug] | Created | data/research/[company-slug]-platform.md |
| Index | Updated | data/index.md |
```

## Data Layer

**No new schemas.** The Platform Document is saved in two places:

1. **Companies record** (Notes field): Summary of key findings appended to existing notes
2. **Research directory** (if workspace exists): Full Platform Document saved as `data/research/{company-slug}-platform.md`

The `data/research/` directory is created on demand if it doesn't exist.

**Refresh behavior:** If a Platform Document already exists for this company, show the user what changed since the last research and ask: "Want me to update the existing Platform Document or create a fresh one?"

## Cross-Command Integration

When a Platform Document exists, downstream capabilities automatically use it:

| Capability | What It Uses from the Platform Document |
|---|---|
| Meeting prep | Voice & language guide + pain signals + prism insights + key contact profile |
| Deal strategy | Strategic intel + competitive position + pain signals for Five-Lens Prism |
| Business case (POV) | Pain signals + selling angles + company snapshot for 5-Component Model |
| Contextual email | Voice & language guide to mirror the company's tone |
| Outreach | Prism insights + selling angles for differentiated opening |
| Competitive analysis (/battle) | Competitive position data from strategic intel |

## Database Read/Write

**Reads:**
- Companies: existing record if available (Company Name, Industry, Notes, Tech Stack)
- Deals: deal history with the company
- Call Notes: previous interaction context
- Web search: multiple sources across 5 search categories

**Writes:**
- Companies: Updates Notes field with Platform Document summary. Updates Tech Stack, Industry, Account Size if new information found.
- Research: Creates/updates `data/research/{company-slug}-platform.md` with full Platform Document.
- Index: Updates `data/index.md` with company changes.
- Does NOT create new company records — suggests adding to pipeline for that.

## Commandment Alignment

| Commandment | How /research Serves It |
|-------------|------------------------|
| #2 Stop Digging | AI does all the research through a structured pipeline — AE gets a finished Platform Document |
| #4 Context is King | Ensures deep, multi-lens context before outreach or strategy |
| #8 Laws of Karma | Evidence-graded throughout — every claim tagged, 50% Rule enforced |
| #9 Pitch Angles | Selling angles are specific to the company's situation, not generic feature pitches |
| #10 Chief of Staff | Platform Document feeds all downstream capabilities — research once, use everywhere |

## Evidence Grading

**Mandatory on this command.** Every claim must be graded:
- Official company sources (website, press releases, SEC filings) → Verified
- Third-party reports, job postings, tech detection tools → Verified
- Inferences from available data (tech stack from job postings, pain from hiring patterns) → Estimated
- Industry assumptions, competitive guesses, projected pain → Hypothesis

**Voice & Language section:** Direct quotes from company materials → Verified. Tone assessments and language pattern analysis → Estimated.

**Prism section:** Insights grounded in specific evidence → Estimated. Speculative perspectives → Hypothesis.

**The 50% Rule applies.** If >50% of quantitative claims in the Platform Document are hypothesis-grade, flag it: "This research is heavy on projections. I'd recommend the AE gather more verified data before using this in external-facing materials."

## Graceful Degradation

| Missing Connector | Impact on /research |
|-------------------|--------------------|
| No workspace | Not in SalesSidekick project. Research still works via web search. Platform Document displayed but not saved. Capability still works from conversation context but nothing persists between sessions. Open your SalesSidekick workspace to save automatically. |
| No web search | Cannot conduct research. Asks user to provide company information manually. Structures whatever the user provides into the Platform Document format with evidence grading. |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Company record | Companies | If company not already tracked |
| Key contacts found | Contacts | If key personnel identified during research |
| Platform Document | Research directory | Always (if workspace exists) |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 15.4).

---
description: Deep account research — web search, structured intel brief, evidence-graded
argument-hint: "[Company name]"
intent-triggers:
  - intent: research-company
    phrases:
      - "tell me about"
      - "what do we know about"
      - "dig into"
      - "research [company]"
---

# /research — Deep Account Research

## Purpose

Conducts thorough research on a target company using web search and available connectors. Produces a structured intelligence brief covering company overview, recent news, key personnel, technology stack, competitive landscape, and strategic selling angles. Evidence-graded throughout.

## When to Use

- Before first outreach to a new prospect
- Preparing for a high-stakes executive meeting
- When /strategy or /pov flagged insufficient verified data
- Refreshing intel on a dormant account
- After /audit recommends upgrading hypothesis-grade claims

## Inputs

**User provides:** Company name (required). Optionally: specific research focus ("focus on their tech stack" or "what's their competitive situation with [competitor]").

**System reads:**
- Web search: company website, news, press releases, leadership, funding, tech analysis
- Workspace data (if company exists): current company record, deal history, call notes

## Execution Steps

1. Search the web for the company: official website, recent news, press releases, leadership changes, funding rounds, product announcements
2. If the company exists in workspace data, load existing context for comparison
3. Research technology stack (built-with analysis, job postings for tech clues, integration partners)
4. Identify key personnel: C-suite, VP-level decision makers relevant to {{PRODUCT_DESCRIPTION}}
5. Map competitive landscape: who else serves this company, what tools/vendors they use
6. Identify strategic selling angles based on findings
7. Evidence-grade every claim throughout the brief
8. Save the structured brief to Companies record (Notes field) if the company exists
9. If the company doesn't exist in records, suggest: "Want me to create this account? Run `/add-company`."

## Output Format

```
🔎 RESEARCH BRIEF — [Company Name]
Generated: [Date] | Sources: [X web sources]

COMPANY OVERVIEW:
- Industry: [industry]
- Size: [employees] | Revenue: [if public/available]
- HQ: [location]
- Founded: [year]
- Website: [url]
- [Verified] [Key fact from official source]
- [Verified] [Key fact from official source]

RECENT NEWS & EVENTS (last 6-12 months):
- [Date] [Verified] [Headline + source] — Selling relevance: [why this matters]
- [Date] [Verified] [Headline + source] — Selling relevance: [why this matters]
- [Date] [Verified] [Headline + source] — Selling relevance: [why this matters]

KEY PERSONNEL:
| Name | Title | Relevance | LinkedIn |
|------|-------|-----------|---------|
| [Name] | [Title] | [Decision maker / Influencer / User] | [if found] |

TECHNOLOGY STACK:
- [Verified] Known tools: [from website, job postings, built-with data]
- [Estimated] Likely tools: [inferred from industry, size, job postings]
- Integration opportunities: [how {{PRIMARY_PRODUCT}} connects]

COMPETITIVE LANDSCAPE:
- [Verified] Current vendors: [known from research]
- [Estimated] Likely incumbent in our space: [based on signals]
- Displacement opportunity: [assessment]

STRATEGIC SELLING ANGLES:
1. [Financial angle] — [specific to their situation, evidence-graded]
2. [Technical angle] — [specific to their tech stack/needs]
3. [Strategic angle] — [tied to their business trends or pain points]

EVIDENCE SUMMARY:
- Verified: [X claims] | Estimated: [X claims] | Hypothesis: [X claims]

ICP FIT: [Hell Yes / Evaluating / Hell No] — based on [{{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}}]

RECOMMENDED NEXT STEP:
[Specific action based on research findings]
```

## Database Read/Write

**Reads:**
- Companies: existing record if available (Company Name, Industry, Notes, Tech Stack)
- Deals: deal history with the company
- Call Notes: previous interaction context
- Web search: multiple sources

**Writes:**
- Companies: Updates Notes field with research brief summary. Updates Tech Stack, Industry, Account Size if new information found.
- Does NOT create new company records — suggests /add-company for that.

## Commandment Alignment

| Commandment | How /research Serves It |
|-------------|------------------------|
| #2 Stop Digging | AI does all the research — AE gets a finished brief |
| #4 Context is King | Ensures full context before outreach or strategy |
| #8 Laws of Karma | Evidence-graded throughout — every claim tagged |

## Evidence Grading

**Mandatory on this command.** Every claim must be graded:
- Official company sources (website, press releases, SEC filings) → Verified
- Third-party reports, job postings, tech detection tools → Verified
- Inferences from available data (tech stack from job postings) → Estimated
- Industry assumptions, competitive guesses → Hypothesis

## Graceful Degradation

| Missing Connector | Impact on /research |
|-------------------|--------------------|
| No workspace | Not in SalesSidekick project. Research still works via web search. Results displayed but not saved. Capability still works from conversation context but nothing saves between sessions. Open your SalesSidekick workspace to save automatically. |
| No web search | Cannot conduct research. Asks user to provide company information manually. Structures whatever the user provides into the brief format. |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Company record | Companies | If company not already tracked |
| Key contacts found | Contacts | If key personnel identified during research |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

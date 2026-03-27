---
description: Bulk call transcript ingestion — populate your entire pipeline from a folder of call recordings or transcripts
argument-hint: "[paste transcripts or describe where they are]"
---

# /ingest-calls — Bulk Transcript Ingestion

## Purpose

The fastest way to populate SalesSidekick with rich deal intelligence. Instead of processing calls one at a time, drop in a batch of transcripts — a month, a quarter, whatever you have — and the system maps your entire territory from the conversations.

This is the "cold start killer." An AE who provides 20-50 call transcripts walks out with a fully populated pipeline: companies, contacts with roles and sentiment, deals with MEDDPICC scoring, competitive intelligence, relationship maps, and pattern memory — all extracted from real conversations.

## When to Use

- First week with SalesSidekick — populate everything at once
- When onboarding a new territory with historical call data
- After a period of not using the system — catch up in one session
- Anytime you have a batch of transcripts to process

## Inputs

**User provides:**
- Call transcripts in any format — pasted text, files in a folder, screenshots, copied from a recording tool
- Can be provided all at once or in batches: "Here are my calls from the last month"
- Optionally: "These are all from Acme" or "These are from Q4" for batch context

**System reads:**
- Existing Companies, Contacts, Deals — to match against and avoid duplicates
- Existing Call Notes — to avoid re-processing calls already in the system

## Execution Steps

### Phase 1: Intake, Scanning & Opportunity Mapping

The user might have 3 months of calls — 50-100+ transcripts. The system needs to scan, cluster, and map before doing any deep processing. This phase is lightweight and fast.

1. **Scan all transcripts — headers only first.** For each transcript, read just enough to extract:
   - Company name (from introductions, email addresses, context clues — first 30 seconds of a call almost always names the company)
   - Primary participant names and titles (from introductions or "this is [name] from [company]")
   - Approximate date (from filename, metadata, date references, or user context)
   - Don't read the full transcript yet. Just enough to classify it.

2. **Cluster by company.** Group all transcripts belonging to the same company. Use fuzzy matching — "Acme," "Acme Corp," "ACME Inc." are the same company. People's names help disambiguate: if Sarah Chen appears in 5 transcripts with different company name variations, those are the same company.

3. **Identify opportunities within each company.** A single company may have multiple deals:
   - Same company but different products discussed → separate opportunities
   - Same company but different buying groups / divisions → separate opportunities
   - Same company, same deal arc across time → single opportunity
   - Flag ambiguous cases for user confirmation

4. **Sort by deal intelligence density.** Order companies by:
   - Number of transcripts (more calls = richer intelligence)
   - Recency (more recent calls = more relevant)
   - Active vs closed (if context suggests a deal closed, deprioritize)

5. **Present the territory map:**
   > "I scanned **47 transcripts** across **3 months** and identified **12 companies** with **14 opportunities:**
   >
   > | Company | Calls | Date Range | Contacts Found | Likely Stage |
   > |---------|-------|------------|----------------|-------------|
   > | Acme Corp | 8 | Jan 5 → Mar 15 | 4 (Sarah Chen, Mike Torres, ...) | Negotiation |
   > | Globex Inc | 6 | Jan 20 → Mar 10 | 3 | Technical Eval |
   > | Initech | 5 | Feb 1 → Mar 20 | 4 | Discovery |
   > | Dunder Mifflin | 3 | Feb 15 → Mar 5 | 2 | Qualification |
   > | ... |
   >
   > **Flagged:** Acme Corp may have 2 separate opportunities (different product lines discussed). Want me to confirm?
   >
   > Want me to process all of them, or start with specific accounts? I'll go deepest first."

6. User confirms scope and resolves any ambiguities (multiple opportunities at same company, unclear company matches).

### Volume Handling

A typical AE has 10+ calls/week, often 30-60 minutes each. A month of transcripts can be 200,000+ words. The system handles this through **signal extraction, not full retention.**

**The principle:** Read each transcript once, extract all signals, discard the raw text. What gets stored is the structured intelligence — not the conversation. Claude's context window is the processing engine, not the storage layer.

**Processing strategy:**
- Process ONE transcript at a time. Extract signals. Write the structured call note. Move to the next.
- Never try to hold multiple full transcripts in context simultaneously.
- For each transcript, the stored call note is a **signal-dense summary** (typically 200-500 words), not the raw transcript (typically 5,000-15,000 words).
- After processing, the raw transcript is no longer needed. The signals live in the data layer.

**What gets extracted per transcript (signal categories):**
- **People signals:** Who was there, their title, role in the deal, sentiment, engagement level
- **MEDDPICC signals:** Evidence for each element — quotes, commitments, gaps identified
- **Competitive signals:** Competitors mentioned, positioning, displacement dynamics
- **Risk signals:** Budget concerns, timeline slips, stakeholder changes, engagement quality
- **Decision signals:** Process steps revealed, criteria discussed, timeline commitments
- **Action signals:** Tasks committed to, next steps agreed, deliverables promised
- **Relationship signals:** Champion strength, executive engagement, multi-threading progress

### Phase 2: Account-by-Account Processing

For each company (process one at a time, in order of most transcripts → fewest):

4. **Create or match company record.** If company doesn't exist in `data/companies/`, create it. If it exists, load existing context.

5. **Create or match contact records.** Extract every person mentioned with their title, role, and relationship to the deal. Match against existing contacts. Create new records for unknowns.

6. **Process calls chronologically, ONE AT A TIME.** For each call, in date order:
   - Read the transcript in full
   - Extract all signals (people, MEDDPICC, competitive, risk, decision, action, relationship)
   - Score MEDDPICC elements cumulatively — each call builds on the previous scores
   - Flag tasks from past calls as likely completed if later context suggests it
   - Write a signal-dense call note to `data/call-notes/` (200-500 words, NOT the raw transcript)
   - Skip coaching feedback (not useful retroactively)
   - Skip follow-up email (already sent)
   - **Release the transcript from context before loading the next one**

7. **Synthesize deal record.** After processing all calls for a company:
   - Create or update the deal with cumulative MEDDPICC scores
   - Set deal stage based on the most recent call's context
   - Calculate deal health from aggregated signal analysis
   - Write relationship map: who's involved, their role, sentiment trajectory across the call arc

8. **Extract patterns.** Run pattern extraction across the signal summaries (NOT raw transcripts):
   - What dynamics emerged across the call arc?
   - Were there turning points?
   - What competitive patterns surfaced?
   - Write 1-3 patterns to `data/patterns/`

9. **Present account summary before moving to the next:**
   > "**Acme Corp** — 5 calls processed (Jan 15 → Mar 10)
   > - Deal: Enterprise Platform — Negotiation stage — $380K
   > - MEDDPICC: M🟡 E🟢 D🟡 D🟡 P🔴 I🟢 C🟢 C🟡 — Health: 62
   > - 4 contacts mapped: Sarah Chen (Champion), Mike Torres (EB), ...
   > - 2 patterns extracted, 3 open tasks identified
   > - Key risk: Paper process not started, close date in 6 weeks
   >
   > Moving to Globex next..."

### Phase 2.5: Per-Account Verification (runs after each company)

After processing all calls for a company, run a self-audit before moving to the next:

**Data integrity check:**
- Does every contact appear in at least one call note? (no orphan contacts)
- Does the deal's MEDDPICC score have evidence for every non-Red element? (no unearned scores)
- Are call notes in chronological order with no date conflicts?
- Does the relationship map match the contacts created?
- Are all cross-references valid? (deal → company, contacts → company, call notes → deal)

**Signal consistency check:**
- Do MEDDPICC scores progress logically across the call arc? (no unexplained jumps from Red to Green)
- Do risk signals align with deal stage? (e.g., late-stage deal shouldn't have Red on Pain)
- Are competitive mentions consistent? (same competitor named the same way across calls)

**Present any issues to the user before proceeding:**
> "Before I move to Globex — I flagged 2 things on the Acme data:
> 1. The EB score jumped from Red to Green between calls on Feb 3 and Feb 10 — I inferred Sarah Chen has EB authority from context. Correct?
> 2. There's a 3-week gap between calls 4 and 5 (Feb 10 → Mar 3). Anything happen in between I should know about?
>
> If these look right, I'll continue."

**If no issues found:** Move to the next company silently. Don't slow the user down with "everything looks clean" messages on every account.

### Phase 3: Territory Synthesis

10. After all accounts processed, run a **territory-level audit:**

**Cross-account consistency:**
- Are any contacts appearing under multiple companies? (could be valid — multi-company contacts — or a clustering error)
- Are any companies suspiciously similar? (merger of names that should have been clustered together)
- Do competitor mentions tell a consistent story across accounts?
- Are deal amounts/stages realistic relative to each other?

**Data completeness audit:**
- How many transcripts were processed vs provided? (should be 100%)
- What percentage of calls resulted in at least one MEDDPICC signal? (below 50% = calls may not be sales conversations)
- How many contacts have no title? (flag for user to fill in)
- How many deals have 3+ Red MEDDPICC elements? (flag as needing attention)

**Present the territory-level report:**
    > "**Ingestion complete.**
    >
    > DATA CREATED:
    > - Companies: 8 (5 new, 3 updated)
    > - Contacts: 22 (18 new, 4 updated)
    > - Deals: 7 active, 1 closed
    > - Call notes: 47 processed
    > - Patterns: 9 extracted
    > - Tasks: 12 open (from recent calls only)
    >
    > AUDIT RESULTS:
    > ✅ All 47 transcripts processed
    > ✅ All cross-references valid
    > ⚠️ 3 contacts have no title — want me to list them so you can fill in?
    > ⚠️ 2 deals have 4+ Red MEDDPICC elements — may need early-stage attention
    >
    > TOP PRIORITIES:
    > 1. [Most urgent deal action]
    > 2. [Biggest risk to address]
    > 3. [Biggest opportunity spotted]
    >
    > Want me to run a full pipeline review?"

## Output Format

Progress-style output. One account summary at a time. Final territory synthesis at the end.

## Database Read/Write

**Reads:**
- All existing entity types (to avoid duplicates)

**Writes:**
- Companies: new or updated records
- Contacts: new or updated records
- Deals: new or updated records with cumulative MEDDPICC
- Call Notes: one per transcript
- Tasks: extracted action items (from recent calls only — old tasks marked as likely completed)
- Patterns: extracted from call arcs
- Index: full update after each account

## Commandment Alignment

| Commandment | How /ingest-calls Serves It |
|-------------|---------------------------|
| #1 Speed is Life | Populate an entire territory in one session instead of weeks |
| #2 Stop Digging | AI does all the extraction — user just provides raw material |
| #4 Context is King | Every future command has deep historical context from day one |
| #7 One Source of Truth | All intelligence flows into the workspace data layer |
| #10 Be the Chief of Staff | System surfaces priorities the AE didn't know they had |

## Evidence Grading

- Company/contact data extracted from transcripts → Verified (from direct conversation)
- MEDDPICC scores → Estimated (system's assessment from transcript evidence)
- Deal stage/health → Estimated (inferred from most recent call context)
- Pattern extraction → Hypothesis (observed once per deal arc, needs more data to confirm)

## Graceful Degradation

| Missing Connector | Impact on /ingest-calls |
|-------------------|------------------------|
| No workspace | Cannot persist. Asks user to open SalesSidekick workspace first. This command requires data persistence. |
| No Drive | User pastes transcripts directly. No auto-discovery. |
| All other connectors | No impact. |

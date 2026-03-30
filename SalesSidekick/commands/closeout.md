---
description: Post-call processing — 6-Output Framework (MEDDPICC, tasks, coaching, email, risks, competitive intel)
argument-hint: "[paste call transcript or describe the call]"
---

# /closeout — Post-Call Processing

## Purpose

The flagship command. Processes any sales call through the 6-Output Framework, extracting maximum intelligence from every conversation. Transforms a raw call transcript into your workspace — MEDDPICC scores, tasks, coaching, and deal updates — ensuring zero intelligence loss between sessions.

Alias: `/call`

## When to Use

- Immediately after any sales call (discovery, demo, negotiation, check-in)
- When processing a call transcript from a recording tool
- After an important meeting where you need structured follow-up

## Inputs

**User provides:**
- Call transcript (pasted directly, or auto-discovered from Drive if connected)
- Company name (if not obvious from transcript)
- (Optional) Specific emphasis: "Focus on competitive intel" or "I need the follow-up email ASAP"

**System reads:**
- Companies: current company record for context
- Contacts: known contacts at the company
- Deals: active deal record (stage, MEDDPICC scores, previous risks)
- Call Notes: previous calls for comparison and pattern detection

## Execution Steps

### First-Use Calibration (runs once, first time /closeout is used)
0. If this is the first time the user runs /closeout, ask: "What call elements matter most to you? For example: MEDDPICC scoring, coaching feedback, follow-up email, risk signals?" Use the response to weight the 6-Output Framework priority in the call-processing skill. This runs once and is remembered for all future /closeout calls.

### Standard Execution
1. If transcript not provided and Drive connected, check for recent recordings. Otherwise ask user to paste transcript.

### Step 1.5: Check for Existing Analysis (BEFORE full processing)

Before running the 6-Output Framework, check if this call has already been processed:

1a. Search `data/call-notes/` for any file matching the same company AND same date (compare `company` + `date` fields in YAML frontmatter).

1b. If existing call note found:
   - Present it with a file link: "I already processed a call with [Company] on [Date]. Here's the existing analysis: [View call notes](computer:///[workspace-path]/data/call-notes/[id].md)"
   - Show a summary: call type, MEDDPICC snapshot, number of tasks extracted
   - Ask: "Want me to reprocess this, or is the existing analysis what you need?"
   - If user says reprocess → continue to Step 2 (flag as reprocess, not new)
   - If user says existing is fine → present the existing analysis and stop

1c. If multiple calls to the same company on the same date exist (valid — e.g., morning discovery + afternoon follow-up), show all and let the user confirm this is a new call.

1d. If no existing call note found → continue to Step 2 as normal.

2. Identify the company and match to Companies record. If no match, ask "Want me to add them to your pipeline first?"
3. Identify participants and match to Contacts.
4. Load current deal record including existing MEDDPICC scores and risk levels.
5. Process transcript through the **6-Output Framework**:

### Output 1: MEDDPICC Scoring
6. Score all 8 MEDDPICC elements (Red/Yellow/Green) based on what was discussed:
   - M — Metrics: Were business metrics or ROI discussed?
   - E — Economic Buyer: Was the EB identified, engaged, or referenced?
   - D — Decision Criteria: Were evaluation criteria discussed?
   - D — Decision Process: Were steps/timeline/approvers clarified?
   - P — Paper Process: Was procurement, legal, or security discussed?
   - I — Identify Pain: Was pain quantified or deepened?
   - C — Champion: Was internal advocacy discussed or demonstrated?
   - C — Competition: Were competitors mentioned or positioned against?
7. For each element, provide: current score, evidence quote from transcript, gap identification, suggested discovery question for next call

### Output 2: Task Extraction
8. Extract every action item from the transcript
9. For each task: assign owner (AE / Prospect / Internal), priority (High / Medium / Low), estimate time, set due date
10. Format tasks for workspace write

### Output 3: Coaching Feedback
11. Score 5 coaching dimensions (1-5 scale):
    - Discovery Quality: Did the AE ask layered, probing questions?
    - Objection Handling: Were objections addressed with evidence and empathy?
    - Rapport Building: Was the conversation natural and trust-building?
    - Next Steps: Were concrete next steps established with dates?
    - Talk Ratio: Did the AE listen more than talk? (Exceptional: AE <40%, Solid: ~50/50, Needs Work: AE >70%)
12. For each dimension, provide specific examples from the transcript

### Output 4: Follow-Up Email
13. Draft a follow-up email in 3 tone options per call-processing skill:
    - **Professional:** Formal, appropriate for economic buyers
    - **Warm:** Friendly, appropriate for champions
    - **Concise:** Minimal, appropriate for technical evaluators
14. Email must be under 200 words, use brand voice, include one clear ask
15. If Gmail connected, offer to send. Otherwise, present as copy-paste text.

### Output 5: Risk Signals
16. Assess 6 risk categories (High / Medium / Low) per call-processing skill:
    - Champion Health: Is the champion engaged, weakening, or silent?
    - Timeline Pressure: Is the timeline holding, slipping, or nonexistent?
    - Competitive Threat: Is a competitor gaining ground?
    - Budget Concerns: Is budget confirmed, uncertain, or frozen?
    - Stakeholder Shifts: Are key stakeholders stable, changing, or disengaged?
    - Engagement Quality: Is the prospect deeply engaged or going through the motions?
17. For each risk flagged Medium or High, provide a specific mitigation action

### Output 6: Competitive Intel
18. Extract any competitive mentions from the transcript
19. Assess win probability (0.0-1.0) based on competitive position
20. Reference relevant battlecard from skills/battlecards/
21. Recommend displacement tactics if competitor is actively engaged

### Pattern Extraction (runs after Output 6)
22. Review the call for reusable patterns — signals that predict outcomes, tactics that worked or failed, competitive dynamics worth remembering.
23. If 1-2 genuine patterns are detected, write them to `data/patterns/` using the pattern-memory skill format. Before writing, check for semantic overlap with existing patterns — if similar, increment occurrences instead of creating a new file.
24. If the deal status changed to closed-won or closed-lost during this closeout, prompt:
    > "This deal just closed. Want me to run a win/loss synthesis? I'll review all call notes and deal history to extract patterns for your future deals."
    If yes, load ALL call notes and quick notes for this deal chronologically, analyze the deal arc, extract 3-5 patterns, write a deal retrospective to `data/call-notes/retro-{date}-{deal-slug}.md`, and present the synthesis.

25. Present all 6 outputs (plus any pattern notes) to the user
26. Suggest: "Want me to audit this analysis before you act on it?"

## Output Format

```
📞 CALL CLOSEOUT — [Company Name] — [Date]

1️⃣ MEDDPICC SCORECARD
| Element | Score | Evidence | Gap |
|---------|-------|----------|-----|
| M - Metrics | 🟡 | "[quote]" | Need to quantify ROI |
| E - Econ Buyer | 🔴 | No mention | Who signs? |
| D - Decision Criteria | 🟢 | "[quote]" | Covered |
| D - Decision Process | 🟡 | "[quote]" | Timeline unclear |
| P - Paper Process | 🔴 | Not discussed | Ask next call |
| I - Identify Pain | 🟢 | "[quote]" | Strong |
| C - Champion | 🟡 | "[quote]" | Needs internal test |
| C - Competition | 🟡 | "[quote]" | [Competitor] mentioned |

Overall Confidence: [High/Medium/Low]

2️⃣ ACTION ITEMS
| Task | Owner | Priority | Due | Est. |
|------|-------|----------|-----|------|
| [Task] | AE | High | [date] | 10m |
| [Task] | Prospect | Medium | [date] | — |
| [Task] | Internal | High | [date] | 15m |

3️⃣ COACHING FEEDBACK
| Dimension | Score | Notes |
|-----------|-------|-------|
| Discovery | [1-5] | [specific example] |
| Objection Handling | [1-5] | [specific example] |
| Rapport | [1-5] | [specific example] |
| Next Steps | [1-5] | [specific example] |
| Talk Ratio | [1-5] | AE: [X]% / Prospect: [Y]% |

4️⃣ FOLLOW-UP EMAIL
[3 tone options presented]

5️⃣ RISK SIGNALS
| Category | Level | Detail | Mitigation |
|----------|-------|--------|------------|
| [Category] | 🔴 | [detail] | [action] |

6️⃣ COMPETITIVE INTEL
Competitor: [Name] | Win Probability: [0.X]
[Analysis and recommended tactics]

📁 FILES UPDATED
| File | Action | Path |
|------|--------|------|
| Call Notes: [id] | Created | [Open](data/call-notes/[id].md) |
| Deal: [id] | Updated (MEDDPICC, risk, next step) | [Open](data/deals/[id].md) |
| Tasks: [N] new | Created | [Open](data/tasks/) |
| Company: [id] | Updated (last_activity) | [Open](data/companies/[id].md) |
| Patterns: [id] | Created/Updated (if any) | [Open](data/patterns/[id].md) |
| Index | Updated | [Open](data/index.md) |

💡 Want me to audit this analysis before you act on it?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Territory Status, Industry, Tech Stack, Current Products
- Contacts: Name, Title, MEDDPICC Role, Sentiment
- Deals: all 22 fields (full deal context including previous MEDDPICC scores)
- Call Notes: previous calls for pattern comparison

**Writes:**
- Call Notes: new record (Call Title, Company, Deal, Call Date, Summary, Action Items, Risk Level, Competitive Intel, Coaching Score, MEDDPICC Changes)
- Tasks: one record per extracted action item (Task, Company, Deal, Owner, Priority, Due Date, Status=Not Started, Source=Call)
- Deals: updated MEDDPICC scores (8 elements), MEDDPICC Confidence, Deal Risk, Next Step, Last Activity
- Companies: Last Activity updated

## Commandment Alignment

| Commandment | How /closeout Serves It |
|-------------|------------------------|
| #1 Speed is Life | Processes entire call in one command, <5 min for full analysis |
| #2 Stop Digging | AI extracts all intelligence — AE just validates |
| #4 Context is King | Reads all prior context before analyzing the new call |
| #6 10-Minute Rule | Every extracted task has a time estimate, all under 10 min |
| #8 Laws of Karma | Evidence-graded coaching feedback, honest risk assessment |
| #10 Be the Chief of Staff | Nothing from the call is lost — everything persisted to workspace |

## Evidence Grading

- MEDDPICC scores: Verified (based on direct transcript quotes)
- Coaching dimensions: Estimated (system's assessment of call quality)
- Win probability: Hypothesis (projection based on available competitive signals)
- Risk levels: Estimated (derived from transcript analysis + deal history)

## Graceful Degradation

| Missing Connector | Impact on /closeout |
|-------------------|---------------------|
| No workspace | All outputs generated. Not in a workspace, so results display but don't save. Open your SalesSidekick workspace to save automatically. |
| No Drive | Cannot auto-discover transcripts. Asks user to paste transcript directly. All processing works normally after that. |
| No Gmail | Follow-up email generated as copy-paste text instead of offering to send directly. |
| No Calendar | No impact. /closeout doesn't use Calendar. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Company record | Companies | If company not already tracked |
| Contacts mentioned | Contacts | If new names/titles identified in transcript |
| Action items extracted | Tasks | Always — one record per task from Output 2 |
| Deal stage update | Deals | If MEDDPICC changes or risk signals warrant stage movement |
| Call notes | Call Notes | Always — full 6-Output summary as structured record |
| Competitive intel | Deals / Battlecards | If competitor mentioned in transcript |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 15.4).

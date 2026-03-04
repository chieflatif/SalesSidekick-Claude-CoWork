---
description: Post-call processing — 6-Output Framework (MEDDPICC, tasks, coaching, email, risks, competitive intel)
argument-hint: "[paste call transcript or describe the call]"
---

# /closeout — Post-Call Processing

## Purpose

The flagship command. Processes any sales call through the 6-Output Framework, extracting maximum intelligence from every conversation. Transforms a raw call transcript into structured data across 4 Notion databases — ensuring zero intelligence loss between sessions.

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

**System reads from Notion:**
- Companies: current company record for context
- Contacts: known contacts at the company
- Deals: active deal record (stage, MEDDPICC scores, previous risks)
- Call Notes: previous calls for comparison and pattern detection

## Execution Steps

1. If transcript not provided and Drive connected, check for recent recordings. Otherwise ask user to paste transcript.
2. Identify the company and match to Notion Companies record. If no match, suggest `/add-company` first.
3. Identify participants and match to Notion Contacts.
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
10. Format tasks for Notion write

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

22. Present all 6 outputs to the user
23. Suggest: "Run `/audit` to validate this analysis?"

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

💡 Run /audit to validate this analysis?
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
| #10 Be the Chief of Staff | Nothing from the call is lost — everything persisted to Notion |

## Evidence Grading

- MEDDPICC scores: Verified (based on direct transcript quotes)
- Coaching dimensions: Estimated (system's assessment of call quality)
- Win probability: Hypothesis (projection based on available competitive signals)
- Risk levels: Estimated (derived from transcript analysis + deal history)

## Graceful Degradation

| Missing Connector | Impact on /closeout |
|-------------------|---------------------|
| No Notion | All 6 outputs still generated but NOT persisted. User receives formatted text output. Warns: "These results won't be saved. Connect Notion and re-run to persist." |
| No Drive | Cannot auto-discover transcripts. Asks user to paste transcript directly. All processing works normally after that. |
| No Gmail | Follow-up email generated as copy-paste text instead of offering to send directly. |
| No Calendar | No impact. /closeout doesn't use Calendar. |

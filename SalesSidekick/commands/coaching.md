---
description: Call pattern analysis — 5 dimensions scored 1-5 across logged calls
argument-hint: "(no arguments)"
intent-triggers:
  - intent: improve-skills
    phrases:
      - "how am I doing on calls"
      - "coaching feedback"
      - "call patterns"
      - "help me improve"
---

# /coaching — Call Pattern Analysis

## Purpose

Analyzes call performance patterns across all logged calls. Scores the AE on 5 coaching dimensions (Discovery, Objection Handling, Rapport, Next Steps, Talk Ratio), each rated 1-5. Identifies strengths, growth areas, and specific improvement recommendations with evidence from actual call data.

## When to Use

- AE wants feedback on their sales call performance
- Preparing for a coaching session with a manager
- After a series of calls to identify patterns (not just one-off feedback)
- Periodic self-assessment (monthly or quarterly)
- When win rates are down and AE wants to diagnose why

## Inputs

**User provides:** Nothing required. Optionally: time period ("last month"), specific dimension focus ("how's my discovery?"), or specific deals to analyze.

**System reads:**
- Call Notes: all logged calls with Coaching Score and Summary
- Deals: deal outcomes for win/loss correlation

## Execution Steps

1. Load all call notes from workspace data (default: last 30 days, or user-specified period)
2. If fewer than 3 calls logged, warn: "I only have [X] calls logged. Coaching analysis is most accurate with 5+ calls. Want me to analyze what I have, or log more calls with /closeout first?"
3. Analyze each coaching dimension across all calls:

| Dimension | What It Measures | Scoring Criteria |
|-----------|-----------------|------------------|
| **Discovery** | Quality of questions asked, depth of understanding gained | 1=surface questions → 5=multi-layered discovery revealing root pain |
| **Objection Handling** | How objections are addressed and reframed | 1=avoids/caves → 5=acknowledges, reframes, advances |
| **Rapport** | Relationship building, empathy, trust signals | 1=transactional → 5=genuine connection, active listening |
| **Next Steps** | Clarity and commitment of agreed actions | 1=vague "follow up" → 5=specific mutual commitments with dates |
| **Talk Ratio** | Balance of talking vs listening | 1=AE dominates (>70%) → 5=prospect-led (AE <40%) |

4. Calculate aggregate scores per dimension (average across calls)
5. Identify patterns:
   - **Strengths:** Dimensions consistently scoring 4-5
   - **Growth areas:** Dimensions consistently scoring 1-3
   - **Trends:** Improving or declining over the period
6. Correlate with deal outcomes where possible:
   - Do higher coaching scores correlate with deal advancement?
   - Are there specific dimensions that predict win/loss?
7. Generate specific improvement recommendations:
   - Tied to actual call examples (not generic advice)
   - Actionable ("In your call with [Company], you asked X — next time try Y")
8. Evidence-grade the analysis

## Output Format

```
🎯 COACHING REPORT — [Period]
Calls analyzed: [X] | Accounts: [Y]

DIMENSION SCORES:
| Dimension | Score | Trend | Assessment |
|-----------|-------|-------|------------|
| Discovery | [X.X]/5 | [↑↓→] | [1-line assessment] |
| Objection Handling | [X.X]/5 | [↑↓→] | [1-line assessment] |
| Rapport | [X.X]/5 | [↑↓→] | [1-line assessment] |
| Next Steps | [X.X]/5 | [↑↓→] | [1-line assessment] |
| Talk Ratio | [X.X]/5 | [↑↓→] | [1-line assessment] |

OVERALL: [X.X]/5 — [summary assessment]

STRENGTHS:
💪 [Dimension]: [specific evidence from calls — quote or pattern]
💪 [Dimension]: [specific evidence]

GROWTH AREAS:
📈 [Dimension]: [specific pattern observed]
   → Try this: [specific, actionable recommendation with example]
📈 [Dimension]: [specific pattern observed]
   → Try this: [specific, actionable recommendation]

CALL-BY-CALL BREAKDOWN:
| Date | Company | Discovery | Objection | Rapport | Next Steps | Talk Ratio |
|------|---------|-----------|-----------|---------|------------|------------|
| [date] | [co] | [X] | [X] | [X] | [X] | [X] |

DEAL CORRELATION:
- Deals advancing: avg coaching score [X.X]
- Deals stalling: avg coaching score [X.X]
- Key finding: [specific correlation if found]

RECOMMENDATIONS:
1. [Specific, actionable improvement tied to evidence]
2. [Specific, actionable improvement]
3. [Specific, actionable improvement]

Evidence: [Verified] scores from logged calls | [Estimated] pattern analysis and correlations | [Hypothesis] improvement projections
```

## Database Read/Write

**Reads:**
- Call Notes: Call Title, Company (relation), Call Date, Summary, Coaching Score, MEDDPICC Changes
- Deals: Stage, Deal Value — for win/loss correlation

**Writes:** None. /coaching is read-only analysis.

## Commandment Alignment

| Commandment | How /coaching Serves It |
|-------------|------------------------|
| #2 Stop Digging | AI analyzes patterns — AE gets actionable insights |
| #3 Illuminate, Don't Dictate | Shows gaps and patterns — AE decides what to work on |
| #8 Laws of Karma | Evidence from actual calls — never fabricates performance claims |

## Evidence Grading

- Individual call coaching scores from workspace data → Verified
- Aggregate dimension scores (averages, trends) → Verified (calculated from data)
- Pattern analysis and correlations → Estimated
- Improvement projections and recommendations → Hypothesis

## Graceful Degradation

| Missing Connector | Impact on /coaching |
|-------------------|--------------------|
| No workspace | Not in SalesSidekick project. Asks: "Walk me through your last 3-5 sales calls — who did you meet, what went well, what was challenging?" Produces coaching analysis from user-provided descriptions. Capability still works from conversation context but nothing saves between sessions. Accuracy improves significantly with logged call data. |
| Fewer than 3 calls | Warns about limited data. Provides analysis with low-confidence caveat. |
| No coaching scores in call notes | If calls logged without coaching dimension (pre-/closeout or manually added), offers to retroactively assess from call summaries if available. |
| All other connectors | No impact. |

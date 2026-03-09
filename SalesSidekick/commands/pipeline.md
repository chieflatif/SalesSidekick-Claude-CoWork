---
description: Full territory pipeline view — MEDDPICC scoring, risk flags, prioritized action plan
argument-hint: "(no arguments)"
intent-triggers:
  - intent: check-pipeline
    phrases:
      - "how's my pipeline"
      - "which deals need attention"
      - "territory health"
      - "deal status"
---

# /pipeline — Pipeline Health Analysis

## Purpose

Complete territory-wide pipeline view. Reads all active deals, scores them by MEDDPICC confidence, flags at-risk deals, identifies stalled opportunities, and produces a prioritized action plan so the AE knows exactly where to focus.

## When to Use

- Weekly pipeline review
- Before a manager 1:1 or pipeline review meeting
- When you feel like deals are slipping but can't pinpoint which ones
- After returning from time off to get current state

## Inputs

**User provides:** Nothing required. Optionally: "focus on deals closing this quarter" or "show me at-risk only."

**System reads from Notion:**
- Deals: all active deals (Stage ≠ Closed Won, Closed Lost)
- Companies: company context for each deal
- Tasks: open tasks per deal
- Call Notes: recency of engagement per deal

## Execution Steps

1. Read all active deals from Notion
2. For each deal, calculate a Health Score based on:
   - MEDDPICC Confidence (H/M/L)
   - Deal Risk level (H/M/L)
   - Days since Last Activity (stale threshold: >14 days)
   - Days until Close Date (urgency threshold: <30 days)
   - Open task count and overdue tasks
3. Classify each deal: Healthy, At Risk, Critical
4. Group deals by Forecast Category: Commit, Best Case, Pipeline, Omitted
5. Calculate total weighted pipeline value per category
6. Identify the top 5 priority actions across the territory
7. Produce the pipeline report

## Output Format

```
📊 PIPELINE HEALTH — {{AE_NAME}}'s Territory
Active Deals: [X] | Total Value: $[X] | Weighted: $[X]

BY FORECAST CATEGORY:
| Category | Deals | Total Value | Weighted |
|----------|-------|-------------|----------|
| Commit | [X] | $[X] | $[X] |
| Best Case | [X] | $[X] | $[X] |
| Pipeline | [X] | $[X] | $[X] |
| Omitted | [X] | $[X] | — |

DEAL DETAILS (sorted by priority):

🔴 CRITICAL:
- [Deal] at [Company] — [Stage] — $[Value] — Close: [Date]
  Risk: [reason] | MEDDPICC: [gaps] | Last activity: [X days ago]
  → Action: [specific recommendation]

🟡 AT RISK:
- [Deal] at [Company] — [Stage] — $[Value] — Close: [Date]
  Risk: [reason] | MEDDPICC: [gaps] | Last activity: [X days ago]
  → Action: [specific recommendation]

🟢 HEALTHY:
- [Deal] at [Company] — [Stage] — $[Value] — Close: [Date]
  Next step: [from deal record]

TOP 5 PRIORITY ACTIONS:
1. [Specific action for specific deal — why it's priority #1]
2. [Action]
3. [Action]
4. [Action]
5. [Action]

TERRITORY STATS:
- Average deal age: [X days]
- Deals with MEDDPICC gaps (any Red): [X of Y]
- Stale deals (>14 days no activity): [X]
- Closing this month: [X] deals worth $[X]
```

## Database Read/Write

**Reads:**
- Deals: all 22 fields for every active deal
- Companies: Company Name, Industry, Account Size
- Tasks: open task counts per deal
- Call Notes: most recent call date per deal

**Writes:** None. /pipeline is read-only analysis.

## Commandment Alignment

| Commandment | How /pipeline Serves It |
|-------------|------------------------|
| #1 Speed is Life | Full territory view in seconds |
| #3 Illuminate, Don't Dictate | Shows health status — AE decides where to focus |
| #10 Be the Chief of Staff | Proactive — surfaces problems before they become emergencies |

## Evidence Grading

- Deal values and stages from Notion → Verified
- Weighted pipeline math (stage probability × value) → Estimated
- Health score classifications → Estimated (system assessment based on data patterns)
- Priority action recommendations → Hypothesis (system-generated strategic suggestions)

## Graceful Degradation

| Missing Connector | Impact on /pipeline |
|-------------------|--------------------|
| No Notion | Cannot read deals. Asks: "List your active deals with stage, value, and close date. I'll help you prioritize." Produces manually-built pipeline view. |
| All other connectors | No impact. /pipeline reads from Notion only. |

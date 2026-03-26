---
description: Weekly pipeline summary — deal movement, MEDDPICC changes, manager 1:1 format
argument-hint: "(no arguments)"
intent-triggers:
  - intent: weekly-review
    phrases:
      - "weekly summary"
      - "pipeline review"
      - "prep for my 1:1"
      - "how was my week"
---

# /weekly — Weekly Pipeline Summary

## Purpose

Territory-wide weekly digest. Tracks deal movement, MEDDPICC score changes, and pipeline health over the past week. Produces a shareable summary formatted for manager 1:1 meetings.

## When to Use

- Monday morning for the week ahead
- Friday afternoon to summarize the week
- Before a manager 1:1 or pipeline review

## Inputs

**User provides:** Nothing required. Optionally: "for last week" or "for the week of March 3."

**System reads:**
- Deals: all active deals with Last Activity, Stage, MEDDPICC scores
- Tasks: completed tasks this week, new tasks created
- Call Notes: calls logged this week
- Companies: company names for context

## Execution Steps

1. Read all active deals and identify changes from the past 7 days:
   - Deals that moved stages
   - Deals with MEDDPICC score changes (any element changed R/Y/G)
   - New deals added this week
   - Deals closed (Won or Lost) this week
2. Identify stalled deals: no activity in >7 days
3. Calculate weekly pipeline metrics:
   - Total active pipeline value
   - Net change from last week (new deals + upgrades - downgrades - closed lost)
   - Forecast by category (Commit, Best Case, Pipeline)
4. Summarize call activity: number of calls, companies engaged
5. Summarize task completion: completed vs created ratio
6. Identify top 3 priorities for the coming week
7. Format as a shareable summary suitable for email, Slack, or 1:1 prep

## Output Format

```
📅 WEEKLY PIPELINE SUMMARY — Week of [Date]

PIPELINE SNAPSHOT:
- Active deals: [X] | Total value: $[X]
- Net change: [+/- $X] ([new deals], [stage movements], [closed])
- Forecast: Commit $[X] | Best Case $[X] | Pipeline $[X]

DEAL MOVEMENT:
📈 Moved forward:
  - [Deal] at [Company]: [Old Stage] → [New Stage] ($[value])
📉 Moved backward / New risk:
  - [Deal] at [Company]: Risk changed to [High] — [reason]
🆕 New deals:
  - [Deal] at [Company]: [Stage] — $[value]
✅ Closed Won:
  - [Deal] at [Company]: $[value]
❌ Closed Lost:
  - [Deal] at [Company]: $[value] — Reason: [if known]

MEDDPICC CHANGES:
- [Deal]: [Element] moved from [Red→Yellow] — evidence: [brief]
- [Deal]: [Element] dropped from [Yellow→Red] — concern: [brief]

STALLED DEALS (>7 days no activity):
⚠️ [Deal] at [Company] — $[value] — Last activity: [date]
   → Recommended action: [specific]

ACTIVITY THIS WEEK:
- Calls logged: [X] across [Y] accounts
- Tasks completed: [X] | New tasks created: [X]
- Accounts engaged: [list]

TOP 3 PRIORITIES NEXT WEEK:
1. [Specific priority with reasoning]
2. [Specific priority with reasoning]
3. [Specific priority with reasoning]

---
Copy-paste ready for email, Slack, or CRM notes.

[If referral_enabled = true in Project CLAUDE.md AND onboarding_week > 1:
"Enjoying SalesSidekick? Refer another AE and get a free month of the community program ($99 value). Share pipelinerebel.com/claude-cowork"]
```

## Database Read/Write

**Reads:**
- Deals: Deal Name, Stage, Value, Close Date, Forecast Category, Deal Risk, MEDDPICC scores, Last Activity, Company (relation)
- Tasks: Status, Due Date, Company (relation) — completed and created this week
- Call Notes: Call Date, Company (relation), Summary — this week's calls
- Companies: Company Name

**Writes:** None. /weekly is read-only.

## Commandment Alignment

| Commandment | How /weekly Serves It |
|-------------|----------------------|
| #1 Speed is Life | Full week recap in one command |
| #7 One Source of Truth | All data from workspace — single source |
| #10 Be the Chief of Staff | AE walks into 1:1 fully prepared |

## Evidence Grading

- Deal values, stages, and dates from workspace data → Verified
- Net pipeline change calculations → Verified (derived from data)
- Priority recommendations → Estimated (system assessment)

## Graceful Degradation

| Missing Connector | Impact on /weekly |
|-------------------|-------------------|
| No workspace | Not in SalesSidekick project. Asks: "Walk me through your week — what deals moved, what calls happened, what's coming up?" Capability still works from conversation context but nothing saves between sessions. |
| All other connectors | No impact. |

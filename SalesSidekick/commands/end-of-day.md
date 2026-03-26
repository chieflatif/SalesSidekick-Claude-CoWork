---
description: Evening wrap — reviews completed tasks, flags overdue, previews tomorrow
argument-hint: "(no arguments)"
intent-triggers:
  - intent: end-my-day
    phrases:
      - "I'm done for today"
      - "wrap up my day"
      - "anything I missed"
      - "end of day"
---

# /end-of-day — Evening Wrap

**Alias:** `/eod`

## Purpose

End-of-day ritual that ensures nothing falls through the cracks. Reviews what got done today, flags what didn't, and previews tomorrow so the AE can close their laptop with confidence.

**Read-only — does NOT write to workspace.** This is a review command, not an update command. If the AE wants to mark tasks as done or update deal status, they do that through other commands.

## When to Use

- End of the workday
- Before signing off for the weekend
- Before PTO (run it to see what needs handoff)

## Inputs

**User provides:** Nothing required.

**System reads:**
- Tasks: all tasks (comparing Status to identify completed vs still open)
- Deals: active deals for progress check
- Call Notes: calls logged today
- Companies: for context grouping

**System reads from Calendar (if connected):** Tomorrow's meetings for preview.

## Execution Steps

1. Read all tasks and identify:
   - Tasks completed today (Status = Done, recently updated)
   - Tasks that were due today but NOT completed (still open, Due Date = today)
   - Tasks that are now overdue (Due Date < today, Status ≠ Done)
2. Read call notes from today — summarize calls processed
3. Read deals and check for any stage changes or significant updates today
4. If Calendar connected, read tomorrow's meetings
5. For each tomorrow meeting, pull: company name, deal stage, recent context
6. Produce the evening wrap summary

## Output Format

```
🌙 END OF DAY — [Day, Date]

COMPLETED TODAY:
✅ [Task] — [Company]
✅ [Task] — [Company]
✅ [Task] — [Company]
[X tasks completed]

CALLS PROCESSED:
📞 [Company] — [1-line summary] (via /closeout)
📞 [Company] — [1-line summary]

NOT COMPLETED (due today):
⚠️ [Task] — [Company] — [Priority]
   → Suggestion: [reschedule to tomorrow / escalate / delegate]
⚠️ [Task] — [Company] — [Priority]
   → Suggestion: [specific recommendation]

OVERDUE (carried forward):
🔴 [Task] — [Company] — Originally due [date]
🔴 [Task] — [Company] — Originally due [date]

DEAL MOVEMENT:
📈 [Deal] moved from [Old Stage] → [New Stage]
📉 [Deal] risk changed to [High/Medium]

TOMORROW PREVIEW:
- [Time] — [Meeting with Company] — Prep tip: [1 line]
- [Time] — [Meeting with Company] — Prep tip: [1 line]
- [X] tasks due tomorrow

Nothing fell through. Rest up. 💪
```

## Database Read/Write

**Reads:**
- Tasks: Task (title), Status, Due Date, Priority, Company (relation) — all tasks
- Deals: Deal Name, Stage, Deal Risk, Last Activity, Company (relation) — active deals
- Call Notes: Call Title, Call Date, Summary, Company (relation) — today's calls
- Companies: Company Name
- Contacts: Name, Title, Company (relation) — for tomorrow meeting context

**Writes:** None. /end-of-day is strictly read-only. This command does NOT modify workspace data.

## Commandment Alignment

| Commandment | How /end-of-day Serves It |
|-------------|--------------------------|
| #1 Speed is Life | Quick scan, no manual review needed |
| #10 Be the Chief of Staff | Nothing falls through the cracks — every open item surfaced |
| #6 10-Minute Rule | Identifies what to tackle first tomorrow in bite-sized tasks |

## Evidence Grading

N/A — this command does not generate quantitative claims. It reads and displays existing data.

## Graceful Degradation

| Missing Connector | Impact on /end-of-day |
|-------------------|-----------------------|
| No workspace | Not in SalesSidekick project. Asks: "Walk me through what you accomplished today and what's still open. I'll help you organize for tomorrow." Capability still works from conversation context but nothing saves between sessions. |
| No Calendar | Asks: "Any meetings tomorrow I should know about?" Tomorrow preview built from user response instead of calendar data. All other sections work normally. |
| No Gmail | No impact. |
| No Drive | No impact. |

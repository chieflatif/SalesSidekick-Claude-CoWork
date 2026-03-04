---
description: Morning briefing — account-grouped tasks and deals needing attention
---

# /today — Morning Briefing

## Purpose

Your daily "home screen." Surfaces everything you need to know this morning: tasks due today (grouped by account), overdue items, and deals that need attention — all in one scannable view. Start every day here.

## When to Use

- First thing in the morning
- When returning from PTO or time away
- Anytime you need to know "what's on my plate right now?"

## Inputs

**User provides:** Nothing required. Optionally, can specify a date: `/today Friday` or `/today 2026-03-07`.

**System reads from Notion:**
- Tasks database: all tasks with Status ≠ Done, filtered by Due Date ≤ today
- Deals database: all active deals (Stage ≠ Closed Won, Closed Lost)
- Call Notes database: recent call notes for context
- Companies database: company names for grouping

**System reads from Calendar (if connected):** Today's meetings with attendee names and times.

## Execution Steps

1. Read all tasks from Notion where Status ≠ Done and Due Date ≤ today
2. Group tasks by Company (using the Company relation field)
3. Within each company group, sort by Priority (High → Medium → Low), then by Due Date (oldest first)
4. Flag overdue tasks (Due Date < today) with ⚠️
5. Read all active deals and identify "Deals Needing Attention":
   - Overdue activities: deals with no task activity in >7 days
   - Approaching close dates: deals with Close Date within 14 days
   - High-risk signals: Deal Risk = High or MEDDPICC Confidence = Low
   - Stale accounts: Last Activity > 14 days ago
6. If Calendar connected, read today's meetings and match attendees to Companies/Contacts in Notion
7. For each meeting found, add a quick context line: company, deal stage, last call summary, top MEDDPICC gap
8. Compile the briefing

## Output Format

```
📋 TODAY — [Day, Date]

MEETINGS (if Calendar connected):
- [Time] — [Company] ([Contact Name], [Title])
  Deal: [Deal Name] at [Stage] | Top gap: [weakest MEDDPICC element]
  Last call: [1-line summary from most recent call note]

TASKS BY ACCOUNT:

[Company Name 1]
  ⚠️ [Overdue task] — Due [date] — [Priority]
  ☐ [Task due today] — [Priority]
  ☐ [Task due today] — [Priority]

[Company Name 2]
  ☐ [Task due today] — [Priority]

UNASSIGNED / INTERNAL
  ☐ [Tasks not linked to a company]

DEALS NEEDING ATTENTION:
🔴 [Deal Name] — Stale (no activity in [X] days). Next step: [suggestion]
🟡 [Deal Name] — Close date in [X] days, MEDDPICC gaps: [elements at Red]
🔴 [Deal Name] — High risk. [Reason]. Consider /strategy [Company].

QUICK STATS:
- Tasks due today: [X] | Overdue: [X]
- Active deals: [X] | At risk: [X]
- Meetings today: [X]
```

## Database Read/Write

**Reads:**
- Tasks: Status, Due Date, Priority, Company (relation), Deal (relation), Task (title)
- Deals: Deal Name, Stage, Close Date, Deal Risk, MEDDPICC Confidence, Last Activity, Company (relation)
- Companies: Company Name
- Call Notes: Call Date, Summary, Company (relation) — most recent per company
- Contacts: Name, Title, Company (relation) — for meeting attendee matching

**Writes:** None. /today is read-only.

## Commandment Alignment

| Commandment | How /today Serves It |
|-------------|---------------------|
| #1 Speed is Life | One command, full day context in seconds |
| #6 10-Minute Rule | Tasks are already micro-sized from /closeout extraction |
| #10 Be the Chief of Staff | AE never has to ask "what's next?" — /today tells them |

## Evidence Grading

N/A — this command does not generate quantitative claims. It reads and displays existing data.

## Graceful Degradation

| Missing Connector | Impact on /today |
|-------------------|-----------------|
| No Notion | Cannot read tasks or deals. Asks: "What's on your plate today? Give me your top 3-5 priorities and I'll help you organize them." Produces a manually-built action plan. |
| No Calendar | Meetings section omitted. Task and deal sections still work. Asks: "Any meetings today I should know about?" |
| No Gmail | No impact. /today doesn't use email. |
| No Drive | No impact. /today doesn't use Drive. |

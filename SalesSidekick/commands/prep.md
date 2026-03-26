---
description: Pre-meeting intelligence brief with talking points and MEDDPICC gaps to probe
argument-hint: "[Company name]"
intent-triggers:
  - intent: prepare-meeting
    phrases:
      - "I have a meeting with"
      - "get me ready for"
      - "prep me for"
      - "what do I need to know"
---

# /prep — Pre-Meeting Intelligence Brief

## Purpose

Loads everything the system knows about an account and produces a structured prep document so the AE walks into every meeting with full context. Includes talking points, MEDDPICC gaps to probe, recent activity summary, and suggested questions.

## When to Use

- Before any customer or prospect meeting
- When preparing for a QBR or executive review
- When picking up a deal you haven't touched in a while

## Inputs

**User provides:** Company name (required).

**System reads:**
- Companies: full company record
- Contacts: all contacts at the company
- Deals: active deal(s) with the company
- Tasks: open tasks related to the company
- Call Notes: recent call history (last 3-5 calls)

**System reads from Calendar (if connected):** Meeting details (time, attendees, agenda if available).

## Execution Steps

1. Resolve company name to Companies record (fuzzy match if needed)
2. Load full company context: industry, size, tech stack, current products, territory status, Hell Yes/Hell No
3. Load all contacts at the company with roles and sentiment
4. Load active deal(s): stage, value, MEDDPICC scores, risk level, next step
5. Load last 3-5 call notes for conversation history
6. Load open tasks for context on outstanding commitments
7. If Calendar connected, match upcoming meeting with attendees to Contacts
8. Identify MEDDPICC gaps (any element at Red) and generate probing questions for each
9. Identify the top 3 talking points based on: recent activity, deal stage, open tasks, competitive context
10. Produce the prep brief

## Output Format

```
📋 PREP BRIEF — [Company Name]
Meeting: [Date/Time if from Calendar] with [Attendee Names]

COMPANY SNAPSHOT:
- Industry: [industry] | Size: [size] | Status: [Territory Status]
- Hell Yes/No: [status] | Products: [current products]
- Website: [url]

DEAL STATUS:
- Deal: [Deal Name] | Stage: [Stage] | Value: [Value]
- Close Date: [Date] | Risk: [H/M/L] | Confidence: [H/M/L]
- Next Step: [from deal record]

MEDDPICC GAPS TO PROBE:
🔴 [Element at Red] — Suggested question: "[specific question]"
🔴 [Element at Red] — Suggested question: "[specific question]"
🟡 [Element at Yellow] — Suggested question: "[specific question]"

KEY CONTACTS:
- [Name] ([Title]) — [MEDDPICC Role] — Sentiment: [status]
- [Name] ([Title]) — [MEDDPICC Role] — Sentiment: [status]

RECENT HISTORY (last [X] calls):
- [Date]: [1-line summary] | Risk: [level]
- [Date]: [1-line summary] | Risk: [level]

OPEN COMMITMENTS:
- [Task] — Owner: [who] — Due: [date] — Status: [status]

TOP 3 TALKING POINTS:
1. [Based on MEDDPICC gaps, recent calls, deal stage]
2. [Based on open tasks or commitments]
3. [Based on competitive context or risk signals]

WATCH OUT FOR:
- [Risk or concern based on call history patterns]
```

## Database Read/Write

**Reads:**
- Companies: all 12 fields
- Contacts: all 9 fields (filtered by company)
- Deals: all 22 fields (active deals with company)
- Tasks: open tasks related to the company
- Call Notes: last 3-5 calls with the company

**Writes:** None. /prep is read-only.

## Commandment Alignment

| Commandment | How /prep Serves It |
|-------------|---------------------|
| #1 Speed is Life | Full account context in one command, <1 min |
| #4 Context is King | Never walk into a meeting uninformed |
| #10 Be the Chief of Staff | AE is always prepared — proactive intel, not reactive scrambling |

## Evidence Grading

N/A — this command displays existing data. It does not generate quantitative claims.

## Graceful Degradation

| Missing Connector | Impact on /prep |
|-------------------|-----------------|
| No workspace | Not in SalesSidekick project. Asks: "Tell me about [Company] — what do they do, what's the deal status, and what happened on your last call?" Capability still works from conversation context but nothing saves between sessions. |
| No Calendar | Meeting details (time, attendees) omitted. User provides meeting context. All workspace-based sections work normally. |
| No Gmail | No impact. /prep doesn't use email. |
| No Drive | No impact. /prep doesn't use Drive. |

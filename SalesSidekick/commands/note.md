---
description: Quick capture — dump deal intel in 10 seconds, auto-classified and stored
argument-hint: "[any deal-related thought, observation, or intel]"
---

# /note — Quick Capture

## Purpose

Fastest path from thought to stored intelligence. Captures informal deal intel — hallway conversations, Slack messages, LinkedIn observations, gut feelings — and auto-classifies them into the right deal/company/contact with zero friction.

Alias: `/n`, `/brain`

## When to Use

- Between calls when you pick up a piece of intel
- After reading a LinkedIn post from a prospect/competitor
- When you remember something from a conversation
- Anytime you think "I should write this down"

## Inputs

**User provides:** Free-text note. That's it. Everything else is auto-detected.
- Optionally prefix with company: `/note @glassbox Dan mentioned budget freeze`
- Optionally tag type: `/note competitive: heard they're also looking at Gong`

**System reads:**
- Companies: match mentioned names to existing records
- Contacts: match mentioned people to existing contacts
- Deals: identify the most likely related deal
- Previous call notes: for context on relationships mentioned

## Execution Steps

1. Parse the free-text input for:
   - **Company references** — match against known companies (fuzzy match on name, people, deal names)
   - **Contact references** — match mentioned names against known contacts
   - **Intel type** — classify as one of: competitive, champion-signal, risk-signal, decision-process, relationship, general
   - **MEDDPICC relevance** — does this note change any MEDDPICC element? (only flag, don't auto-change unless clear)

2. If company cannot be identified, ask: "Which company is this about?" (one question, nothing else)

3. If company identified, resolve to the active deal (if multiple deals, pick the most recent active one)

4. Create a lightweight note file:

   **File:** `data/call-notes/note-{YYYY-MM-DD}-{HH}{MM}-{company-slug}.md`

   ```yaml
   ---
   _schema: 1
   type: call-note
   id: note-{YYYY-MM-DD}-{HHMM}-{company-slug}
   company: {company-slug}
   deal: {deal-slug}
   contacts: [{matched-contact-slugs}]
   date: {today}
   duration: ""
   call_type: note
   meddpicc_changes: {}
   signals_detected: [{if any}]
   tasks_extracted: 0
   created: {now}
   ---

   **Type:** {intel-type}

   {original note text}

   **Auto-classified:** {company} / {contact} / {deal}
   ```

5. Update `data/index.md` — update last_activity on the deal row

6. If a MEDDPICC-relevant signal is detected, flag it:
   > "Noted. This sounds like it could affect **Competition** scoring on the Glassbox deal. Want me to update C2 from Green to Yellow?"

7. If a task is implied (e.g., "need to send Dan the case study"), flag it:
   > "Want me to create a task: 'Send Dan case study'?"

8. Log to ops.log

9. Confirm with ONE line:
   > Captured → Glassbox / Dan Hayter / competitive intel

## Output Format

Minimal. The whole point is speed.

```
✅ Captured → {Company} / {Contact} / {intel-type}
{Optional: MEDDPICC flag or task suggestion — max 1 line}
```

## What This Command Does NOT Do

- No 6-Output Framework (that's /closeout)
- No coaching feedback
- No follow-up email generation
- No research
- Maximum ONE follow-up question (company identification if ambiguous)

## Database Read/Write

**Reads:**
- Companies: Company Name (for fuzzy matching)
- Contacts: Name, Company (for contact matching)
- Deals: Deal Name, Company, Stage, Status (for deal resolution)

**Writes:**
- Call Notes: one lightweight record (call_type: note)
- Deals: last_activity updated
- Index: last_activity updated on deal row

## Commandment Alignment

| Commandment | How /note Serves It |
|-------------|---------------------|
| #1 Speed is Life | Under 10 seconds from thought to stored intel |
| #4 Context is King | Captures context that would otherwise be lost between calls |
| #7 One Source of Truth | All intel flows to the same data layer, available to every command |
| #10 Be the Chief of Staff | Nothing falls through the cracks — even hallway conversations get captured |

## Evidence Grading

N/A — this command stores user-provided observations. It does not generate quantitative claims.

## Graceful Degradation

| Missing Connector | Impact on /note |
|-------------------|-----------------|
| No workspace | Note is acknowledged and displayed but not persisted. Nudge: "Open your SalesSidekick workspace to save notes between sessions." |
| All other connectors | No impact. /note uses only local workspace data. |

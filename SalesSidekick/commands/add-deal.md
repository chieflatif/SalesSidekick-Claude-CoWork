---
description: Guided deal record creation — links company + contacts, MEDDPICC defaults to Red
argument-hint: "[Company name]"
intent-triggers:
  - intent: add-account
    context: with deal context
    phrases:
      - "add to my pipeline"
      - "new deal"
      - "new opportunity"
      - "log this deal"
---

# /add-deal — Add Deal Record

## Purpose

Guided creation of a new deal record. Links the deal to an existing company and contacts, initializes all 8 MEDDPICC scores to Red (nothing is assumed), and sets up the deal for proper pipeline tracking. This is the only command that creates deal records — ensures MEDDPICC discipline from day one.

## When to Use

- New opportunity identified at an existing account
- Formalizing a prospect into a tracked deal
- After qualification confirms a real opportunity
- Moving from prospecting to active pipeline

## Inputs

**User provides:** Company name (required). Optionally: deal details upfront ("add deal at Acme Corp, $50K platform license, closing Q2").

**System reads:**
- Companies: verify company exists, load context
- Contacts: available contacts at the company for linking
- Deals: check for existing active deals (prevent unintentional duplicates)

## Execution Steps

1. Verify company exists in records:
   - If found: load company record and proceed
   - If not found: "I don't have [Company] in my records yet. Want me to run `/add-company [Company]` first?"
2. Check for existing active deals at this company:
   - If active deal exists: "There's already an active deal at [Company]: [Deal Name] at [Stage] — $[Value]. Want to add a second deal, or update the existing one?"
3. If user provided details upfront, pre-fill what's available
4. Gather deal information conversationally:
   - **Required fields:** Deal Name, Deal Value, Close Date, Stage
   - **Recommended fields:** Primary Competitor, Forecast Category
   - **Auto-set fields:** Company (linked), Created Date (today), MEDDPICC (all Red), Deal Risk (Medium default), MEDDPICC Confidence (Low default)
   - Accept natural input: "about 50K, hoping to close by end of Q2" → Deal Value: $50,000, Close Date: [end of current Q2]
5. Link contacts:
   - Show available contacts at the company: "I have these contacts at [Company]: [list]. Who's involved in this deal?"
   - If no contacts exist: "No contacts on file for [Company]. Want to add key stakeholders now, or do that later?"
6. **Initialize MEDDPICC — ALL RED:**
   - M-Metrics: Red
   - E-Economic Buyer: Red
   - D-Decision Criteria: Red
   - D-Decision Process: Red
   - P-Paper Process: Red
   - I-Identify Pain: Red
   - C-Champion: Red
   - C-Competition: Red
   - This is intentional — nothing is assumed. The AE must earn Green through verified evidence.
7. Create the deal record
8. Confirm creation and suggest immediate next steps

## Output Format

```
💰 NEW DEAL — [Deal Name]

RECORD CREATED:
| Field | Value |
|-------|-------|
| Deal Name | [name] |
| Company | [Company Name] (linked) |
| Contacts | [linked contacts or "none yet"] |
| Stage | [stage] |
| Deal Value | $[value] |
| Close Date | [date] |
| Forecast Category | [category] |
| Primary Competitor | [if known] |
| Deal Risk | Medium (default) |
| Next Step | [if provided] |

MEDDPICC INITIALIZED — ALL RED:
| Element | Score | What It Means |
|---------|-------|---------------|
| M - Metrics | 🔴 Red | No quantified business case yet |
| E - Economic Buyer | 🔴 Red | Not identified or not accessed |
| D - Decision Criteria | 🔴 Red | Unknown evaluation criteria |
| D - Decision Process | 🔴 Red | Unknown decision process |
| P - Paper Process | 🔴 Red | Unknown procurement/legal path |
| I - Identify Pain | 🔴 Red | Pain not yet validated |
| C - Champion | 🔴 Red | No champion identified |
| C - Competition | 🔴 Red | Competitive landscape unknown |

→ Use /closeout after calls to start upgrading these scores with evidence.

NEXT STEPS:
1. Run `/prep [Company]` before your next meeting
2. After the meeting, run `/closeout` to score MEDDPICC elements
3. Run `/strategy [Company]` when you need strategic direction

💡 Want me to prep for your first meeting with them?
```

## Database Read/Write

**Reads:**
- Companies: verify existence, load Company Name for linking
- Contacts: list available contacts at company for linking
- Deals: duplicate check (active deals at same company)

**Writes:**
- Deals: creates new record with all 22 fields:
  - Deal Name (Title) — user provided
  - Company (Relation→Companies) — linked
  - Contacts (Relation→Contacts) — linked if available
  - Stage — user provided (default: Prospecting)
  - Deal Value — user provided
  - Close Date — user provided
  - Forecast Category — user provided or default: Pipeline
  - Primary Competitor — user provided or empty
  - Next Step — user provided or empty
  - Deal Risk — default: Medium
  - MEDDPICC Confidence — default: Low
  - M through C (8 MEDDPICC fields) — ALL set to Red
  - Created Date — today
  - Last Activity — today
  - Notes — creation context

## Commandment Alignment

| Commandment | How /add-deal Serves It |
|-------------|------------------------|
| #1 Speed is Life | Conversational intake, not a 22-field form |
| #7 One Source of Truth | Creates in workspace, properly linked |
| #8 Laws of Karma | MEDDPICC starts at Red — nothing is assumed, everything is earned |

## Evidence Grading

- Deal value, close date, stage → Verified (user provided)
- Company linking → Verified (from workspace data)
- MEDDPICC initialization → Verified (deliberate Red defaults, not assessment)
- Forecast category if auto-assigned → Estimated

## Graceful Degradation

| Missing Connector | Impact on /add-deal |
|-------------------|--------------------|
| No workspace | Not in SalesSidekick project. Collects all information and presents it formatted. Capability still works from conversation context but nothing saves between sessions. Open your SalesSidekick workspace to save automatically. |
| All other connectors | No impact. |

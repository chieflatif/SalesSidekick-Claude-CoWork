---
description: CRM-ready forecast — weighted pipeline math, commit/upside/best-case breakdown
---

# /forecast-update — CRM-Ready Forecast

## Purpose

Generates a formatted forecast update ready to paste into Salesforce, HubSpot, or any CRM. Applies weighted pipeline math (stage probability × deal value) and breaks down by Commit, Upside, and Best Case categories.

## When to Use

- Before submitting forecast updates to CRM
- Manager asks "where are you going to land this quarter?"
- Weekly or bi-weekly forecast cadence
- End of quarter pipeline scrub

## Inputs

**User provides:** Nothing required. Optionally: "for Q1" or "closing this month only."

**System reads from Notion:**
- Deals: all active deals with Stage, Value, Close Date, Forecast Category

## Execution Steps

1. Read all active deals from Notion
2. Filter by relevant time period (current quarter by default, or user-specified)
3. Apply default stage probability weights (customizable via {{DEAL_STAGES}}):
   | Stage | Default Probability |
   |-------|-------------------|
   | Prospecting | 10% |
   | Discovery | 20% |
   | Qualification | 40% |
   | Proposal | 60% |
   | Negotiation | 80% |
   | Closed Won | 100% |
   | Closed Lost | 0% |
4. Calculate weighted value for each deal: Stage Probability × Deal Value
5. Group by Forecast Category:
   - **Commit:** Deals the AE is confident about (high MEDDPICC, late stage)
   - **Best Case (Upside):** Deals that could close but have gaps
   - **Pipeline:** Early stage, building
   - **Omitted:** Not expected this period
6. Calculate totals for each category (raw and weighted)
7. Evidence-grade all numbers
8. Format for the AE's CRM system ({{CRM_SYSTEM}})
9. Present with confidence assessment

## Output Format

```
📈 FORECAST UPDATE — [Quarter/Period]
As of: [Date] | CRM Format: {{CRM_SYSTEM}}

SUMMARY:
| Category | Deals | Raw Value | Weighted Value |
|----------|-------|-----------|----------------|
| Commit | [X] | $[X] | $[X] |
| Best Case | [X] | $[X] | $[X] |
| Pipeline | [X] | $[X] | $[X] |
| TOTAL | [X] | $[X] | $[X] |

Quota: {{QUOTA_AMOUNT}} | Gap to quota: $[X] | Coverage ratio: [X]x

COMMIT DEALS:
| Deal | Company | Stage | Value | Weighted | Close Date |
|------|---------|-------|-------|----------|------------|
| [Deal] | [Co] | [Stage] | $[X] | $[X] | [date] |
[Verified] Values from Notion. [Estimated] Weights from stage probability.

BEST CASE DEALS:
| Deal | Company | Stage | Value | Weighted | Close Date | Risk |
|------|---------|-------|-------|----------|------------|------|
| [Deal] | [Co] | [Stage] | $[X] | $[X] | [date] | [risk] |

PIPELINE DEALS:
| Deal | Company | Stage | Value | Weighted | Close Date |
|------|---------|-------|-------|----------|------------|
| [Deal] | [Co] | [Stage] | $[X] | $[X] | [date] |

RISKS TO FORECAST:
- [Deal] at [Company]: [specific risk that could change the number]
- [Deal] at [Company]: [specific risk]

UPSIDE OPPORTUNITIES:
- [Deal] at [Company]: [what could pull this in sooner]

---
CRM PASTE-READY:
[Formatted for {{CRM_SYSTEM}} — copy and paste directly]

Evidence: Deal values [Verified] from Notion | Stage probabilities [Estimated] standard weights | Gap analysis [Estimated]
```

## Database Read/Write

**Reads:**
- Deals: Deal Name, Company (relation), Stage, Deal Value, Close Date, Forecast Category, Deal Risk, MEDDPICC Confidence

**Writes:** None. /forecast-update is read-only. It produces CRM-formatted output for the AE to paste.

## Commandment Alignment

| Commandment | How /forecast-update Serves It |
|-------------|-------------------------------|
| #1 Speed is Life | Forecast done in seconds, not hours of spreadsheet work |
| #7 One Source of Truth | Notion data → CRM output. No dual-write. |
| #8 Laws of Karma | Evidence-graded math — never fudges numbers |

## Evidence Grading

**Mandatory on this command.** All numbers must be tagged:
- Deal values from Notion → Verified
- Stage probability weights → Estimated (standard multipliers, documented)
- Forecast projections (gap analysis, coverage ratio) → Estimated
- Growth assumptions or "upside" projections → Hypothesis

## Graceful Degradation

| Missing Connector | Impact on /forecast-update |
|-------------------|-----------------------------|
| No Notion | Cannot read deals. Asks: "List your deals with stage, value, close date, and forecast category. I'll build the forecast." Produces manually-built forecast. |
| No CRM connector | Default behavior — generates paste-ready text. No degradation. |
| All other connectors | No impact. |

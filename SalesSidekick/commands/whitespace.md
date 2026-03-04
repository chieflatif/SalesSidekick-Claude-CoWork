---
description: Territory expansion analysis — product gaps, upsell opportunities, underserved segments
argument-hint: "(no arguments)"
---

# /whitespace — Territory Whitespace Analysis

## Purpose

Identifies expansion opportunities across the territory by analyzing product coverage gaps, upsell potential, cross-sell opportunities, and underserved market segments. Produces a ranked opportunity list with estimated value.

## When to Use

- Quota planning and territory planning sessions
- Looking for new pipeline to fill the gap
- Manager asks "where's your next deal coming from?"
- After closing a big deal and needing to refill pipeline

## Inputs

**User provides:** Nothing required. Optionally: "focus on upsell" or "look at [specific industry segment]."

**System reads from Notion:**
- Companies: all companies with Territory Status, Industry, Current Products, Account Size, Hell Yes/Hell No
- Deals: deal history (including Closed Won) for penetration analysis
- Contacts: contact coverage across accounts

## Execution Steps

1. Read all companies from Notion
2. Analyze product penetration: for each company, compare Current Products against the full product portfolio ({{PRIMARY_PRODUCT}}, {{SECONDARY_PRODUCTS}})
3. Identify product gaps: accounts using some products but not others
4. Identify dormant accounts: Territory Status = Active but no recent deals or activity
5. Identify underserved segments: industry/size combinations with low coverage
6. Identify green field: accounts at "Prospect" status with Hell Yes signals
7. For each opportunity, estimate potential value based on Account Size and {{AVERAGE_DEAL_SIZE}}
8. Rank opportunities by: estimated value × likelihood × strategic fit
9. Produce the whitespace map

## Output Format

```
🗺️ WHITESPACE ANALYSIS — {{AE_NAME}}'s Territory

SUMMARY:
- Total accounts: [X] | Active deals: [X] | Product penetration: [X]%
- Estimated untapped value: $[X]

UPSELL OPPORTUNITIES (existing customers, new products):
| Company | Current Products | Gap Product | Est. Value | Priority |
|---------|-----------------|-------------|------------|----------|
| [Co] | [product A] | [product B] | $[X] | High |
| [Co] | [product A] | [product B, C] | $[X] | Medium |

CROSS-SELL OPPORTUNITIES (adjacent use cases):
| Company | Current Use Case | New Use Case | Est. Value | Priority |
|---------|-----------------|--------------|------------|----------|
| [Co] | [use case] | [new use case] | $[X] | High |

GREEN FIELD (new accounts with Hell Yes signals):
| Company | Industry | Size | ICP Match | Estimated Value |
|---------|----------|------|-----------|----------------|
| [Co] | [ind] | [size] | [X signals] | $[X] |

DORMANT ACCOUNTS (active but no recent engagement):
| Company | Last Activity | Products | Potential |
|---------|--------------|----------|-----------|
| [Co] | [date] | [products] | [re-engage / deprioritize] |

UNDERSERVED SEGMENTS:
| Segment | Accounts | Penetration | Opportunity |
|---------|----------|-------------|-------------|
| [Industry × Size] | [X] | [X]% | $[X] est. |

TOP 5 WHITESPACE ACTIONS:
1. [Specific next step for highest-priority opportunity]
2. [Action]
3. [Action]
4. [Action]
5. [Action]
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Territory Status, Industry, Account Size, Hell Yes/Hell No, Current Products, Last Activity, Annual Revenue
- Deals: Company (relation), Stage, Deal Value — for penetration analysis
- Contacts: Company (relation) — for contact coverage assessment

**Writes:** None. /whitespace is read-only analysis.

## Commandment Alignment

| Commandment | How /whitespace Serves It |
|-------------|--------------------------|
| #2 Stop Digging | AI analyzes the territory — AE decides where to expand |
| #3 Illuminate, Don't Dictate | Presents ranked opportunities — AE chooses which to pursue |
| #9 Pitch Angles | Frames expansion as use-case angles, not feature pushes |

## Evidence Grading

- Account data and product penetration from Notion → Verified
- Estimated deal values (based on account size × average deal size) → Estimated
- Market segment potential → Hypothesis

## Graceful Degradation

| Missing Connector | Impact on /whitespace |
|-------------------|----------------------|
| No Notion | Cannot analyze territory. Asks: "Describe your territory — how many accounts, what industries, what products do they use?" Produces a framework-based analysis from user input. |
| All other connectors | No impact. /whitespace reads from Notion only. |

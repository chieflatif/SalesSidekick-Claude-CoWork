---
description: Point of View document — 5-Component Model with evidence-graded claims
argument-hint: "[Company name]"
intent-triggers:
  - intent: build-case
    phrases:
      - "I need a business case"
      - "build a POV"
      - "executive justification"
      - "value story"
---

# /pov — Point of View Document

## Purpose

Generates a persuasive Point of View document using the 5-Component Model. A POV is not a pitch — it's a perspective piece that demonstrates you understand the prospect's world better than they do, and that you see a blind spot they've been missing.

This is the most hypothesis-prone command in the system. Evidence grading is critical.

## When to Use

- Preparing for an executive-level meeting
- Building a case for budget allocation or project prioritization
- Attempting to create urgency in a stalled deal
- When the prospect says "show me why I should care"

## Inputs

**User provides:** Company name (required). Optionally, a specific angle or topic.

**System reads:**
- Companies: full company profile
- Deals: deal context, MEDDPICC scores
- Call Notes: conversation insights
- Contacts: stakeholder map (especially Economic Buyer)

**System reads from skills:**
- skills/deal-strategy/SKILL.md: 5-Component POV Model
- skills/company-intel/SKILL.md: company data and case studies

## Execution Steps

1. Load full account context from workspace data
2. Load company-intel skill for product positioning and case studies
3. Load deal-strategy skill for the 5-Component Model framework
4. Build the POV using the **5-Component Model**:

| Component | Purpose | Content |
|-----------|---------|---------|
| **Executive Anchor** | Hook the reader in 2 sentences | Industry trend or business pressure the exec already feels |
| **Blind Spot** | Show them something they haven't considered | The hidden cost, risk, or opportunity they're missing |
| **Math of Pain** | Quantify the blind spot | Specific numbers that make the blind spot feel real and urgent |
| **Mechanism** | Present the solution approach | How this problem gets solved (outcome-focused, not feature-focused) |
| **Call to Action** | One clear next step | Low-commitment, high-value next move |

5. Evidence-grade EVERY quantitative claim:
   - Case study metrics → Verified (with source)
   - Calculations based on prospect's data → Estimated (show assumptions)
   - Industry projections → Hypothesis (flag clearly)
6. Check the 50% Rule: if more than half the Math of Pain section is hypothesis-grade, flag the document for more research
7. Present the POV draft with evidence grades visible
8. Suggest: "Run `/audit` to validate this before sharing externally?"

## Output Format

```
📄 POINT OF VIEW — [Company Name]
For: [Contact Name, Title] (Economic Buyer / Decision Maker)

EXECUTIVE ANCHOR
[2-3 sentences connecting to a pressure the exec already feels.
Reference industry trend or market shift with source.]

THE BLIND SPOT
[What they're not seeing. The hidden cost, risk, or missed opportunity.
Specific to their situation, not generic.]

THE MATH OF PAIN
[Quantified impact. Every number tagged:]
- [Verified] [Metric from case study — source cited]
- [Estimated] Based on [their data], we estimate [calculation]. Assumptions: [listed]
- [Hypothesis] Companies in [their segment] typically see [range]. Source: [industry report/pattern]

⚠️ Evidence composition: [X]% Verified / [Y]% Estimated / [Z]% Hypothesis
[50% Rule assessment]

THE MECHANISM
[How this gets solved. Outcome-first language. Not "our product does X" but
"companies who address this typically Y." Reference case study if available.]

CALL TO ACTION
[One specific, low-commitment next step. Not "buy our product" but
"let's spend 30 minutes mapping this to your specific situation."]

---
Evidence Summary: [X] Verified / [Y] Estimated / [Z] Hypothesis
Confidence Level: [High / Medium / Low — based on evidence composition]

💡 Run /audit to validate before sharing externally?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Industry, Account Size, Tech Stack, Annual Revenue
- Deals: Deal Name, Stage, Value, MEDDPICC scores (especially M-Metrics, I-Identify Pain)
- Call Notes: pain points mentioned, quantified impacts from conversations
- Contacts: Economic Buyer and Decision Maker details

**Writes:** None. /pov is read-only. The document is for the AE to review, refine, and share.

## Commandment Alignment

| Commandment | How /pov Serves It |
|-------------|-------------------|
| #3 Illuminate, Don't Dictate | POV illuminates a blind spot — prospect decides what to do |
| #8 Laws of Karma | Evidence-graded throughout — the 50% Rule is critical here |
| #9 Pitch Angles | Financial/Technical/Strategic angle, not feature pitch |

## Evidence Grading

**CRITICAL on this command — most hypothesis-prone output in the system.**

Every quantitative claim MUST be tagged:
- Case study metrics, published data → Verified (cite source)
- Calculations from available data → Estimated (show formula and assumptions)
- Industry patterns, projections → Hypothesis (flag clearly)

**The 50% Rule is especially important here.** If Math of Pain is >50% hypothesis, tell the AE: "This POV is heavy on projections. I'd recommend running /research [Company] and gathering verified data before presenting this to [Economic Buyer name]."

## Graceful Degradation

| Missing Connector | Impact on /pov |
|-------------------|----------------|
| No workspace | Not in SalesSidekick project. Asks user to describe the company, deal, and pain points. Produces a POV based on user input, but notes that evidence grading will be weaker. Capability still works from conversation context but nothing saves between sessions. |
| No company-intel skill | If company-intel hasn't been generated (pre-/setup), POV relies on user-provided context and web search. Notes that running /setup will enable richer POVs. |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| POV document | Google Drive (working folder) | If Drive connected — save as shareable document |
| POV summary | Companies (Notes field) | Always — append POV summary to company notes for reference |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

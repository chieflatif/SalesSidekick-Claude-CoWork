---
description: Point of View document — 5-Component Model with evidence-graded claims
argument-hint: "[Company name]"
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
- Platform Document (if exists): `data/research/{company-slug}-platform.md` — loads pain signals for Math of Pain, selling angles for Mechanism, company snapshot for Executive Anchor, and prism insights for Blind Spot identification

**System reads from skills:**
- skills/deal-strategy/SKILL.md: 5-Component POV Model
- skills/company-intel/SKILL.md: company data, case studies, pain signal mapping

## Execution Steps

### Step 1: Load context
1. Load full account context from workspace data
2. Load Platform Document if it exists (`data/research/{company-slug}-platform.md`)
3. Load company-intel skill for product positioning and case studies
4. Load deal-strategy skill for the 5-Component Model framework (including Context Reasoning)

### Step 2: Context Reasoning (MANDATORY — before any writing)

Run the full Context Reasoning pass from the deal-strategy skill. This is internal reasoning, not questions to the user. Use whatever the AE provided — their words, the client's message, research data, call notes, deal history — to reason through:

1. **Relationship reality** — What's the relationship? Personal or company engagement? What tone does it demand?
2. **IS vs BECOMING** — Does the research match the AE's description of this client's situation? If research says "established" but the AE says "fledgling," write to the AE's reality.
3. **Language mapping** — Identify the words this client uses. Map every term before writing. If they say "families" not "accounts," "advisors" not "reps" — use their language throughout.
4. **Framing** — Frame every pain point as an opportunity for value, not a threat. Positive capability, not disaster avoidance.
5. **Assumption check** — What am I assuming about maturity, urgency, decision-makers, budget? Flag anything that would change the POV if wrong.
6. **ROI calibration** — Default to modest, break-even framing. Deep research grounds the numbers; gentle presentation earns trust.
7. **Specificity pass** — Every section must contain something that couldn't be copy-pasted into another POV.

**If Context Reasoning surfaces a critical gap** (e.g., can't determine if the firm is established or building, don't know the relationship type, no sense of budget expectations), ask ONE focused question. Not a questionnaire — one question about the thing that would most change the output.

### Step 3: Build the POV using the 5-Component Model

| Component | Purpose | Content |
|-----------|---------|---------|
| **Executive Anchor** | Open with THEIR situation | Their words, their goals, their specific context — not a generic industry trend |
| **Blind Spot** | Show them something they haven't quantified | Framed as opportunity, in their language, specific to their situation |
| **Math of Pain** | Quantify modestly | Break-even framing, defensible numbers, research-grounded but gently presented |
| **Mechanism** | Present the approach | Outcome-focused, described in terms the buyer can picture, not jargon |
| **Call to Action** | One next step | Ideally their own idea reflected back. Proportional to the relationship stage. |

### Step 4: Evidence-grade and quality gate
5. Evidence-grade EVERY quantitative claim:
   - Case study metrics → Verified (with source)
   - Calculations based on prospect's data → Estimated (show assumptions)
   - Industry projections → Hypothesis (flag clearly)
6. Check the 50% Rule: if more than half the Math of Pain section is hypothesis-grade, flag the document for more research
7. Run the Quality Gate from the deal-strategy skill:
   - Language test: any words the buyer wouldn't use?
   - Specificity test: would this work for a different company?
   - Tone test: does it match the relationship?
   - Assumption test: did IS vs BECOMING conflicts get resolved correctly?
   - ROI test: numbers modest enough to defend?
   - Sendability test: would the AE put their name on this?
8. Present the POV draft with evidence grades visible
9. Suggest: "Want me to audit this before you share it externally?"

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
| No company-intel skill | If company-intel hasn't been generated (pre-/setup), POV relies on user-provided context and web search. Notes: "Share company docs or intel and I'll build richer business cases." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| POV document | Google Drive (working folder) | If Drive connected — save as shareable document |
| POV summary | Companies (Notes field) | Always — append POV summary to company notes for reference |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 15.4).

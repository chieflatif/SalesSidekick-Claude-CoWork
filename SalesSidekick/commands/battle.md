---
description: Competitive displacement analysis — win probability, talk tracks, and proof points
argument-hint: "[Company name]"
---

# /battle — Competitive Displacement

## Purpose

Loads the relevant competitive battlecard and applies it to a specific deal. Assesses the current competitive position, calculates win probability, and recommends specific displacement tactics with talk tracks and proof points.

## When to Use

- Competitor has been mentioned in a deal or call
- AE is preparing for a competitive bake-off or evaluation
- Prospect is comparing you directly to a specific competitor
- AE needs ammunition for a competitive conversation

## Inputs

**User provides:** Company name (required). Optionally, the competitor name if known.

**System reads:**
- Companies: company profile
- Deals: deal record including Primary Competitor field
- Call Notes: recent calls mentioning competitors
- Contacts: stakeholder map (who's aligned with which vendor)

**System reads from skills:**
- skills/battlecards/SKILL.md: per-competitor displacement playbooks

## Execution Steps

1. Load deal and company context from workspace data
2. Identify the primary competitor:
   - If specified by user, use that
   - If not, check Deal record's Primary Competitor field
   - If still unknown, check recent call notes for competitive mentions
   - If no competitor identified, ask: "Who are you competing against in this deal?"
3. Load the relevant battlecard from skills/battlecards/SKILL.md
4. Analyze the competitive position based on:
   - What the prospect has told us (from call notes)
   - The competitor's known strengths and weaknesses (from battlecard)
   - Our positioning advantages (from battlecard + company-intel)
5. Calculate win probability (0.0-1.0) based on available signals
6. Generate displacement recommendations:
   - Talk tracks: specific things to say in conversation
   - Trap questions: questions that expose competitor weaknesses
   - Landmine responses: how to respond when the competitor has set traps
   - Proof points: specific evidence that favors us
7. Evidence-grade the competitive claims
8. Present analysis

## Output Format

```
⚔️ COMPETITIVE BATTLE — [Company Name] vs [Competitor]
Deal: [Deal Name] | Stage: [Stage] | Value: [Value]

WIN PROBABILITY: [0.X] — [Assessment: Favorable / Contested / Uphill]

THEIR POSITION:
- Strengths in this deal: [specific to this account]
- Relationship advantage: [who they know, how deep]
- Known pitch angle: [what they're leading with]

OUR ADVANTAGE:
- Key differentiators: [specific to this account's needs]
- Proof points: [case studies, metrics relevant to this prospect]
- Wedge opportunity: [where we can change the evaluation criteria]

DISPLACEMENT TACTICS:

💬 Talk Tracks:
1. "[Specific thing to say when prospect mentions competitor strength]"
2. "[Specific thing to say to reframe the conversation]"
3. "[Specific thing to say to plant doubt]"

❓ Trap Questions (ask these):
1. "[Question that exposes competitor weakness]"
2. "[Question that shifts evaluation criteria in our favor]"

🛡️ Landmine Responses (they'll say → you respond):
- "[Competitor claim]" → "[Your counter]"
- "[Competitor claim]" → "[Your counter]"

📊 Proof Points:
- [Verified] [Case study or metric]
- [Estimated] [Calculated comparison]
- [Hypothesis] [Industry projection]

RECOMMENDED NEXT MOVE:
[Specific action with rationale]

💡 Run /audit to validate this analysis?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Industry, Tech Stack, Current Products
- Deals: Deal Name, Stage, Value, Primary Competitor, MEDDPICC scores
- Call Notes: competitive mentions from recent calls
- Contacts: stakeholder alignment and sentiment

**Writes:** None. /battle is read-only analysis.

## Commandment Alignment

| Commandment | How /battle Serves It |
|-------------|----------------------|
| #4 Context is King | Loads full competitive context before analysis |
| #8 Laws of Karma | Evidence-graded claims — never fabricates competitive intel |
| #9 Pitch Angles | Financial/Technical/Strategic wedges, not feature comparisons |

## Evidence Grading

**Mandatory on this command.** All competitive claims must be graded:
- Competitor features and pricing from public sources → Verified
- Competitive positioning based on call mentions → Estimated
- Win probability and displacement predictions → Hypothesis

## Graceful Degradation

| Missing Connector | Impact on /battle |
|-------------------|-------------------|
| No workspace | Not in SalesSidekick project. Asks user to describe the competitive situation. Loads battlecard from skills/battlecards/ and applies generically. Capability still works from conversation context but nothing saves between sessions. |
| No battlecards skill | If battlecards haven't been generated (pre-/setup), provides generic competitive analysis framework based on available information. Notes: "Share your competitive intel and I'll build battlecards." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Competitive intel updates | Deals (Primary Competitor field) | If competitor identified or changed |
| Battlecard refinements | skills/battlecards/SKILL.md | If new displacement tactics or talk tracks discovered |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 15.4).

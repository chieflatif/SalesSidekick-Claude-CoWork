---
description: Five-Lens Prism deal strategy with Three Paths recommendation — evidence-graded
argument-hint: "[Company name]"
---

# /strategy — Deal Strategy Analysis

## Purpose

Deep strategic analysis of a deal using the Five-Lens Prism. Examines the deal from five distinct angles, then recommends three distinct paths forward (Velocity, Diagnostic, Protective). All claims evidence-graded.

This is the thinking command — when a deal is complex, stuck, or at a crossroads and the AE needs strategic clarity.

## When to Use

- Deal is stalled or stuck between stages
- Major competitive threat detected
- AE is unsure whether to accelerate, investigate, or pause
- Before a big meeting where strategic direction matters
- Manager asks "what's your plan for this deal?"

## Inputs

**User provides:** Company name (required). Optionally, a specific concern: "worried about the champion going quiet" or "competitor just got a meeting with the CTO."

**System reads:**
- Companies: company profile, industry, size
- Deals: full deal record (stage, value, MEDDPICC scores, risk, competitor)
- Contacts: stakeholder map with roles and sentiment
- Call Notes: recent history for patterns
- Tasks: open commitments
- Platform Document (if exists): `data/research/{company-slug}-platform.md` — loads strategic intel, competitive position, and pain signals to enrich Five-Lens Prism analysis

## Execution Steps

### First-Use Calibration (runs once, first time /strategy is used)
0. If this is the first time the user runs /strategy, ask: "Do you prefer aggressive or conservative deal strategy? When deals are stuck, do you tend to push or pull back?" Use the response to calibrate Three Paths threshold in the deal-strategy skill. This runs once and is remembered for all future /strategy calls.

### Standard Execution
1. Load full account and deal context from workspace data
2. Apply the **Five-Lens Prism** — analyze the deal through each lens:

| Lens | Analyzes | Key Questions |
|------|----------|---------------|
| **Stakeholder Psychology** | Who supports us, who blocks us, who's undecided | Power map? Motivations? Political dynamics? |
| **Political Capital** | Internal dynamics at the prospect | Who has capital to spend? Who's rising/falling? Reorg risk? |
| **Competitive Dynamics** | Where we stand vs alternatives | Incumbent advantage? Competitor relationships? Our unique wedge? |
| **Hidden Leverage** | Untapped advantages we haven't used | Unused proof points? Adjacent relationships? Timing advantages? |
| **Temporal Dynamics** | Time-based pressures and windows | Budget cycles? Contract expirations? Seasonal factors? Urgency triggers? |

3. For each lens, assess strength (Strong / Mixed / Weak) with evidence
4. Synthesize into the **Three Paths** recommendation:

| Path | When | What It Means |
|------|------|---------------|
| 🔴 **Velocity** (Strike) | Strong signals, champion engaged, window closing | Accelerate. Push for next step. Propose a timeline. |
| 🟡 **Diagnostic** (Go Deep) | Mixed signals, MEDDPICC gaps, need more intel | Slow down to speed up. Run discovery. Fill gaps. |
| 🟢 **Protective** (Pause) | Red flags, stalled champion, competitive threat | Protect the relationship. Re-engage differently. |

5. For each path, provide: specific actions, timeline, expected outcomes, risks
6. Evidence-grade all claims throughout
7. Present analysis and ask: "Which path feels right? I can build out the execution plan."

## Output Format

```
🔍 DEAL STRATEGY — [Company Name]
Deal: [Deal Name] | Stage: [Stage] | Value: [Value] | Close: [Date]

FIVE-LENS PRISM ANALYSIS:

🧠 Stakeholder Psychology: [Strong/Mixed/Weak]
[Analysis with evidence quotes and assessment]

🏛️ Political Capital: [Strong/Mixed/Weak]
[Analysis with evidence and assessment]

⚔️ Competitive Dynamics: [Strong/Mixed/Weak]
[Analysis with evidence — competitor named, position assessed]

🔑 Hidden Leverage: [Strong/Mixed/Weak]
[Untapped advantages identified with specific suggestions]

⏰ Temporal Dynamics: [Strong/Mixed/Weak]
[Time pressures, windows, urgency factors]

THREE PATHS:

🔴 VELOCITY (Strike)
- Actions: [1, 2, 3]
- Timeline: [specific]
- Expected outcome: [specific]
- Risk: [what could go wrong]

🟡 DIAGNOSTIC (Go Deep)
- Actions: [1, 2, 3]
- Timeline: [specific]
- Expected outcome: [specific]
- Risk: [what could go wrong]

🟢 PROTECTIVE (Pause)
- Actions: [1, 2, 3]
- Timeline: [specific]
- Expected outcome: [specific]
- Risk: [what could go wrong]

RECOMMENDATION: Based on the analysis, [Lens X] and [Lens Y] suggest [Path].
But this is your deal — which direction feels right?

Evidence grades: [X] Verified / [Y] Estimated / [Z] Hypothesis
⚠️ [50% Rule warning if applicable]

💡 Run /audit to validate this analysis?
```

## Database Read/Write

**Reads:**
- Companies: all 12 fields
- Deals: all 22 fields
- Contacts: all 9 fields (filtered by company)
- Call Notes: last 5-10 calls
- Tasks: open tasks for the deal

**Writes:** None. /strategy is read-only analysis. The AE decides and acts.

## Commandment Alignment

| Commandment | How /strategy Serves It |
|-------------|------------------------|
| #2 Stop Digging | AI does the strategic analysis — AE orchestrates the decision |
| #3 Illuminate, Don't Dictate | Three Paths — never a single recommendation |
| #4 Context is King | Loads full context before analysis |
| #8 Laws of Karma | Evidence-graded — never presents assumptions as facts |
| #9 Pitch Angles | Identifies strategic wedges, not feature pitches |

## Evidence Grading

**Mandatory on this command.** All strategic claims must be graded:
- Deal data from workspace (stage, MEDDPICC, value) → Verified
- Competitive positioning based on battlecard + call history → Estimated
- Strategic projections (win probability, path outcomes) → Hypothesis

If >50% of the analysis is hypothesis-grade, flag it: "This strategy is heavy on assumptions. Consider running /research [Company] to gather more verified intel before acting."

## Graceful Degradation

| Missing Connector | Impact on /strategy |
|-------------------|---------------------|
| No workspace | Not in SalesSidekick project. Asks user to describe the deal situation. Produces analysis based on user-provided context only. Capability still works from conversation context but nothing saves between sessions. |
| No Calendar | No impact. /strategy doesn't use Calendar. |
| No Gmail | No impact. /strategy doesn't use email. |
| No Drive | No impact. /strategy doesn't use Drive. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Deal stage update | Deals | If Five-Lens analysis reveals stage should change |
| MEDDPICC score updates | Deals | If analysis surfaces new evidence changing element scores |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 15.4).

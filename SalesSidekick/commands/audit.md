---
description: 7-question self-critique battery — validates outputs for accuracy, gaps, and integrity
argument-hint: "[optional: specify which output to audit]"
intent-triggers:
  - intent: check-quality
    phrases:
      - "check my work"
      - "audit this"
      - "is this solid"
      - "quality check"
---

# /audit — Self-Critique Protocol

## Purpose

Forces rigorous self-examination of any output through a structured 7-question adversarial battery. Catches gaps, fabrication, unsupported assumptions, and evidence grading violations before the AE acts on the information.

This is the integrity enforcement mechanism for the entire system. It operationalizes Commandment 8 (Laws of Karma — never fabricate).

## When to Use

**Three trigger modes:**
- **Manual:** User types `/audit` to critique the most recent output or a specified prior output
- **Suggested:** After any major output (`/strategy`, `/pov`, `/battle`, `/deck`, `/closeout`), the system recommends: "Want me to audit this output?"
- **Mandatory:** During `/setup` Phase 7, the audit runs automatically on the initial configuration

## Inputs

**User provides:**
- (Optional) Reference to a specific output to audit. If omitted, audits the most recent substantial output.

**System reads:**
- The output being audited (from current session context)
- Relevant workspace data for fact-checking (if available)

## Execution Steps

1. Identify the target output (most recent, or user-specified)
2. Run the 7-Question Battery against the target:

| # | Question | What It Catches |
|---|----------|-----------------|
| 1 | **What did you miss?** | Gaps in coverage, overlooked stakeholders, missing data points |
| 2 | **What did you get wrong?** | Factual errors, misattributions, outdated information |
| 3 | **What assumptions might bite us?** | Unvalidated premises, market assumptions, competitive assumptions |
| 4 | **What didn't you consider that is potentially critical?** | Blind spots, adjacent risks, second-order effects |
| 5 | **What did you make up?** | Hallucinated facts, fabricated metrics, invented quotes — CRITICAL for AI integrity |
| 6 | **What other perspectives should we consider?** | Stakeholder viewpoints, competitive counter-arguments, risk scenarios |
| 7 | **What are the biggest opportunities for improvement?** | Quality gaps, depth opportunities, actionable next steps |

3. For each question, provide a specific, honest answer — not "nothing" unless truly nothing
4. If auditing a `/pov`, `/strategy`, or `/forecast-update` output, additionally run the Evidence Grading Check:
   - Verify all claims are tagged as Verified, Estimated, or Hypothesis
   - Check the 50% Rule: flag if more than half of the math is hypothesis-grade
   - Identify claims that should be re-graded based on available evidence
   - Recommend specific research actions to upgrade hypothesis-grade claims
5. Classify each finding by severity: Critical (act now), Notable (address before sharing), Minor (track for improvement)
6. Present findings with recommended fixes

## Output Format

```
🔍 AUDIT REPORT — [Output Being Audited]

1. WHAT DID I MISS?
   [Specific findings]

2. WHAT DID I GET WRONG?
   [Specific findings or "No factual errors identified"]

3. WHAT ASSUMPTIONS MIGHT BITE US?
   [Specific findings]

4. WHAT DIDN'T I CONSIDER?
   [Specific findings]

5. WHAT DID I MAKE UP?
   [Specific findings — zero tolerance for fabrication]

6. WHAT OTHER PERSPECTIVES?
   [Specific findings]

7. BIGGEST OPPORTUNITIES FOR IMPROVEMENT?
   [Specific findings]

EVIDENCE GRADING CHECK (if applicable):
- Claims tagged: [X/Y correctly graded]
- 50% Rule: [PASS/FAIL — N% hypothesis-grade]
- Re-grading recommendations: [list]

SEVERITY SUMMARY:
- Critical: [count] — [brief list]
- Notable: [count] — [brief list]
- Minor: [count] — [brief list]

RECOMMENDED ACTIONS:
1. [Most important fix]
2. [Second fix]
3. [Third fix]
```

## Database Read/Write

**Reads:** Relevant workspace records for fact-checking (Companies, Deals, Contacts, Call Notes — whichever pertain to the output being audited).

**Writes:** None. /audit is read-only — it produces analysis but does not modify any records.

## Commandment Alignment

| Commandment | How /audit Serves It |
|-------------|---------------------|
| #3 Illuminate, Don't Dictate | Shows what's wrong and offers fixes — AE decides what to act on |
| #8 Laws of Karma | The primary enforcement mechanism for never fabricating |
| #10 Be the Chief of Staff | Proactive quality control — catches problems before the AE shares outputs externally |

## Evidence Grading

/audit is itself the evidence grading enforcement tool. When auditing outputs from /pov, /strategy, /battle, /forecast-update, or /research, it specifically validates that:
- Every quantitative claim has a grade (Verified, Estimated, Hypothesis)
- The 50% Rule is respected
- Hypothesis-grade claims have recommended research paths to upgrade them

## Graceful Degradation

| Missing Connector | Impact on /audit |
|-------------------|-----------------|
| No workspace | Not in SalesSidekick project. Audit still runs the 7-question battery and evidence grading check based on session context alone. Notes that data verification was not possible. |
| All other connectors | No impact. /audit operates on session context. |

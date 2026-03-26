---
description: Prospecting email — account intel, Financial/Technical/Strategic angle, sub-200 words
argument-hint: "[Company name]"
intent-triggers:
  - intent: write-email
    context: new relationship
    phrases:
      - "cold email to"
      - "outreach to"
      - "prospecting email"
      - "reach out to"
---

# /outreach — Prospecting Email

## Purpose

Generates a tailored prospecting email for cold or warm outreach. Reads account intelligence, identifies the best angle (Financial, Technical, or Strategic), and drafts a short, punchy email under 200 words. Never reaches out naked — every outreach is backed by intelligence (Commandment 4).

## When to Use

- First outreach to a new prospect
- Re-engaging a dormant account
- Following up on a trigger event (funding, leadership change, expansion)
- After /research has surfaced a compelling angle

## Inputs

**User provides:** Company name (required). Optionally: specific contact name, specific angle, or trigger event context.

**System reads:**
- Companies: company profile, industry, tech stack, notes (including /research briefs)
- Contacts: target contact if specified (name, title, email)
- Deals: any prior deal history

**System reads from skills:**
- skills/brand-voice/SKILL.md: email formatting rules, vocabulary, banned phrases
- skills/company-intel/SKILL.md: product positioning and proof points

## Execution Steps

### First-Use Calibration (runs once, first time /outreach is used)
0. If this is the first time the user runs /outreach, ask: "What is your typical email tone? Paste a sample email if you have one." Use the response to calibrate email voice in the brand-voice skill. This runs once and is remembered for all future /outreach calls.

### Standard Execution
1. Load company record from workspace data (if exists)
2. Check for recent /research brief in company Notes — if none exists, suggest: "No research brief found. Want me to run `/research [Company]` first for a stronger outreach?"
3. Identify the best outreach angle:
   - **Financial:** ROI, cost savings, revenue impact — use when company has public financial pressure or growth targets
   - **Technical:** Integration, capability gaps, modernization — use when tech stack signals are strong
   - **Strategic:** Market position, competitive pressure, transformation — use when business news provides a hook
4. Select the angle automatically based on available intelligence, or use user-specified angle
5. Draft the email applying brand voice rules:
   - Subject line: lowercase, specific, no clickbait
   - Body: under 200 words
   - One clear ask (meeting, call, reply)
   - Reference specific intelligence (not generic claims)
   - No feature dumping — angle-based positioning (Commandment 9)
6. Apply 7-point voice check from brand-voice skill
7. Evidence-grade the outreach claims
8. If contact email is available and Gmail is connected, offer to send
9. If no contact email, suggest finding the right person

## Output Format

```
📧 OUTREACH — [Company Name]
Target: [Contact Name, Title] | Angle: [Financial/Technical/Strategic]

INTELLIGENCE USED:
- [Verified] [Specific fact that informed the angle]
- [Verified/Estimated] [Supporting context]

SUBJECT: [lowercase, specific subject line]

BODY:
[Under 200 words. Natural tone. One clear CTA.]

---

ANGLE RATIONALE:
Selected [angle] because: [specific reason tied to intelligence]

Alternative angles available:
- [Other angle]: [why it could also work]

VOICE CHECK: ✅ [X/7 passed] — [any flags]

Evidence: [Verified] company data from workspace/research | [Estimated] angle selection | [Hypothesis] projected relevance

💡 Want me to try a different angle? Or run /research [Company] for deeper intel?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Industry, Tech Stack, Notes (research briefs), Website, Account Size
- Contacts: Name, Title, Email, Sentiment, Notes
- Deals: prior deal history (if any)

**Writes:** None. /outreach generates email text. Sending is manual (paste) or via Gmail if connected.

## Commandment Alignment

| Commandment | How /outreach Serves It |
|-------------|------------------------|
| #4 Context is King | Never reaches out naked — always backed by intelligence |
| #8 Laws of Karma | Evidence-graded claims — never fabricates relevance |
| #9 Pitch Angles | Financial/Technical/Strategic angles, not feature lists |

## Evidence Grading

**Mandatory on this command.** All outreach claims must be graded:
- Company facts from workspace data or /research → Verified
- Angle selection rationale → Estimated
- Projected relevance or pain point assumptions → Hypothesis

If the outreach is primarily hypothesis-grade (no research, no workspace data), warn: "This outreach is based on assumptions. Consider running `/research [Company]` first for verified intelligence."

## Graceful Degradation

| Missing Connector | Impact on /outreach |
|-------------------|---------------------|
| No workspace | Not in SalesSidekick project. Asks: "Tell me about this company — industry, size, what they might need. I'll craft the outreach." Produces angle-based email from user input. Capability still works from conversation context but nothing saves between sessions. Quality improves with /research. |
| No Gmail | Default behavior — generates copy-paste email text. No degradation. |
| No brand-voice skill | Uses generic professional tone. Notes: "Run /setup to personalize your email voice." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Email draft | Companies (Notes field) | If user wants to save the outreach for reference |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

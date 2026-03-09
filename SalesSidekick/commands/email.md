---
description: Quick contextual email — existing relationships, no puffing, reads like AE wrote it
argument-hint: "[Company name] about [topic]"
intent-triggers:
  - intent: write-email
    context: existing relationship
    phrases:
      - "send a follow-up"
      - "email to"
      - "write a reply"
      - "follow up with"
---

# /email — Quick Contextual Email

## Purpose

Drafts a quick, natural email for existing relationships. Not cold outreach (/outreach handles that). Not post-call follow-up (/closeout handles that). This is everyday business communication with accounts the AE already knows — checking in on POC timelines, sharing resources, scheduling next steps, or any routine correspondence.

## When to Use

- Quick email to an existing contact about a specific topic
- Following up on a conversation or commitment
- Sharing a resource, update, or scheduling request
- Any routine business email that isn't cold outreach or post-call follow-up

## Inputs

**User provides:** Company name + topic (required). Example: `/email Acme Corp about the POC timeline` or `/email Sarah at TechCo re: contract renewal`

**System reads from Notion:**
- Companies: company profile, recent activity
- Contacts: primary contact (or specified contact), email, title
- Deals: current deal status, stage, next steps
- Call Notes: recent calls for context

**System reads from skills:**
- skills/brand-voice/SKILL.md: email formatting rules, vocabulary, banned phrases

## Execution Steps

1. Parse the user's request to identify: company, contact (if specified), and topic
2. Load company record from Notion
3. Load relevant contact(s) — if user specified a name, find that contact; otherwise use Primary Contact
4. Load current deal status and recent call notes for context
5. **Confidence check** (CRITICAL — Commandment 8):
   - If company doesn't exist in Notion: "I don't have [Company] in Notion. Want me to draft a generic version, or do you want to give me context first?"
   - If no recent call notes: "I don't have recent call notes for [Company]. Want me to draft based on the deal record alone, or give me a quick update?"
   - If contact info missing: "I found [Company] but no contact on file. Who should this go to?"
6. **Draft the email — NO PUFFING:**
   - Use context to inform tone and relevance ONLY
   - If the user says "POC timeline," the email is about POC timeline — period
   - Do NOT wedge in deal value, competitive landscape, case studies, or any context that doesn't directly serve the stated topic
   - Short. Natural. Reads like the AE wrote it in 90 seconds
   - Apply brand voice rules (lowercase subjects, vocabulary substitutions, banned phrases)
7. Apply 7-point voice check
8. If Gmail is connected, offer to send
9. Present with evidence grades

## Output Format

```
📧 EMAIL — [Company Name] re: [Topic]
To: [Contact Name] ([email]) | Context: [Deal Stage, Last Activity]

SUBJECT: [lowercase, natural subject line]

BODY:
[Short, natural email. Directly addresses the stated topic. No filler. No puffing.]

---

CONTEXT USED (not included in email):
- Deal: [Stage] — $[Value]
- Last call: [Date] — [1-line summary]
- Open tasks: [relevant tasks if any]

CONFIDENCE: [High/Medium/Low] — [what context was available vs missing]
Evidence: [Verified] contact and deal data from Notion | [Estimated] tone and framing

💡 Want me to adjust the tone or add anything?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Last Activity, Notes
- Contacts: Name, Title, Email, Company (relation)
- Deals: Stage, Deal Value, Next Step, Close Date
- Call Notes: recent 2-3 calls (Call Date, Summary)

**Writes:** None. /email generates text. Sending is manual (paste) or via Gmail if connected.

## Commandment Alignment

| Commandment | How /email Serves It |
|-------------|---------------------|
| #1 Speed is Life | Draft in seconds, not minutes of composing |
| #4 Context is King | Uses Notion context to inform (not pad) the email |
| #8 Laws of Karma | Confidence check — honest about what it knows and doesn't know. No puffing. |

## Evidence Grading

- Contact details and deal data from Notion → Verified
- Email tone and framing choices → Estimated
- If drafted without full context (missing call notes, etc.) → flag as reduced confidence

## Graceful Degradation

| Missing Connector | Impact on /email |
|-------------------|-----------------|
| No Notion | Cannot load context. Asks: "Tell me about the situation — who's it to, what's the context, what do you need to say?" Drafts from user-provided context only. Notes reduced confidence. |
| No Gmail | Default behavior — generates copy-paste text with subject and body. No degradation. |
| No brand-voice skill | Uses generic professional tone. Notes: "Run /setup to personalize your email voice." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Email draft | Companies (Notes field) | If user wants to save the email for reference |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

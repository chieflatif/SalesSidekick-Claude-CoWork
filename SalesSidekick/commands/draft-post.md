---
description: LinkedIn post — 3-Type Framework, brand voice, 6-point pre-publish checklist
argument-hint: "[optional: topic or post type]"
intent-triggers:
  - intent: write-post
    phrases:
      - "LinkedIn post about"
      - "draft a post"
      - "write something for LinkedIn"
      - "social media post"
---

# /draft-post — LinkedIn Post

## Purpose

Drafts a LinkedIn post using the 3-Type Framework (Personal, Business, Educational). Applies the AE's brand voice, structures for engagement, and runs a 6-point pre-publish checklist before presenting. Designed to build the AE's personal brand and create inbound pipeline opportunities.

## When to Use

- AE wants to post on LinkedIn but needs help with structure or content
- After a win, event, or insight worth sharing
- Maintaining posting cadence (target: 4-5/week per posting guide)
- Repurposing a deal insight, customer story, or industry observation

## Inputs

**User provides:** Topic or idea (required). Optionally: post type preference, specific audience, or draft text to refine.

Examples:
- `/draft-post about the importance of multi-threading in enterprise sales`
- `/draft-post personal story about losing a deal and what I learned`
- `/draft-post educational framework for qualifying enterprise deals`

**System reads from skills:**
- skills/posting-guide/SKILL.md: 3-Type Framework, structure templates, hook formulas, frequency goals, pre-publish checklist
- skills/brand-voice/SKILL.md: vocabulary, tone, banned phrases

**System reads from Notion:**
- LinkedIn Posts: recent posts for variety (avoid repeating types or topics)

## Execution Steps

### First-Use Calibration (runs once, first time /draft-post is used)
0. If this is the first time the user runs /draft-post, ask: "What LinkedIn topics resonate with your audience? And who is your target audience on LinkedIn?" Use the response to refine {{LINKEDIN_TOPICS}} and {{LINKEDIN_AUDIENCE}} in the posting-guide skill. This runs once and is remembered for all future /draft-post calls.

### Standard Execution
1. Parse the user's topic and determine the best post type:
   - **Personal:** Story → Lesson → Takeaway. First-person, vulnerable, relatable. Hook: personal experience.
   - **Business:** Insight → Proof → Application. Data-driven, authoritative. Hook: contrarian take or surprising stat.
   - **Educational:** Framework → Steps → Result. Teach something useful. Hook: "Here's how to..." or numbered list.
2. If user didn't specify a type, select based on topic fit and recent posting history (vary types)
3. Draft the post using the appropriate structure template:
   - Strong hook (first 2 lines visible in feed — must stop the scroll)
   - Body structured per type template
   - Clear takeaway or call-to-engagement
   - Appropriate length (typically 150-300 words for LinkedIn)
4. Apply brand voice rules (vocabulary substitutions, banned phrases, tone)
5. Check against recent posts in Notion — flag if topic or type was used recently
6. Run 6-point pre-publish checklist:
   1. **Hook test:** Would you stop scrolling? First 2 lines must create curiosity or tension.
   2. **Value test:** Does the reader learn something or feel something?
   3. **Voice test:** Does this sound like {{AE_NAME}}, not generic AI?
   4. **Length test:** Appropriate for LinkedIn (not too short, not a blog post)?
   5. **CTA test:** Is there a clear call-to-engagement (question, invitation, challenge)?
   6. **Brand safety test:** Nothing that could embarrass {{COMPANY}} or damage relationships?
7. Present draft with checklist results
8. Offer to save to LinkedIn Posts database in Notion

## Output Format

```
📝 LINKEDIN POST — [Type: Personal/Business/Educational]
Topic: [topic] | Audience: {{LINKEDIN_AUDIENCE}}

---

[Full post text, ready to copy-paste]

---

PRE-PUBLISH CHECKLIST:
✅/❌ Hook test: [assessment]
✅/❌ Value test: [assessment]
✅/❌ Voice test: [assessment]
✅/❌ Length test: [X words]
✅/❌ CTA test: [assessment]
✅/❌ Brand safety: [assessment]

Result: [X/6 passed]

POSTING CONTEXT:
- Recent posts: [last 3 post types and dates from Notion]
- Suggested posting time: [based on {{LINKEDIN_AUDIENCE}}]
- This week's count: [X of 4-5 target]

💡 Want me to try a different type or angle? Save to Notion as draft?
```

## Database Read/Write

**Reads:**
- LinkedIn Posts: recent posts (Post Type, Scheduled/Posted Date, Post Title) — for variety check

**Writes:**
- LinkedIn Posts: saves draft if user confirms (Post Title, Post Type, Status=Draft, Hook, Content)

## Commandment Alignment

| Commandment | How /draft-post Serves It |
|-------------|--------------------------|
| #1 Speed is Life | Post drafted in seconds, not 30 minutes of staring at a blank screen |
| #4 Context is King | Uses brand voice and audience context — not generic content |
| #8 Laws of Karma | Brand safety check — never publishes something that damages trust |

## Evidence Grading

- Post content is creative output, not factual claims — evidence grading applies only to:
  - Any statistics or metrics cited in the post → must be Verified or clearly attributed
  - Company claims or case study references → must be Verified
  - Industry trends or projections → label as Estimated or Hypothesis
- Brand voice compliance → Verified (checked against skill)

## Graceful Degradation

| Missing Connector | Impact on /draft-post |
|-------------------|-----------------------|
| No Notion | Cannot check recent posts or save drafts. Generates post without variety check. Notes: "Connect Notion to track your posting history and save drafts." |
| No posting-guide skill | Uses generic LinkedIn best practices. Notes: "Run /setup to personalize your posting framework." |
| No brand-voice skill | Uses generic professional tone. Notes: "Run /setup to personalize your voice." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Post draft | LinkedIn Posts | Always — save as draft with post type, hook, and content |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

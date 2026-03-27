---
description: Optional deep personalization — sharpen SalesSidekick beyond what organic use captures
argument-hint: "(optional: phase name — 'battlecards', 'brand voice', 'connectors', etc.)"
---

# Deep Personalization Session

## Purpose

An optional deep personalization session that captures context the system can't easily learn organically — competitive battlecards from structured research, calibrated brand voice from writing samples, explicit territory numbers, and bulk database creation.

**This is not required.** SalesSidekick works from the first conversation (see CLAUDE.md Section 15.2). Deep personalization sharpens output for users who want to go all-in. Most users reach this naturally after a few sessions, when the system nudges them (see Section 15.5).

## When to Use

- When the system suggests it ("Want to spend 15 minutes sharpening things up?")
- When the user explicitly asks to personalize or calibrate
- To build structured competitive battlecards (vs. one-off competitor notes)
- To calibrate brand voice from writing samples (vs. gradually learning from edits)
- To capture explicit-only variables (quota, fiscal year, deal stages) that can't be inferred
- To set up connectors or create databases in bulk
- After changing companies, products, or territory

**Not needed for:**
- First-time use (the getting-started conversation in Section 15.2 handles that)
- Users who are happy with progressive personalization through organic capture

## Inputs

**System reads first:** Before asking anything, check all existing Tier 2 variables and databases. Skip anything already known. Acknowledge what you already have: "I already know you're [name] at [company] selling [product] — let's focus on what I don't have yet."

**User provides (conversational — not a form):**
1. Identity details not yet captured (title, territory size, quota, fiscal year, deal stages, CRM)
2. Competitor names for structured battlecard generation (3-5)
3. Communication style preference + optional writing samples (2-3 emails)
4. Presentation brand tokens (screenshot or URL for extraction)
5. Connector availability (auto-detected + confirmed)

## Execution Steps

> **Key rule:** Every phase detects what's already known and skips redundant questions. Each phase can run independently — the user can say "just do battlecards" or "calibrate my brand voice" and skip everything else.

### Phase 1: Identity & Territory (~3 min)

**Skip if:** All explicit variables already populated.

1. Check the Global CLAUDE.md identity block for already-populated variables. Also check local workspace files for any existing data. CLAUDE.md Section 13 documents the full list of variables.
2. For each unpopulated **explicit** variable, ask conversationally — batch related questions together
3. Key explicit variables to capture if missing:
   - {{AE_TITLE}} — title/role
   - {{TERRITORY_SIZE}}, {{TERRITORY_TYPE}} — territory details
   - {{QUOTA_AMOUNT}}, {{FISCAL_YEAR_START}} — quota and fiscal timing
   - {{AVERAGE_DEAL_SIZE}}, {{SALES_CYCLE_LENGTH}} — deal economics
   - {{CRM_SYSTEM}} — CRM platform
   - {{MANAGER_NAME}}, {{TEAM_NAME}}, {{REGION}} — organizational context
   - {{DEAL_STAGES}} — pipeline stage names
4. If already inferred from organic use, confirm rather than re-ask:
   - {{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}} — these are normally inferred from deal patterns, but deep personalization can capture them directly if not yet available
5. Write all captured values to the Global CLAUDE.md identity block (within `<!-- SALESSIDEKICK-IDENTITY-START/END -->` markers — read the file first, replace only the SalesSidekick section). Also update signal thresholds in the Project CLAUDE.md if average deal size was captured.
6. Confirm: "Identity locked in. [summary of what was captured]."

### Phase 2: Company Intelligence (~5-10 min, AI-assisted)

**Skip if:** `skills/company-intel/SKILL.md` has already been regenerated with rich content.

7. Using {{COMPANY_URL}} and {{PRODUCT_DESCRIPTION}}, research the company via web search
8. Generate draft `skills/company-intel/SKILL.md` with: company overview, product portfolio, market position, key differentiators, pricing context, case studies by vertical, key metrics
9. Present the draft to the user for review and refinement
10. Save the company intel to `knowledge-bases/company-intel.md` in the workspace. This is a knowledge base file — always accessible, editable, and survives plugin updates.

**Value beyond organic capture:** Structured, comprehensive company profile vs. fragments gathered across individual interactions.

### Phase 3: Competitive Landscape (~5-10 min, AI-assisted)

**Skip if:** Battlecards already exist for 3+ competitors.

11. Check existing competitor variables — use competitors already captured organically
12. Ask if there are additional competitors to add (up to 5 total)
13. For each competitor, research via web search
14. Generate battlecard content with per-competitor sections: overview, strengths, weaknesses, displacement strategy, talk tracks, proof points, trap questions, landmine responses
15. Present battlecards to user for validation — competitors know things AI doesn't
16. Save to `knowledge-bases/battlecards.md` in the workspace
17. Populate any missing {{TOP_COMPETITOR_N}} variables

**Value beyond organic capture:** Systematic displacement playbooks for each competitor vs. ad-hoc competitive notes from individual calls.

### Phase 4: Brand Voice (~3-5 min)

**Skip if:** `skills/brand-voice/SKILL.md` has been calibrated from writing samples.

18. Present voice dimension options: formal↔casual, data-driven↔narrative, concise↔detailed
19. Optionally ask user to paste 2-3 sample emails for voice extraction
20. Generate `skills/brand-voice/SKILL.md` with: vocabulary rules, email formatting preferences, banned phrases, 7-point voice check
21. Populate {{COMMUNICATION_STYLE}}, {{EMAIL_SIGN_OFF}}
22. If not yet captured: ask about LinkedIn posting goals (topics, target audience)
23. Populate {{LINKEDIN_TOPICS}}, {{LINKEDIN_AUDIENCE}}

**Value beyond organic capture:** Calibrated from actual writing samples vs. gradually learned from corrections over 5-10 emails.

### Phase 5: Presentation Brand (~3-5 min)

**Skip if:** Brand tokens already exist in `skills/pptx/SKILL.md`.

24. Ask user for a screenshot of existing presentation or marketing material, OR company website URL
25. Extract brand tokens: primary colors, font families, layout patterns
26. Store brand tokens in `skills/pptx/SKILL.md` Brand Configuration section
27. Generate one sample slide for user verification
28. Adjust tokens based on feedback

**Value beyond organic capture:** Explicit brand token extraction vs. best-effort from company website.

### Phase 6: Connectors (~2 min)

**Skip if:** Connectors already verified.

29. Auto-detect available connectors (Gmail, Calendar, Drive, Gamma)
30. For each detected connector, verify access and record status
31. Note connector status for this session's behavior
32. Confirm graceful degradation rules for any missing connectors

**Value beyond organic capture:** Connector verification and awareness in one pass.

### Phase 7: Verification (~2 min)

37. Run smoke test: create a test company record, write and read back, then delete
38. Verify Global CLAUDE.md identity block contains all captured data
39. Run the self-audit capability on the configuration
40. Fix any issues found during audit

---

41. **Update Global CLAUDE.md identity block — automatic.**

This is the most important output of the deep personalization session. Read `/mnt/.claude/CLAUDE.md`, find the `<!-- SALESSIDEKICK-IDENTITY-START/END -->` markers, and replace the content between them with the complete updated identity block using all captured data. Leave all content outside the markers untouched.

If auto-write fails, present the block for copy-paste: "I wasn't able to save your settings automatically. Here's the updated block — paste it into your Cowork settings."

Say: "I've updated your identity settings with everything we just captured. Every future session will know all of this from the first message."

42. Produce the Personalization Complete summary.

## Output Format

**Per phase:** Conversational progress updates. User confirms before advancing to the next phase. No command syntax, no technical jargon — speak like a senior colleague, not a setup wizard.

**If running a single phase:** Just run that phase and confirm.

**Final output structure:**

1. **Auto-write identity to Global CLAUDE.md** (step 41 — automatic)
2. **Personalization Complete summary**

**Personalization Complete summary format:**
```
You're all set, [name].

Here's what I've got:
- You: [AE_NAME], [AE_TITLE] at [COMPANY]
- What you sell: [PRIMARY_PRODUCT] — [one-line description]
- Your territory: [TERRITORY_TYPE]
- Competitors I know: [TOP_COMPETITOR_1], [TOP_COMPETITOR_2], [TOP_COMPETITOR_3]
- Your data lives in: your SalesSidekick workspace
- Connectors: Gmail [✅/❌] | Calendar [✅/❌] | Drive [✅/❌]

What's next? You can:
- Share your top 3 active deals (paste a screenshot or just name them)
- Drop in a recent call transcript to see what I do with it
- Ask me to prep you for an upcoming meeting
- Or just tell me what you're working on

I'll take it from there.
```

## Data Read/Write

**Writes:**
- Updates Global CLAUDE.md identity block (within markers — auto-write)
- Writes company intel to `knowledge-bases/company-intel.md`
- Writes battlecards to `knowledge-bases/battlecards.md`
- Updates signal thresholds in Project CLAUDE.md


**Reads:**
- Global CLAUDE.md for existing identity (to skip redundant questions)
- Local workspace files for existing data
- Web search for company intel and competitor research

## Commandment Alignment

| Commandment | How Deep Personalization Serves It |
|-------------|-----------------------------------|
| #1 Speed is Life | 15 minutes, not 45 — skips what's already known from organic use |
| #2 Stop Digging, Start Orchestrating | AI researches company and competitors — user validates |
| #4 Context is King | Captures explicit-only variables that organic use can't infer |
| #7 One Source of Truth | Writes identity to Global CLAUDE.md, knowledge to workspace files |
| #8 Laws of Karma | Mandatory audit at end catches fabrication in generated content |
| #10 Be the Chief of Staff | After deep personalization, system operates at maximum intelligence |

## Evidence Grading

During Phase 2 (Company Intelligence) and Phase 3 (Competitive Landscape), all generated content must be evidence-graded:
- Company data from official sources → Verified
- Calculated metrics or market estimates → Estimated
- Industry pattern assumptions → Hypothesis

The audit in Phase 7 checks evidence grading compliance.

## Graceful Degradation

| Missing Connector | Impact on Deep Personalization |
|-------------------|-------------------------------|
| No Gmail | Skips Gmail verification. {{GMAIL_CONNECTED}} set to false. Capabilities will generate copy-paste email text. |
| No Calendar | Skips Calendar verification. {{CALENDAR_CONNECTED}} set to false. Morning briefing will ask about meetings. |
| No Drive | Skips Drive verification. {{DRIVE_CONNECTED}} set to false. Call processing will ask for transcript paste. |
| No Gamma | Skips Gamma. {{GAMMA_CONNECTED}} set to false. Presentations use native .pptx path. |
| No web search | Company intel and battlecard generation degrade to user-provided information only. Skills are created but thinner. |

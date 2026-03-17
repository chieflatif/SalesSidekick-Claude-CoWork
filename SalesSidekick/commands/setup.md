---
description: Optional deep personalization — sharpen SalesSidekick beyond what organic use captures
argument-hint: "(optional: phase name — 'battlecards', 'brand voice', 'connectors', etc.)"
intent-triggers:
  - intent: customize-system
    phrases:
      - "let's personalize"
      - "configure my settings"
      - "deep setup"
      - "set me up"
      - "let's go deep"
      - "full personalization"
      - "set everything up"
---

# Deep Personalization Session

## Purpose

An optional deep personalization session that captures context the system can't easily learn organically — competitive battlecards from structured research, calibrated brand voice from writing samples, explicit territory numbers, and bulk database creation.

**This is not required.** SalesSidekick works from the first conversation (see CLAUDE.md Section 14.2). Deep personalization sharpens output for users who want to go all-in. Most users reach this naturally after a few sessions, when the system nudges them (see Section 14.5).

## When to Use

- When the system suggests it ("Want to spend 15 minutes sharpening things up?")
- When the user explicitly asks to personalize or calibrate
- To build structured competitive battlecards (vs. one-off competitor notes)
- To calibrate brand voice from writing samples (vs. gradually learning from edits)
- To capture explicit-only variables (quota, fiscal year, deal stages) that can't be inferred
- To set up connectors or create databases in bulk
- After changing companies, products, or territory

**Not needed for:**
- First-time use (the getting-started conversation in Section 14.2 handles that)
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

1. Check existing Tier 2 variables against CLAUDE.md Section 13
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
5. Write all captured values to the Notion Config page (`SalesSidekick — Config`). Create it if it doesn't exist. **Do not attempt to write to CLAUDE.md — plugin files are read-only in Cowork.**
6. Confirm: "Identity locked in. [summary of what was captured]."

### Phase 2: Company Intelligence (~5-10 min, AI-assisted)

**Skip if:** `skills/company-intel/SKILL.md` has already been regenerated with rich content.

6. Using {{COMPANY_URL}} and {{PRODUCT_DESCRIPTION}}, research the company via web search
7. Generate draft `skills/company-intel/SKILL.md` with: company overview, product portfolio, market position, key differentiators, pricing context, case studies by vertical, key metrics
8. Present the draft to the user for review and refinement
9. Finalize and save the company-intel skill file. Note: if skill files are read-only (Cowork mode), present the finalized content and instruct the user to paste it into `skills/company-intel/SKILL.md` manually, or store key facts in the Notion Config page under a "Company Intel" section.

**Value beyond organic capture:** Structured, comprehensive company profile vs. fragments gathered across individual interactions.

### Phase 3: Competitive Landscape (~5-10 min, AI-assisted)

**Skip if:** Battlecards already exist for 3+ competitors.

10. Check existing competitor variables — use competitors already captured organically
11. Ask if there are additional competitors to add (up to 5 total)
12. For each competitor, research via web search
13. Generate draft `skills/battlecards/SKILL.md` with per-competitor sections: overview, strengths, weaknesses, displacement strategy, talk tracks, proof points, trap questions, landmine responses
14. Present battlecards to user for validation — competitors know things AI doesn't
15. Finalize and save the battlecards skill file
16. Populate any missing {{TOP_COMPETITOR_N}} variables

**Value beyond organic capture:** Systematic displacement playbooks for each competitor vs. ad-hoc competitive notes from individual calls.

### Phase 4: Brand Voice (~3-5 min)

**Skip if:** `skills/brand-voice/SKILL.md` has been calibrated from writing samples.

17. Present voice dimension options: formal↔casual, data-driven↔narrative, concise↔detailed
18. Optionally ask user to paste 2-3 sample emails for voice extraction
19. Generate `skills/brand-voice/SKILL.md` with: vocabulary rules, email formatting preferences, banned phrases, 7-point voice check
20. Populate {{COMMUNICATION_STYLE}}, {{EMAIL_SIGN_OFF}}
21. If not yet captured: ask about LinkedIn posting goals (topics, target audience)
22. Populate {{LINKEDIN_TOPICS}}, {{LINKEDIN_AUDIENCE}}

**Value beyond organic capture:** Calibrated from actual writing samples vs. gradually learned from corrections over 5-10 emails.

### Phase 5: Presentation Brand (~3-5 min)

**Skip if:** Brand tokens already exist in `skills/pptx/SKILL.md`.

23. Ask user for a screenshot of existing presentation or marketing material, OR company website URL
24. Extract brand tokens: primary colors, font families, layout patterns
25. Store brand tokens in `skills/pptx/SKILL.md` Brand Configuration section
26. Generate one sample slide for user verification
27. Adjust tokens based on feedback

**Value beyond organic capture:** Explicit brand token extraction vs. best-effort from company website.

### Phase 6: Connectors & Databases (~3-5 min)

**Skip if:** All 6 databases exist and connectors are verified.

28. Check which of the 6 Notion databases exist (some may have been created on-demand)
29. For any missing databases, create with exact schemas (Companies 12 fields, Contacts 9, Deals 22, Tasks 9, Call Notes 10, LinkedIn Posts 8)
30. Store all database IDs in the Notion Config page (`SalesSidekick — Config`). **Do not attempt to write to CLAUDE.md — plugin files are read-only in Cowork.**
31. Auto-detect available connectors (Gmail, Calendar, Drive, Gamma)
32. For each detected connector, verify access and record status
33. Set connector status variables: {{NOTION_CONNECTED}}, {{GMAIL_CONNECTED}}, {{CALENDAR_CONNECTED}}, {{DRIVE_CONNECTED}}, {{GAMMA_CONNECTED}}
34. Confirm graceful degradation rules for any missing connectors

**Value beyond organic capture:** Bulk database creation and connector verification vs. one-at-a-time on-demand creation.

### Phase 7: Verification (~2 min)

35. Run smoke test: create a test company record, write and read back, then delete
36. Regenerate `skills/profile/SKILL.md` with full AE identity and context
37. Run `/audit` on the configuration
38. Fix any issues found during audit
39. Produce "Personalization Complete" confirmation summary

## Output Format

**Per phase:** Conversational progress updates. User confirms before advancing to the next phase.

**If running a single phase:** Just run that phase and confirm.

**Final output — Personalization Complete confirmation:**
```
✅ Deep Personalization Complete for {{AE_NAME}}

Identity: {{AE_NAME}}, {{AE_TITLE}} at {{COMPANY}}
Territory: {{TERRITORY_SIZE}} accounts ({{TERRITORY_TYPE}})
Product: {{PRIMARY_PRODUCT}}
CRM: {{CRM_SYSTEM}}

Databases: [N] active (Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts)
Skills calibrated: profile, brand-voice, company-intel, battlecards
Competitors mapped: {{TOP_COMPETITOR_1}}, {{TOP_COMPETITOR_2}}, {{TOP_COMPETITOR_3}}

Connectors:
- Notion: ✅
- Gmail: [✅/❌]
- Calendar: [✅/❌]
- Drive: [✅/❌]
- Gamma: [✅/❌]

Personalization state: CALIBRATED
Estimated time saved: [X] variables and [Y] databases were already in place from organic use.
```

## Database Read/Write

**Writes:**
- Creates Notion databases (only those not already created on-demand)
- Writes database IDs to CLAUDE.md Section 10
- Writes all captured Tier 2 variables to CLAUDE.md
- Regenerates Tier 3 skill files (profile, brand-voice, company-intel, battlecards)

**Reads:**
- All existing Tier 2 variables (to skip redundant questions)
- Existing Notion databases (to skip creation)
- Web search for company intel and competitor research
- Existing CLAUDE.md for current configuration state

## Commandment Alignment

| Commandment | How Deep Personalization Serves It |
|-------------|-----------------------------------|
| #1 Speed is Life | 15 minutes, not 45 — skips what's already known from organic use |
| #2 Stop Digging, Start Orchestrating | AI researches company and competitors — user validates |
| #4 Context is King | Captures explicit-only variables that organic use can't infer |
| #7 One Source of Truth | Creates/completes the Notion backbone that all capabilities use |
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
| No Notion | Cannot create databases. Session completes identity capture and skill generation only. Offer to connect Notion and resume later. |
| No Gmail | Skips Gmail verification. {{GMAIL_CONNECTED}} set to false. Capabilities will generate copy-paste email text. |
| No Calendar | Skips Calendar verification. {{CALENDAR_CONNECTED}} set to false. Morning briefing will ask about meetings. |
| No Drive | Skips Drive verification. {{DRIVE_CONNECTED}} set to false. Call processing will ask for transcript paste. |
| No Gamma | Skips Gamma. {{GAMMA_CONNECTED}} set to false. Presentations use native .pptx path. |
| No web search | Company intel and battlecard generation degrade to user-provided information only. Skills are created but thinner. |

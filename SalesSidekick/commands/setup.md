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

1. Check the Notion Config page (`SalesSidekick — Config`) for already-populated variables. CLAUDE.md Section 13 documents the full list of variables — use it as a reference, not as the state source. CLAUDE.md template variables are read-only defaults and always show placeholder text; only the Notion Config page shows real values.
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

7. Using {{COMPANY_URL}} and {{PRODUCT_DESCRIPTION}}, research the company via web search
8. Generate draft `skills/company-intel/SKILL.md` with: company overview, product portfolio, market position, key differentiators, pricing context, case studies by vertical, key metrics
9. Present the draft to the user for review and refinement
10. Finalize and save the company-intel skill file. Note: if skill files are read-only (Cowork mode), present the finalized content and instruct the user to paste it into `skills/company-intel/SKILL.md` manually, or store key facts in the Notion Config page under a "Company Intel" section.

**Value beyond organic capture:** Structured, comprehensive company profile vs. fragments gathered across individual interactions.

### Phase 3: Competitive Landscape (~5-10 min, AI-assisted)

**Skip if:** Battlecards already exist for 3+ competitors.

11. Check existing competitor variables — use competitors already captured organically
12. Ask if there are additional competitors to add (up to 5 total)
13. For each competitor, research via web search
14. Generate draft `skills/battlecards/SKILL.md` with per-competitor sections: overview, strengths, weaknesses, displacement strategy, talk tracks, proof points, trap questions, landmine responses
15. Present battlecards to user for validation — competitors know things AI doesn't
16. Finalize and save the battlecards skill file
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

### Phase 6: Connectors & Databases (~3-5 min)

**Skip if:** All 6 databases exist (verified by Notion search) and connectors are verified.

29. **Search Notion for each database by name** — use the Notion search API to search for: "Companies", "Contacts", "Deals", "Tasks", "Call Notes", "LinkedIn Posts". Do this regardless of whether a Config page exists. **Absence of a Config page does NOT mean databases don't exist** — the user may have databases from a previous session, a prior install, or a partial setup. Never assume FRESH = no databases.
   - For each database found by name: confirm with the user — "I found a database called [name] in your Notion — is that your SalesSidekick one, or a different one?" Record its ID only after the user confirms it's the right one. Never silently adopt databases the user doesn't confirm.
   - If the user says "that's not mine" or "that's a different one": skip that match and create a new database with a distinct name, e.g., "SalesSidekick — Companies".
   - If the user says "I'm not sure" or "it might be": say "No problem — I'll create a fresh one to be safe. You can always delete the old one from Notion later." Then create.
   - For databases genuinely not found after searching: create them with exact schemas (see skills/notion/SKILL.md — Companies 12 fields, Contacts 9, Deals 22, Tasks 9, Call Notes 10, LinkedIn Posts 8).
30. Create any missing databases in dependency order: Companies → Contacts → Deals → Tasks → Call Notes → LinkedIn Posts.
31. Wire cross-relations after all databases exist (Companies↔Contacts, Deals→Companies, Deals→Contacts, Tasks→Companies, Tasks→Deals, Call Notes→Companies, Call Notes→Deals, LinkedIn Posts→Companies).
32. Store all 6 database IDs in the Notion Config page (`SalesSidekick — Config`). **Do not attempt to write to CLAUDE.md — plugin files are read-only in Cowork.**
33. Auto-detect available connectors (Gmail, Calendar, Drive, Gamma)
34. For each detected connector, verify access and record status
35. Set connector status variables: {{NOTION_CONNECTED}}, {{GMAIL_CONNECTED}}, {{CALENDAR_CONNECTED}}, {{DRIVE_CONNECTED}}, {{GAMMA_CONNECTED}} — write to Notion Config page
36. Confirm graceful degradation rules for any missing connectors

**Value beyond organic capture:** Bulk database creation and connector verification vs. one-at-a-time on-demand creation.

### Phase 7: Verification (~2 min)

37. Run smoke test: create a test company record, write and read back, then delete
38. Regenerate `skills/profile/SKILL.md` with full AE identity and context (note: if skill files are read-only in Cowork, write full profile content to a Notion page titled "SalesSidekick — Profile" instead)
39. Run the self-audit capability on the configuration
40. Fix any issues found during audit

---

**⚠️ STOP — Step 41 is mandatory. Do not produce the completion summary until this step is done and the user has acknowledged it.**

---

41. **Generate personalized global settings block — REQUIRED, cannot be skipped.**

This is the most important output of the entire deep personalization session. Without it, every future session restarts without knowing who the user is — no identity, no competitive context, no communication style. The global settings field is the 'slow lane' context layer that loads before any plugin fires, before any API call.

Generate the block now using all captured identity, product, ICP, competitive, and communication data. Present it as the FIRST prominent item in your final output — before the completion summary. Label it clearly.

Say:

> "**Your Global Settings Block — paste this before anything else.**
>
> Go to **Claude Desktop > Settings > Cowork** (the custom instructions field) and paste this in now. This is the slow lane — the context that never changes and loads before every session, before any API call, before Notion. It means every future session knows who you are, what you sell, and how to beat your competitors from the very first message. No re-asking. No setup.
>
> Update it only when something fundamental changes: new company, new product, new primary competitors. Everything that refreshes regularly (battlecards, case studies, deal data) lives in Notion."

**Format of the generated block:**
```
I am [AE_NAME], [AE_TITLE] at [COMPANY] ([COMPANY_URL]).

What I sell: [PRODUCT_DESCRIPTION]
Primary product: [PRIMARY_PRODUCT]
I sell to: [ICP_INDUSTRY] companies, [ICP_SIZE], focused on [ICP_USE_CASE]
Territory: [TERRITORY_TYPE]
Communication style: [COMMUNICATION_STYLE]. Sign-off: "[EMAIL_SIGN_OFF]"

Top competitors and how I beat them:
- [TOP_COMPETITOR_1]: [1-line displacement angle from research]
- [TOP_COMPETITOR_2]: [1-line displacement angle from research]
- [TOP_COMPETITOR_3]: [1-line displacement angle from research]

Key differentiation angles:
- Financial: [from research — cost savings, ROI, risk reduction]
- Technical: [from research — integration advantages, architecture, security]
- Strategic: [from research — market position, partnerships, roadmap]

I use voice-to-text — interpret my intent, not my grammar.

QUALITY RULES — apply to everything you produce:

No hallucination. If you don't have a verified source, say "I don't know" or "I'd need to verify." Grade every factual claim: Verified (sourced), Estimated (calculated with stated assumptions), or Hypothesis (pattern-based guess).

No slop. Every sentence must earn its place. Match MY voice, not a corporate template. Before delivering any written content, ask: "Would this person actually write this?"

Do the research first. Before generating content about a company, person, deal, or topic — look it up. Check Notion. Search the web. Notion is my single source of truth for deal and account data.

Give me options, not decisions. Always present 2-3 paths with trade-offs. Never prescribe a single answer.

Respect my time. Lead with the answer, not the reasoning. Break complex work into 10-minute chunks.
```

42. After user confirms they've saved the global settings block (or acknowledges it), produce the Personalization Complete summary.

## Output Format

**Per phase:** Conversational progress updates. User confirms before advancing to the next phase. No command syntax, no technical jargon — speak like a senior colleague, not a setup wizard.

**If running a single phase:** Just run that phase and confirm.

**Final output structure — in this exact order:**

1. **Global settings block first** (step 41 — mandatory, cannot be skipped)
2. **Personalization Complete summary** (only after user acknowledges the global settings block)

**Personalization Complete summary format:**
```
You're all set, [name].

Here's what I've got:
- You: [AE_NAME], [AE_TITLE] at [COMPANY]
- What you sell: [PRIMARY_PRODUCT] — [one-line description]
- Your territory: [TERRITORY_TYPE]
- Competitors I know: [TOP_COMPETITOR_1], [TOP_COMPETITOR_2], [TOP_COMPETITOR_3]
- Your data lives in: 6 Notion databases ([X] existing, [Y] new)
- Connectors: Notion ✅ | Gmail [✅/❌] | Calendar [✅/❌] | Drive [✅/❌] | Gamma [❌]

What's next? You can:
- Share your top 3 active deals (paste a screenshot or just name them)
- Drop in a recent call transcript to see what I do with it
- Ask me to prep you for an upcoming meeting
- Or just tell me what you're working on

I'll take it from there.
```

## Database Read/Write

**Writes:**
- Creates Notion databases (only those not found by name search — never duplicate)
- Writes all database IDs to the Notion Config page (`SalesSidekick — Config`)
- Writes all captured Tier 2 variables to the Notion Config page
- Generates personalized global settings block for user to paste into Cowork global settings
- Writes PLUGIN_VERSION to Notion Config page

**Reads:**
- Notion Config page for existing Tier 2 variables (to skip redundant questions)
- Notion search for each database by name (to skip creation of existing databases)
- Web search for company intel and competitor research

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

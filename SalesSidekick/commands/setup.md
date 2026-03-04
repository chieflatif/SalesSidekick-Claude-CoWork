---
description: First-run personalization wizard — transforms SalesSidekick into YOUR sales AI
argument-hint: "(no arguments — interactive wizard)"
---

# /setup — Personalization Wizard

## Purpose

The most important command in SalesSidekick. Transforms a generic plugin into a fully personalized sales operating system by collecting your context, researching your company and competitors, configuring your databases, and calibrating your communication style.

Run this once. After setup, every command knows who you are, what you sell, who you compete with, and how you communicate.

## When to Use

- First time installing SalesSidekick (mandatory)
- After changing companies, products, or territory
- To add or update connectors
- To regenerate competitive battlecards with new competitors

## Inputs

**User provides (conversational):**
1. Name and title
2. Company name (triggers web research)
3. Product/service description
4. Ideal Customer Profile (industry, size, use case)
5. Top 3-5 competitors (triggers battlecard generation)
6. Communication style preference (casual to enterprise-formal)
7. CRM system (Salesforce, HubSpot, other, none)
8. Territory size and type (account count, named vs geographic vs vertical)
9. Connector availability (auto-detected + confirmed)

**System reads:** Web search results for company intel and competitor research.

## Execution Steps

### Phase 1: Identity Setup (~5 min)
1. Greet the user and explain the setup process (7 phases, ~45 minutes total)
2. Ask Layer 1 questions (items 1-9 above) conversationally — not as a form
3. Write answers to CLAUDE.md Section 1 (identity), Section 9 (operating rhythm), Section 10 (system config)
4. Populate all Tier 2 template variables: {{AE_NAME}}, {{AE_TITLE}}, {{COMPANY}}, {{COMPANY_URL}}, {{PRODUCT_DESCRIPTION}}, {{TERRITORY_SIZE}}, {{TERRITORY_TYPE}}, {{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}}, {{CRM_SYSTEM}}, {{QUOTA_AMOUNT}}, {{FISCAL_YEAR_START}}, {{AVERAGE_DEAL_SIZE}}, {{SALES_CYCLE_LENGTH}}, {{MANAGER_NAME}}, {{TEAM_NAME}}, {{REGION}}, {{PRIMARY_PRODUCT}}, {{SECONDARY_PRODUCTS}}

### Phase 2: Company Intelligence (~10-15 min, AI-assisted)
5. Using the company URL and product description, research the company via web search
6. Generate draft `skills/company-intel/SKILL.md` with: company overview, product portfolio, market position, key differentiators, pricing context, case studies by vertical, key metrics
7. Present the draft to the user for review and refinement
8. Finalize and save the company-intel skill file

### Phase 3: Competitive Landscape (~10-15 min, AI-assisted)
9. For each competitor identified (3-7), research via web search
10. Generate draft `skills/battlecards/SKILL.md` with per-competitor sections: overview, strengths, weaknesses, displacement strategy, talk tracks, proof points, trap questions, landmine responses
11. Present battlecards to user for validation — competitors know things AI doesn't
12. Finalize and save the battlecards skill file
13. Populate {{TOP_COMPETITOR_1}}, {{TOP_COMPETITOR_2}}, {{TOP_COMPETITOR_3}}

### Phase 4: Brand Voice (~5 min)
14. Present voice dimension options: formal↔casual, data-driven↔narrative, concise↔detailed
15. Optionally ask user to paste 2-3 sample emails for voice extraction
16. Generate `skills/brand-voice/SKILL.md` with: vocabulary rules, email formatting preferences, banned phrases, 7-point voice check
17. Populate {{COMMUNICATION_STYLE}}, {{EMAIL_SIGN_OFF}}

### Phase 5: Presentation Brand (~5 min)
18. Ask user for a screenshot of existing presentation or marketing material, OR company website URL
19. Extract brand tokens: primary colors, font families, layout patterns
20. Generate `brand-tokens.json` with hex codes, font specs, logo placement rules
21. Generate one sample slide for user verification
22. Adjust tokens based on feedback

### Phase 6: Connector Setup (~5-10 min)
23. Create 6 Notion databases with exact schemas (Companies 12 fields, Contacts 9, Deals 22, Tasks 9, Call Notes 10, LinkedIn Posts 8)
24. Share databases with the Notion integration
25. Store all 6 database IDs in CLAUDE.md Section 10
26. Auto-detect available connectors (Gmail, Calendar, Drive, Gamma)
27. For each detected connector, verify access and record status
28. Set connector status variables: {{NOTION_CONNECTED}}, {{GMAIL_CONNECTED}}, {{CALENDAR_CONNECTED}}, {{DRIVE_CONNECTED}}, {{GAMMA_CONNECTED}}
29. Confirm graceful degradation rules for any missing connectors

### Phase 7: Verification + Mandatory Audit (~5 min)
30. Run smoke test: create a test company record, write and read back, then delete
31. Regenerate `skills/profile/SKILL.md` with full AE identity and context
32. Run mandatory `/audit` on the entire configuration
33. Fix any issues found during audit
34. Produce "System Ready" confirmation summary showing: identity configured, databases created, connectors active, skills regenerated, all commands available
35. Set {{SETUP_COMPLETE}} to true

## Output Format

**Per phase:** Conversational progress updates with user confirmation before advancing.

**Final output — System Ready confirmation:**
```
✅ SalesSidekick Configured for {{AE_NAME}}

Identity: {{AE_NAME}}, {{AE_TITLE}} at {{COMPANY}}
Territory: {{TERRITORY_SIZE}} accounts ({{TERRITORY_TYPE}})
Product: {{PRIMARY_PRODUCT}}
CRM: {{CRM_SYSTEM}}

Databases: 6 created (Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts)
Skills regenerated: profile, brand-voice, company-intel, battlecards
Competitors mapped: {{TOP_COMPETITOR_1}}, {{TOP_COMPETITOR_2}}, {{TOP_COMPETITOR_3}}

Connectors:
- Notion: ✅
- Gmail: [✅/❌]
- Calendar: [✅/❌]
- Drive: [✅/❌]
- Gamma: [✅/❌]

Audit: Passed
Status: Ready

Try /today for your morning briefing, or /prep [Company] before your next meeting.
```

## Database Read/Write

**Writes:**
- Creates all 6 Notion databases (Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts)
- Writes database IDs to CLAUDE.md Section 10
- Writes all Tier 2 variables to CLAUDE.md
- Regenerates 4 Tier 3 skill files (profile, brand-voice, company-intel, battlecards)

**Reads:**
- Web search for company intel and competitor research
- Existing CLAUDE.md for current configuration state

## Commandment Alignment

| Commandment | How /setup Serves It |
|-------------|---------------------|
| #2 Stop Digging, Start Orchestrating | AI researches company and competitors — user validates |
| #4 Context is King | Ensures every future command has full context loaded |
| #7 One Source of Truth | Creates the Notion backbone that all commands use |
| #8 Laws of Karma | Mandatory audit at end catches fabrication in generated content |
| #10 Be the Chief of Staff | After setup, AE never needs to configure anything again |

## Evidence Grading

During Phase 2 (Company Intelligence) and Phase 3 (Competitive Landscape), all generated content must be evidence-graded:
- Company data from official sources → Verified
- Calculated metrics or market estimates → Estimated
- Industry pattern assumptions → Hypothesis

The mandatory audit in Phase 7 checks evidence grading compliance.

## Graceful Degradation

| Missing Connector | Impact on /setup |
|-------------------|-----------------|
| No Notion | **Critical.** Cannot create databases. Setup completes identity and skill generation only. Prompt user to configure Notion and re-run /setup. |
| No Gmail | Setup skips Gmail verification. {{GMAIL_CONNECTED}} set to false. Commands will generate copy-paste email text. |
| No Calendar | Setup skips Calendar verification. {{CALENDAR_CONNECTED}} set to false. /today and /prep will ask about meetings. |
| No Drive | Setup skips Drive verification. {{DRIVE_CONNECTED}} set to false. /closeout will ask for transcript paste. |
| No Gamma | Setup skips Gamma. {{GAMMA_CONNECTED}} set to false. /deck uses native .pptx path. |
| No web search | Company intel and battlecard generation degrade to user-provided information only. Skills are created but thinner. |

# SalesSidekick — Validated Build Specification
## Reconciled from Product Bible v2.0 + Dev Plan + Owner Decisions

**Date:** 2026-03-03
**Status:** CANONICAL — This document supersedes both the Product Bible and Dev Plan where they conflict.

---

## RESOLVED DECISIONS

| # | Conflict | Resolution |
|---|----------|------------|
| 1 | CLAUDE.md section count (Bible=13, Plan=8) | **Bible's 13 sections + 3 from dev plan = 16 sections total** |
| 2 | /Q command definition conflict | **Renamed to /email** — quick contextual email, not free-form query. Full spec below. |
| 3 | Command file format differs | **Bible base + dev plan extras** — 9 sections total per command |
| 4 | Skill file format differs | **Unified** — YAML frontmatter + 6 sections |
| 5 | `mapbox-intel` skill name in Bible | **company-intel** — Bible remnant, dev plan is correct |
| 6 | `specs/` directory in Bible | **SKIP** — undefined content, no commands reference it |
| 7 | Repo strategy | **Separate repos** — this workspace is dev/planning, clean plugin pushes to github |
| 8 | 34 template variables (plan) vs 104 personalization elements (Bible) | **Bible's 104 governs scope**, plan's 34 are the Tier 2 variables subset |
| 9 | /end-of-day write behavior | **Read-only** — Bible explicitly states no Notion writes |

---

## REPO STRATEGY

**Development workspace:** `/Users/latifhorst/Cowork-Sidekick/`
- Contains: Dev plan, Product Bible, validated build spec, `.claude/` governance
- NOT pushed to GitHub
- This is our workshop

**Clean plugin repo:** `github.com/chieflatif/SalesSidekick-Claude-CoWork`
- Contains: ONLY the plugin files (CLAUDE.md, commands/, skills/, etc.)
- Zero planning artifacts, zero governance files, zero dev tooling
- What users download = what they install

**Build process:**
1. Build all plugin files in this workspace under a `SalesSidekick/` subdirectory
2. Each phase: build files → validate → audit → commit to THIS workspace
3. When ready to publish: push the `SalesSidekick/` contents to the clean GitHub repo

---

## CANONICAL FILE FORMATS

### Command File Format (9 sections)

```markdown
---
description: One-line description shown in command list
argument-hint: "[Company] or [topic]"  # if the command takes arguments
---

# /command-name — Display Title

## Purpose
What this command does and why it matters.

## When to Use
Trigger conditions and use cases.

## Inputs
What the user provides (required vs optional) and what the system reads from Notion.

## Execution Steps
Numbered step-by-step logic. Specific, not vague.

## Output Format
Exact templates for what the user receives.

## Database Read/Write
Which Notion databases are read and written, with specific field mappings.

## Commandment Alignment
Which of the 10 Commandments this command serves and how.

## Evidence Grading
How evidence grading applies to this command's output. (Write "N/A — this command does not generate quantitative claims" if not applicable.)

## Graceful Degradation
What changes when specific connectors are missing. Must cover: No Notion, No Calendar, No Gmail, No Drive (whichever are relevant).
```

### Skill File Format (6 sections)

```markdown
---
name: skill-name
description: One-line description (triggers auto-loading)
---

# SKILL: [Display Name]

## Auto-Fire Trigger
When this skill activates (which commands, which topics).

## Purpose
What knowledge this skill provides.

## When Referenced
Which commands use this skill and how.

## Core Framework
The actual methodology, rubric, or framework content. This is the bulk of the file.

## Personalization Notes
Tier designation (1/2/3). What changes during /setup. What stays universal.
```

### CLAUDE.md Structure (16 sections)

| Section | Content | Personalization Tier |
|---------|---------|---------------------|
| 1. You Are | Role definition, identity statement | Tier 2 (name, company, title) |
| 2. The 10 Commandments | Operating principles with enforcement paragraph | Tier 1 (universal) |
| 3. The Flashlight Philosophy | Illuminate Don't Dictate, Three Paths, confirmation protocol | Tier 1 (universal) |
| 4. Qualification: Hell Yes | Ideal customer profile signals | Tier 2/3 (industry, signals) |
| 5. Qualification: Hell No | Disqualification criteria | Tier 2/3 (industry, signals) |
| 6. Voice and Style | Communication rules, voice-to-text protocol | Tier 3 (regenerated per user) |
| 7. The Context Wedge Script | 30-second pitch framework | Tier 3 (company-specific) |
| 8. Discovery Methodology | Question frameworks, probing patterns | Tier 2 (product references) |
| 9. Operating Rhythm | Territory context, account counts, key accounts | Tier 2 (account counts, names) |
| 10. System Configuration | Database IDs, connector status, degradation rules | Tier 2 (automated by /setup) |
| 11. Evidence Grading | Three grades, 50% Rule, language patterns | Tier 1 (universal) |
| 12. Voice-to-Text Protocol | Input interpretation rules, mistranscription handling | Tier 1 (universal) |
| 13. Personalization Variables | Full variable inventory, what's set during /setup vs first-use | Tier 1 (reference) |
| 14. Command Index | All 22 commands grouped by category | Tier 1 (universal) |
| 15. Skill Index | All 11 skills with auto-fire descriptions | Tier 1 (universal) |
| 16. System Notes | Version, update history | Tier 1 (universal) |

---

## COMPLETE COMMAND INVENTORY (22 Commands)

### Naming Changes from Dev Plan
- `/Q` → **`/email`** (renamed, file: `commands/email.md`)
- All other names unchanged

### Command Registry by Category

**Daily Ops (3)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /today | today.md | — | Morning briefing. Account-grouped tasks (due today + overdue) + Deals Needing Attention (overdue activities, approaching close dates, high-risk signals, stale accounts). |
| /end-of-day | end-of-day.md | /eod | Evening wrap. Reviews completed tasks, flags overdue, previews tomorrow. **READ-ONLY — does NOT write to Notion.** |
| /weekly | weekly.md | — | **(NEW)** Weekly pipeline summary. Territory-wide digest, deal movement tracking, MEDDPICC score changes, shareable format for manager 1:1s. |

**Call Processing (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /closeout | closeout.md | /call | Flagship command. 6-Output Framework: MEDDPICC scoring, task extraction, coaching feedback (5 dimensions scored 1-5), follow-up email (3 tone options), risk signals (6 categories H/M/L), competitive intel. Writes to 4 Notion databases. |

**Meeting Prep (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /prep | prep.md | — | Pre-meeting intelligence brief. Reads company + contacts + deals + recent calls + open tasks. Structured prep with talking points and MEDDPICC gaps to probe. |

**Deal Strategy (3)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /strategy | strategy.md | — | Five-Lens Prism (Stakeholder Psychology, Political Capital, Competitive Dynamics, Hidden Leverage, Temporal Dynamics). Three Paths output (Velocity/Diagnostic/Protective). Evidence-graded. |
| /battle | battle.md | — | Competitive displacement. Loads battlecard, assesses win probability (0.0-1.0), recommends displacement tactics with talk tracks and proof points. |
| /pov | pov.md | — | Point of View document. 5-Component Model (Executive Anchor, Blind Spot, Math of Pain, Mechanism, Call to Action). All claims evidence-graded. Most hypothesis-prone command. |

**Territory Management (3)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /pipeline | pipeline.md | — | Full territory view. Scores deals by MEDDPICC confidence, flags at-risk, identifies stalled. Prioritized action plan. |
| /whitespace | whitespace.md | — | Territory expansion analysis. Product gaps, upsell opportunities, underserved segments. Ranked opportunity list. |
| /forecast-update | forecast-update.md | — | **(NEW)** CRM-ready forecast. Weighted pipeline math (stage prob × deal value). Commit/upside/best-case. Salesforce or HubSpot paste-ready format per {{CRM_SYSTEM}}. Evidence-graded. |

**Content Creation (3)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /deck | deck.md | — | Presentation generation. 5 templates (Discovery, POV, Executive Summary, QBR, Competitive Displacement), 8 slides each. Auto-selects by deal stage. Native .pptx (PptxGenJS) or Gamma. Brand tokens from /setup. |
| /outreach | outreach.md | — | Prospecting email. Reads account intel, identifies angle (Financial/Technical/Strategic). Sub-200 words. Never reaches out naked (Commandment 4). |
| /draft-post | draft-post.md | — | LinkedIn post. 3-Type Framework (Personal story→lesson, Business insight→proof, Educational framework→takeaway). Brand voice + 6-point pre-publish checklist. |

**Communication (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /email | email.md | — | Quick contextual email for existing relationships. Pulls company/contact/deal/call context from Notion. **No puffing** — only reference details relevant to the stated topic. Confidence check before drafting (flags missing context honestly). Short, natural, reads like AE wrote it in 90 seconds. |

**Intelligence (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /research | research.md | — | **(NEW)** Deep account research. Web search + available connectors. Structured brief: overview, news, key personnel, tech stack, competitive landscape, selling angles. Evidence-graded throughout. Works with web search alone. |

**Data Management (2)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /add-company | add-company.md | — | Guided company record creation. Sequential conversational intake. Applies Hell Yes / Hell No qualification before committing. |
| /add-deal | add-deal.md | — | Guided deal record creation. Links to company + contacts. Initializes all 8 MEDDPICC scores to Red. |

**Performance (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /coaching | coaching.md | — | Call pattern analysis across logged calls. 5 dimensions (Discovery, Objection Handling, Rapport, Next Steps, Talk Ratio) scored 1-5. |

**Quality (1)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /audit | audit.md | — | **(NEW)** 7-question self-critique battery. Three trigger modes: manual, suggested (after major outputs), mandatory (during /setup). Evidence grading integration: checks 50% Rule, re-grading recommendations. |

**System (2)**
| Command | File | Aliases | Key Behavior |
|---------|------|---------|-------------|
| /setup | setup.md | — | Personalization wizard. 7 phases: Identity → Company Intel → Competitive Landscape → Brand Voice → Presentation Brand → Connector Setup → Verification + Mandatory Audit. |
| /skill-builder | skill-builder.md | — | **(NEW)** 5-phase command creation: Intent → Commandment Check → Scaffold → Audit → Install. |

---

## COMPLETE SKILL INVENTORY (11 Skills)

| Skill | Directory | Tier | Key Content |
|-------|-----------|------|-------------|
| Profile | skills/profile/ | 3 (regenerated) | AE identity, cognitive profile, territory context, qualification gates, operating rhythm. Fully rewritten during /setup. |
| Brand Voice | skills/brand-voice/ | 3 (regenerated) | Voice rules, email formatting (lowercase subjects, <200 words), vocabulary substitutions (10+), banned phrases, 7-point voice check. Regenerated from user preferences/samples. |
| Company Intel | skills/company-intel/ | 3 (regenerated) | Company overview, product portfolio, competitive positioning, pricing, case studies by vertical, key metrics. Regenerated via web research during /setup. |
| Battlecards | skills/battlecards/ | 3 (regenerated) | Per-competitor: overview, strengths, weaknesses, displacement strategy, talk tracks, proof points, trap questions, landmine responses. Regenerated during /setup. |
| MEDDPICC | skills/meddpicc/ | 1 (universal) | 8-element scoring rubric. Each element: R/Y/G definitions, discovery questions, gap flags. Per Bible Section 7.2. |
| Deal Strategy | skills/deal-strategy/ | 1 (universal) | Five-Lens Prism (5 lenses with analysis prompts), Three Paths Framework, 5-Component POV Model, evidence grading reference. |
| Call Processing | skills/call-processing/ | 1 (universal) | 6-Output Framework with detailed formats. 5 coaching dimensions (scored 1-5). 6 risk signal categories (H/M/L). Competitive intel format. |
| Notion | skills/notion/ | 1 (universal) | 6 database schemas (70+ fields total per Bible Section 5.4.1). Read/write patterns per command. Account resolution logic. Write failure handling. |
| PPTX | skills/pptx/ | 1 (universal) | PptxGenJS pipeline. Brand token integration. 5 deck templates × 8 slides each. Visual QA pipeline (LibreOffice → PDF → JPEG). |
| Gamma | skills/gamma/ | 1 (universal) | Alternative presentation path. When to use vs PPTX. Prompt templates. Setup requirements. Limitations. |
| Posting Guide | skills/posting-guide/ | 2 (template) | 3-Type Framework (Personal/Business/Educational). Structure templates. Hook formulas. Frequency goals (4-5/week). 6-point pre-publish checklist. Template variables for {{LINKEDIN_TOPICS}}, {{LINKEDIN_AUDIENCE}}. |

---

## NOTION DATABASE SCHEMAS (6 Databases, 70+ Fields)

Per Bible Section 5.4.1, with Select option defaults defined below. Select options marked with (default) are pre-populated defaults; /setup may customize these per user.

### Companies (12 fields)
1. Company Name (Title)
2. Territory Status (Select: Active, Prospect, Inactive, Former Customer)
3. Industry (Select — options populated during /setup per user's market)
4. Account Size (Select: Enterprise, Mid-Market, SMB)
5. Hell Yes/Hell No (Select: Hell Yes, Evaluating, Hell No)
6. Primary Contact (Relation→Contacts)
7. Tech Stack (Multi-select — populated per account)
8. Current Products (Multi-select — user's product line, set during /setup)
9. Annual Revenue (Number)
10. Last Activity (Date)
11. Notes (Text)
12. Website (URL)

### Contacts (9 fields)
1. Name (Title)
2. Company (Relation→Companies)
3. Title (Text)
4. MEDDPICC Role (Multi-select: Champion, Economic Buyer, Technical Buyer, Influencer, Blocker, User)
5. Email (Email)
6. LinkedIn (URL)
7. Sentiment (Select: Champion, Supportive, Neutral, Skeptical, Blocker)
8. Last Contacted (Date)
9. Notes (Text)

### Deals (22 fields)
1. Deal Name (Title)
2. Company (Relation→Companies)
3. Contacts (Relation→Contacts) — linked during /add-deal
4. Stage (Select: Prospecting, Discovery, Qualification, Proposal, Negotiation, Closed Won, Closed Lost) — 7 stages with default probability weights below
5. Deal Value (Number)
6. Close Date (Date)
7. Forecast Category (Select: Commit, Best Case, Pipeline, Omitted)
8. Primary Competitor (Select — options populated during /setup Phase 3)
9. Next Step (Text)
10. Deal Risk (Select: High, Medium, Low)
11. MEDDPICC Confidence (Select: High, Medium, Low)
12. M-Metrics (Select: Red, Yellow, Green)
13. E-Economic Buyer (Select: Red, Yellow, Green)
14. D-Decision Criteria (Select: Red, Yellow, Green)
15. D-Decision Process (Select: Red, Yellow, Green)
16. P-Paper Process (Select: Red, Yellow, Green)
17. I-Identify Pain (Select: Red, Yellow, Green)
18. C-Champion (Select: Red, Yellow, Green)
19. C-Competition (Select: Red, Yellow, Green)
20. Created Date (Date)
21. Last Activity (Date)
22. Notes (Text)

**Default Stage Probability Weights** (used by /forecast-update):
| Stage | Default Probability |
|-------|-------------------|
| Prospecting | 10% |
| Discovery | 20% |
| Qualification | 40% |
| Proposal | 60% |
| Negotiation | 80% |
| Closed Won | 100% |
| Closed Lost | 0% |

Note: These are defaults. /setup should ask if the user's org uses different stage names or probabilities, and store customized values in {{DEAL_STAGES}} and the notion/SKILL.md.

### Tasks (9 fields)
1. Task (Title)
2. Company (Relation→Companies)
3. Deal (Relation→Deals)
4. Owner (Select: AE, Prospect, Internal)
5. Priority (Select: High, Medium, Low)
6. Due Date (Date)
7. Status (Select: Not Started, In Progress, Done, Blocked)
8. Source (Select: Call, Strategy, Manual)
9. Notes (Text)

Note: Bible header says "8 fields" but enumerates 9 including Notes. Actual count is 9.

### Call Notes (10 fields)
1. Call Title (Title)
2. Company (Relation→Companies)
3. Deal (Relation→Deals)
4. Call Date (Date)
5. Summary (Text)
6. Action Items (Text)
7. Risk Level (Select: High, Medium, Low)
8. Competitive Intel (Text)
9. Coaching Score (Number: 1-5)
10. MEDDPICC Changes (Text)

### LinkedIn Posts (8 fields)
1. Post Title (Title)
2. Post Type (Select: Personal, Business, Educational)
3. Status (Select: Idea, Draft, Ready, Scheduled, Posted)
4. Scheduled/Posted Date (Date)
5. Hook (Text)
6. Content (Text)
7. Related Company (Relation→Companies)
8. Notes (Text)

Note: Bible header says "9 fields" but only 8 are defined anywhere. Corrected to 8. Total across all databases: 12 + 9 + 22 + 9 + 10 + 8 = **70 fields**.

---

## BUILD PHASES — VALIDATED

All files are built inside `SalesSidekick/` subdirectory of this workspace.

### Phase 1: Scaffold & Core Identity (5 files)

| File | Path | Notes |
|------|------|-------|
| plugin.json | .claude-plugin/plugin.json | Name, version 2.0.0, description, author |
| CLAUDE.md | CLAUDE.md | 16 sections. Template variables for all personalizable content. |
| .mcp.json | .mcp.json | Notion MCP config with {{NOTION_API_KEY}} placeholder |
| CONNECTORS.md | CONNECTORS.md | How MCP works, supported connectors, setup steps, graceful degradation |
| .gitignore | .gitignore | .env, node_modules/, .DS_Store, *.log |

**Phase gate:** CLAUDE.md has all 16 sections with real content. Zero hardcoded personal references. All template variables use {{DOUBLE_CURLY_BRACES}}.

### Phase 2: The 22 Commands (22 files)

Build in 6 batches per dependency order:

**Batch 1 — Foundation (4 files):**
setup.md, audit.md, today.md, end-of-day.md

**Batch 2 — Core Sales Workflow (5 files):**
closeout.md, prep.md, strategy.md, battle.md, pov.md

**Batch 3 — Intelligence & Territory (5 files):**
pipeline.md, whitespace.md, research.md, weekly.md, forecast-update.md

**Batch 4 — Communication & Content (4 files):**
outreach.md, email.md, draft-post.md, deck.md

**Batch 5 — Account Management (3 files):**
add-company.md, add-deal.md, coaching.md

**Batch 6 — System (1 file):**
skill-builder.md

**Phase gate:** All 22 files exist, follow the 9-section format, have valid YAML frontmatter, cross-reference real skills, specify graceful degradation, align with Commandments.

### Phase 3: The 11 Skills (11 files)

| Build Order | Skill | Why This Order |
|-------------|-------|----------------|
| 1 | notion/ | Database schemas — everything references this |
| 2 | meddpicc/ | Scoring rubrics — /closeout, /strategy, /prep reference this |
| 3 | call-processing/ | 6-Output Framework — /closeout depends on this |
| 4 | deal-strategy/ | Five-Lens Prism, Three Paths, POV Model — /strategy, /pov depend on this |
| 5 | posting-guide/ | 3-Type Framework — /draft-post depends on this |
| 6 | pptx/ | Presentation pipeline — /deck depends on this |
| 7 | gamma/ | Alternative path — /deck references this |
| 8 | profile/ | AE identity template — Tier 3, needs template structure |
| 9 | brand-voice/ | Voice rules template — Tier 3, needs template structure |
| 10 | company-intel/ | Company intel template — Tier 3, needs template structure |
| 11 | battlecards/ | Competitive template — Tier 3, needs template structure |

**Phase gate:** All 11 SKILL.md files exist, follow the 6-section format, match the Tier designations, Notion schemas cover all 70+ fields.

### Phase 4: Personalization Engine (updates to existing files)

**The 34 Tier 2 Template Variables** (all use `{{DOUBLE_CURLY_BRACES}}`):
```
{{AE_NAME}}, {{AE_TITLE}}, {{COMPANY}}, {{COMPANY_URL}},
{{PRODUCT_DESCRIPTION}}, {{TERRITORY_SIZE}}, {{TERRITORY_TYPE}},
{{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}},
{{CRM_SYSTEM}}, {{COMMUNICATION_STYLE}}, {{EMAIL_SIGN_OFF}},
{{LINKEDIN_TOPICS}}, {{LINKEDIN_AUDIENCE}},
{{TOP_COMPETITOR_1}}, {{TOP_COMPETITOR_2}}, {{TOP_COMPETITOR_3}},
{{NOTION_WORKSPACE_ID}}, {{NOTION_API_KEY}},
{{CALENDAR_CONNECTED}}, {{GMAIL_CONNECTED}}, {{DRIVE_CONNECTED}},
{{GAMMA_CONNECTED}}, {{PRIMARY_PRODUCT}}, {{SECONDARY_PRODUCTS}},
{{DEAL_STAGES}}, {{AVERAGE_DEAL_SIZE}}, {{SALES_CYCLE_LENGTH}},
{{FISCAL_YEAR_START}}, {{QUOTA_AMOUNT}}, {{MANAGER_NAME}},
{{TEAM_NAME}}, {{REGION}}
```

**104 Personalization Elements** (Bible Section 6.4 — 7 categories):
| Category | Count | Examples |
|----------|-------|---------|
| Company References | 23 | Company name, product names, pricing models, competitive positioning |
| Personal References | 18 | AE name, title, territory, key accounts, personal context |
| Competitive Intelligence | 21 | Competitor battlecards, displacement strategies, talk tracks |
| Industry/Vertical Data | 12 | Case studies by vertical, ICP definitions, industry-specific angles |
| Brand Voice Elements | 15 | Vocabulary substitutions, email sign-off, LinkedIn voice, banned phrases |
| Technical Configuration | 8 | Database IDs, connector settings, presentation brand tokens |
| Methodology Customization | 7 | Qualification gate criteria, discovery questions, coaching dimensions |

**Phase 4 tasks:**
- Verify all 104 personalization elements are addressed across files
- Ensure /setup's 7 phases populate every Tier 2 variable and trigger Tier 3 regeneration
- Verify Layer 2 first-use calibration is specified in relevant commands (see dev plan lines 516-526 for the per-command table)
- Add brand token capture spec to /setup (see dev plan lines 527-533 for the 4-step workflow)
- Cross-check: every {{VARIABLE}} used is set during /setup or first-use

**Note:** /setup is the most complex command. For full 7-phase execution detail (duration estimates, specific questions, what gets generated per phase), see `SALESSIDEKICK-DEV-PLAN.md` Phase 4, Section "The /setup Command — The Personalization Wizard." The dev plan's /setup spec is authoritative where this build spec is silent.

**Phase gate:** Template variable inventory complete. No orphaned variables. /setup covers everything.

### Phase 5: Connector Architecture (updates + CONNECTORS.md polish)

- CONNECTORS.md fully written with step-by-step setup for each connector
- .mcp.json finalized
- Every command's graceful degradation verified against the degradation table
- CRM export-first strategy documented

**Phase gate:** Graceful degradation table complete. System works with zero connectors.

### Phase 6: Distribution Package (6+ files)

**Primary channel:** GitHub (`github.com/chieflatif/SalesSidekick-Claude-CoWork`)
**Secondary channels:** Anthropic Plugin Directory (submit post-launch), Cowork custom upload, enterprise private marketplaces

| File | Path | Purpose |
|------|------|---------|
| README.md | README.md | First impression — sells the plugin, installation instructions for ALL channels |
| INSTALLATION-GUIDE.md | INSTALLATION-GUIDE.md | Detailed setup: GitHub download, Cowork upload, CLI install, enterprise deployment |
| QUICK-START.md | QUICK-START.md | 5-minute "run /setup, try /today, try /pipeline" onboarding |
| LICENSE | LICENSE (MIT) | Open-source license |
| CHANGELOG.md | CHANGELOG.md | Version history (starts with v2.0 launch) |
| docs/ | docs/ | Screenshots, GTM materials (Phase 7) |

**Distribution-aware requirements:**
- README must include installation instructions for: (1) GitHub download + Cowork upload, (2) Claude Code CLI `claude plugin install`, (3) enterprise admin deployment
- plugin.json (Phase 1) metadata must be optimized for directory discoverability — clear description, accurate tags, correct category
- README must position against Anthropic's existing generic `sales` plugin in the knowledge-work-plugins repo: SalesSidekick is a self-personalizing sales operating system, not a template collection
- All files must meet Anthropic plugin directory quality bar even though GitHub is the primary launch channel

**Note:** The dev plan's directory tree shows `SalesSidekick-Product-Bible-v2.docx` inside `docs/`. This is WRONG for the clean plugin repo. The Product Bible stays in the dev workspace only. The plugin's `docs/` directory contains only screenshots and GTM materials (Phase 7).

**Phase gate:** README is compelling and scannable. Installation steps cover all channels. All internal links work. plugin.json metadata is directory-ready.

### Phase 7: Go-To-Market Materials (4+ files)

| File | Path | Purpose |
|------|------|---------|
| Landing page | docs/landing-page.html | Standalone product page for sharing |
| Demo script | docs/demo-script.md | Walk-through covering key commands |
| Launch post | docs/launch-post.md | Social announcement (under 1,300 chars) |
| Enterprise guide | docs/enterprise-deployment.md | Team lead / admin deployment instructions |

**Phase gate:** Landing page renders clean. Demo script covers key commands. Launch post is under 1,300 characters. Enterprise guide covers private marketplace setup.

### Phase 8: Quality Assurance & Final Audit (0 new files)

Full audit checklist:

**Content integrity:**
- [ ] File inventory: 33+ files all present and non-empty
- [ ] Cross-reference: every command ↔ skill ↔ CLAUDE.md reference resolves
- [ ] Template variables: all {{VARIABLES}} documented, all populated by /setup or first-use
- [ ] Zero leakage: no André, Mapbox, Netflix, Bloomberg, CNN, Darden
- [ ] Commandment compliance: every command aligned, none violating
- [ ] Evidence grading: specified on all quantitative commands
- [ ] Graceful degradation: documented per command, system works with zero connectors

**Distribution quality:**
- [ ] README quality: accurate steps, working links, no typos, installation for all channels
- [ ] Clean for distribution: zero planning artifacts in the plugin directory
- [ ] plugin.json: valid metadata, accurate description, proper tags for discoverability
- [ ] INSTALLATION-GUIDE: all 3 installation methods tested/documented
- [ ] QUICK-START: 5-minute path works without connectors
- [ ] Plugin directory readiness: meets Anthropic submission quality bar
- [ ] Differentiation: clear positioning vs. generic sales plugins

---

## /EMAIL COMMAND SPEC (Full — Formerly /Q)

**File:** `commands/email.md`

**What it does:** Quick contextual email for existing relationships. Not cold outreach (/outreach). Not post-call follow-up (/closeout). Everyday business communication with accounts the AE already knows.

**How it works:** User says `/email Acme Corp about the POC timeline`. System pulls company record, primary contact, recent call notes, deal status from Notion. Drafts a short, natural email.

**Critical guardrails:**
1. **No puffing.** Use context to inform tone and relevance, NOT to pad the email. If user says "POC timeline," the email mentions POC timeline — it doesn't wedge in deal value, competitive landscape, and three case studies. A good /email output reads like something the AE would have written in 90 seconds.
2. **Confidence check.** Before drafting, assess if there's enough context. If company doesn't exist in Notion, or no recent call notes, or contact info missing — say so honestly. Offer to draft a generic version but flag what's missing: "I don't have recent call notes for {{COMPANY}}. Want me to draft based on the deal record alone, or do you want to give me a quick update first?"
3. **Evidence grading integration.** Maps to Commandment 8 (Laws of Karma — never fake confidence).

**Graceful degradation:** Gmail connected = can send. Gmail not connected = generates copy-paste text with subject line and body.

---

*This validated build spec is the single source of truth for the SalesSidekick build. Where it conflicts with the Product Bible or Dev Plan, this document wins.*

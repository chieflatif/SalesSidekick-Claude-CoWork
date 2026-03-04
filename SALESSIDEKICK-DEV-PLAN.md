# SalesSidekick Development Plan & Build Instructions
## The Complete Kickoff Brief for Building v2.0

**Project:** SalesSidekick — A self-personalizing AI sales plugin for Claude Cowork
**Owner:** Latif Horst (Pipeline Rebel / chieflatif)
**Repository:** `https://github.com/chieflatif/SalesSidekick-Claude-CoWork`
**Date:** 2026-03-03
**Status:** Ready for clean rebuild

---

## EXECUTIVE SUMMARY

You are building SalesSidekick from scratch in a clean repo. This is a **Claude Cowork plugin** — a downloadable folder that any enterprise sales rep installs into Claude's desktop app to get an AI-powered Chief of Staff for their entire sales workflow.

The prototype was built for one user (André Gouyet, Mapbox Enterprise AE). Your job is to rebuild it as a **productized, self-personalizing plugin** that works for ANY enterprise AE on day one. When they run `/setup`, the system asks them questions and rewrites itself to become *their* personalized sales AI.

**The Product Bible v2.0 (included in this repo as `SalesSidekick-Product-Bible-v2.docx`) is the source of truth for everything.** Read it before you start coding. This dev plan tells you *how* to build it. The Product Bible tells you *what* to build.

---

## TABLE OF CONTENTS

1. [Architecture Overview](#1-architecture-overview)
2. [File Inventory (33+ Files)](#2-file-inventory)
3. [Build Phases](#3-build-phases)
4. [Phase 1: Scaffold & Core Identity](#phase-1-scaffold--core-identity)
5. [Phase 2: The 22 Commands](#phase-2-the-22-commands)
6. [Phase 3: The 11 Skills](#phase-3-the-11-skills)
7. [Phase 4: Personalization Engine](#phase-4-personalization-engine)
8. [Phase 5: Connector Architecture](#phase-5-connector-architecture)
9. [Phase 6: GitHub Distribution Package](#phase-6-github-distribution-package)
10. [Phase 7: Go-To-Market Materials](#phase-7-go-to-market-materials)
11. [Phase 8: Quality Assurance & Final Audit](#phase-8-quality-assurance--final-audit)
12. [The 10 Commandments (Must Permeate Everything)](#the-10-commandments)
13. [Evidence Grading System (CRITICAL)](#evidence-grading-system)
14. [Reference: André Prototype → Productized Mapping](#reference-prototype-mapping)

---

## 1. ARCHITECTURE OVERVIEW

### What Is a Claude Cowork Plugin?

A Cowork plugin is a folder with a specific structure that Claude's desktop app recognizes. When a user installs it, Claude reads the files and becomes the persona/system defined in them.

### Directory Structure

```
SalesSidekick/
├── .claude-plugin/
│   └── plugin.json                    # Plugin metadata (name, version, description)
├── CLAUDE.md                          # ROOT IDENTITY — the brain of the entire system
├── CONNECTORS.md                      # MCP connector configuration guide
├── .mcp.json                          # MCP connector declarations
├── .gitignore
├── commands/
│   ├── setup.md                       # Initial personalization wizard
│   ├── closeout.md                    # Post-call processing (alias: /call)
│   ├── prep.md                        # Pre-meeting preparation
│   ├── strategy.md                    # Deal strategy (Five-Lens Prism)
│   ├── battle.md                      # Competitive battlecard generation
│   ├── pov.md                         # Point-of-view document creation
│   ├── deck.md                        # Presentation generation
│   ├── pipeline.md                    # Pipeline health analysis
│   ├── outreach.md                    # Prospecting email sequences
│   ├── coaching.md                    # Self-coaching framework
│   ├── whitespace.md                  # Territory whitespace analysis
│   ├── draft-post.md                  # LinkedIn post drafting
│   ├── today.md                       # Daily action view
│   ├── end-of-day.md                  # Evening ritual (alias: /eod)
│   ├── add-company.md                 # New account intake
│   ├── add-deal.md                    # New opportunity setup
│   ├── Q.md                           # Quick contextual email
│   ├── weekly.md                      # NEW — Weekly pipeline summary
│   ├── forecast-update.md             # NEW — CRM-ready forecast output
│   ├── research.md                    # NEW — Deep account research
│   ├── audit.md                       # NEW — 7-question self-critique protocol
│   └── skill-builder.md              # NEW — Commandment-validated command creation
├── skills/
│   ├── profile/SKILL.md               # AE identity and operating doctrine (Tier 3 — regenerated)
│   ├── brand-voice/SKILL.md           # Communication style guide (Tier 3 — regenerated)
│   ├── meddpicc/SKILL.md              # MEDDPICC scoring rubrics
│   ├── deal-strategy/SKILL.md         # Five-Lens Prism and strategy frameworks
│   ├── call-processing/SKILL.md       # 6-Output Framework specification
│   ├── company-intel/SKILL.md         # Company intelligence (Tier 3 — regenerated)
│   ├── battlecards/SKILL.md           # Competitive battlecards (Tier 3 — regenerated)
│   ├── notion/SKILL.md                # Database schemas and API patterns
│   ├── pptx/SKILL.md                  # PPTX generation pipeline
│   ├── gamma/SKILL.md                 # Alternative presentation path
│   └── posting-guide/SKILL.md         # LinkedIn 3-Type Framework
├── README.md                          # GitHub landing page (installation + overview)
├── INSTALLATION-GUIDE.md              # Detailed step-by-step setup
├── QUICK-START.md                     # 5-minute getting started guide
├── LICENSE                            # MIT License
└── docs/
    ├── SalesSidekick-Product-Bible-v2.docx   # Full product specification
    └── screenshots/                   # README images
        ├── setup-flow.png
        ├── command-examples.png
        └── notion-databases.png
```

### Key Architecture Principles

1. **CLAUDE.md is the brain.** Everything flows from it. It defines identity, references skills, lists commands, and enforces the 10 Commandments.
2. **Commands are .md files with YAML frontmatter.** They define what happens when a user types `/command-name`.
3. **Skills are SKILL.md files in subdirectories.** They provide deep knowledge that commands reference.
4. **Personalization happens through template variables.** `{{AE_NAME}}`, `{{COMPANY}}`, `{{TERRITORY_SIZE}}` etc. get replaced during `/setup`.
5. **The system must work with ZERO connectors.** Notion is the only "required" integration, and even without it, commands should degrade gracefully.

---

## 2. FILE INVENTORY

### Total: 33+ files across 4 categories

| Category | Count | Files |
|----------|-------|-------|
| Core Identity | 4 | CLAUDE.md, .mcp.json, CONNECTORS.md, plugin.json |
| Commands | 22 | setup.md through skill-builder.md |
| Skills | 11 | profile/ through posting-guide/ |
| Distribution | 5+ | README.md, INSTALLATION-GUIDE.md, QUICK-START.md, LICENSE, docs/ |

### Five NEW Commands (Not in Prototype)

These must be built from scratch. They do NOT exist in the André prototype:

| Command | Purpose | Key Behavior |
|---------|---------|--------------|
| `/weekly` | Weekly pipeline summary | Reads all active deals, groups by stage, highlights movement since last week, flags stalled deals, produces manager-ready summary |
| `/forecast-update` | CRM-ready forecast output | Weighted pipeline math (stage probability × deal value), commit/upside/best-case breakdown, Salesforce paste-ready format |
| `/research [Company]` | Deep account research | Web search + any available connectors, structured intel brief (overview, news, key personnel, tech stack, competitive landscape, selling angles), evidence-graded throughout |
| `/audit` | 7-question self-critique | Runs against most recent output OR specified output, asks 7 questions (see Evidence Grading section), three trigger modes |
| `/skill-builder` | Commandment-validated command creation | 5-phase workflow: Intent → Commandment Check → Scaffold → Audit → Install |

---

## 3. BUILD PHASES

Execute these phases in order. Each phase should be committable and testable independently.

| Phase | What | Est. Files | Priority |
|-------|------|-----------|----------|
| 1 | Scaffold & Core Identity | 5 | CRITICAL |
| 2 | The 22 Commands | 22 | CRITICAL |
| 3 | The 11 Skills | 11 | CRITICAL |
| 4 | Personalization Engine | Updates to existing files | HIGH |
| 5 | Connector Architecture | 2-3 | HIGH |
| 6 | GitHub Distribution Package | 5+ | HIGH |
| 7 | Go-To-Market Materials | 3-5 | MEDIUM |
| 8 | Quality Assurance & Final Audit | 0 (validation) | CRITICAL |

---

## PHASE 1: SCAFFOLD & CORE IDENTITY

### 1.1 Initialize Repository

```bash
mkdir SalesSidekick
cd SalesSidekick
git init
mkdir -p .claude-plugin commands skills docs/screenshots
```

### 1.2 Create plugin.json

```json
{
  "name": "salessidekick",
  "version": "2.0.0",
  "description": "Your AI-powered Chief of Staff for enterprise sales. 22 commands, 11 skills, 6 Notion databases. Self-personalizing — runs /setup once and becomes YOUR sales AI.",
  "author": {
    "name": "Pipeline Rebel (Latif Horst)"
  },
  "repository": "https://github.com/chieflatif/SalesSidekick-Claude-CoWork"
}
```

### 1.3 Create CLAUDE.md — The Brain

This is the most important file. It must include:

**Section 1: Identity**
- Generic: "You are {{AE_NAME}}'s AI sales partner and Chief of Staff."
- NOT locked to André, Mapbox, or any specific company
- Template variables for AE name, company, territory, product
- The voice/personality should be: sharp, strategic, no-BS, action-oriented

**Section 2: The 10 Commandments**
(Copy exactly from Product Bible v2.0 Section 1.2)

1. Speed is Life
2. Stop Digging, Start Orchestrating
3. Illuminate, Don't Dictate
4. Context is King
5. Voice First
6. The 10-Minute Rule
7. One Source of Truth
8. The Laws of Karma
9. Don't Pitch Features, Pitch Angles
10. Be the Chief of Staff

**Add enforcement paragraph:** "The 10 Commandments are not advisory — they are structural. The /audit command validates outputs against them. The /skill-builder command checks new commands for Commandment compliance. Every output you generate should be traceable to at least one Commandment."

**Section 3: The Flashlight Philosophy**
- Protocol: Illuminate, Don't Dictate
- Analyze → Identify Gaps → Propose 3 Paths → Ask
- Never execute a strategic pivot without user confirmation
- The Three Paths Framework: Velocity (Strike), Diagnostic (Go Deep), Protective (Pause)

**Section 4: Command Registry**
- List all 22 commands with one-line descriptions
- Group by category: Core Workflow, Intelligence & Strategy, Content & Communication, Pipeline Management, Territory Management, System Management

**Section 5: Evidence Grading**
- Three grades: Verified, Estimated, Hypothesis
- The 50% Rule
- Language patterns for each grade
- Mark as CRITICAL

**Section 6: Voice-to-Text Protocol**
- All input assumed to be voice-dictated
- Common mistranscriptions to watch for
- Interpret intent over literal text

**Section 7: Connector Awareness**
- Reference CONNECTORS.md
- Graceful degradation rules
- Never break, always adapt

**Section 8: Personalization Variables**
- List all template variables: `{{AE_NAME}}`, `{{AE_TITLE}}`, `{{COMPANY}}`, `{{PRODUCT_DESCRIPTION}}`, `{{TERRITORY_SIZE}}`, `{{ICP_DESCRIPTION}}`, `{{CRM_SYSTEM}}`, `{{COMMUNICATION_STYLE}}`, etc.
- Mark which are set during `/setup` vs. first-use calibration

### 1.4 Create .mcp.json

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer {{NOTION_API_KEY}}\", \"Notion-Version\": \"2022-06-28\"}"
      }
    }
  }
}
```

### 1.5 Create .gitignore

```
.env
*.log
node_modules/
.DS_Store
```

---

## PHASE 2: THE 22 COMMANDS

### Command File Format

Every command file follows this structure:

```markdown
---
name: command-name
description: One-line description shown in command list
aliases: [alias1, alias2]  # optional
category: Core Workflow | Intelligence & Strategy | Content & Communication | Pipeline Management | Territory Management | System Management
---

# /command-name — Display Title

## Purpose
What this command does and why it matters.

## When to Use
Trigger conditions and use cases.

## Inputs
What the user provides (required vs optional).

## Process
Step-by-step execution logic.

## Output Format
What the user receives.

## Database Interactions
Which Notion databases are read/written and which fields.

## Commandment Alignment
Which of the 10 Commandments this command serves.

## Evidence Grading
How evidence grading applies to this command's output.

## Graceful Degradation
What changes if specific connectors are missing.
```

### Command Build Order (by dependency)

**Batch 1 — Foundation (build first, everything depends on these):**
1. `setup.md` — The personalization wizard. MOST IMPORTANT COMMAND. See Phase 4 for full spec.
2. `audit.md` — Self-critique protocol. Everything references this.
3. `today.md` — Daily action view. The "home screen."
4. `end-of-day.md` — Evening ritual (alias: `/eod`).

**Batch 2 — Core Sales Workflow:**
5. `closeout.md` — Post-call processing (alias: `/call`). Uses 6-Output Framework skill.
6. `prep.md` — Pre-meeting preparation. Reads company + deal + contact + recent calls.
7. `strategy.md` — Deal strategy using Five-Lens Prism. Always offers Three Paths.
8. `battle.md` — Competitive battlecard. References battlecards skill.
9. `pov.md` — Point-of-view document. Evidence-graded. Most hypothesis-prone command.

**Batch 3 — Intelligence & Territory:**
10. `pipeline.md` — Pipeline health analysis. Reads all deals, scores health.
11. `whitespace.md` — Territory whitespace analysis. Identifies expansion opportunities.
12. `research.md` — NEW. Deep account research via web + connectors.
13. `weekly.md` — NEW. Weekly pipeline summary with week-over-week movement.
14. `forecast-update.md` — NEW. Weighted forecast math. CRM paste-ready.

**Batch 4 — Communication:**
15. `outreach.md` — Prospecting emails. Multi-thread sequences. Evidence-graded.
16. `Q.md` — Quick contextual email for existing relationships.
17. `draft-post.md` — LinkedIn post using 3-Type Framework.
18. `deck.md` — Presentation generation. Uses pptx skill or gamma skill.

**Batch 5 — Account Management:**
19. `add-company.md` — New account intake. Conversational, sequential.
20. `add-deal.md` — New opportunity. Links to company. MEDDPICC defaults to Red.
21. `coaching.md` — Self-coaching against MEDDPICC and call performance.

**Batch 6 — System:**
22. `skill-builder.md` — NEW. 5-phase command creation with Commandment validation.

### Detailed Specs for the 5 NEW Commands

#### /audit — Self-Critique Protocol

**The 7-Question Battery:**

| # | Question | What It Catches |
|---|----------|-----------------|
| 1 | What did you miss? | Gaps in coverage, overlooked stakeholders, missing data points |
| 2 | What did you get wrong? | Factual errors, misattributions, outdated information |
| 3 | What assumptions might bite us? | Unvalidated premises, market assumptions, competitive assumptions |
| 4 | What didn't you consider that is potentially critical? | Blind spots, adjacent risks, second-order effects |
| 5 | What did you make up? | Hallucinated facts, fabricated metrics, invented quotes (CRITICAL for AI integrity) |
| 6 | What other perspectives should we consider? | Stakeholder viewpoints, competitive counter-arguments, risk scenarios |
| 7 | What are the biggest opportunities for improvement? | Quality gaps, depth opportunities, actionable next steps |

**Three Trigger Modes:**
- **Manual:** User types `/audit` at any time to critique the most recent output or a specified prior output.
- **Suggested:** After any major output (`/strategy`, `/pov`, `/battle`, `/deck`, `/closeout`), the system recommends: "Run `/audit` to validate this output?" User can accept or skip.
- **Mandatory:** During `/setup`, the audit runs automatically on the initial configuration to catch setup errors before the user starts working.

**Audit + Evidence Grading Integration:**
When auditing a `/pov` or `/strategy` output, `/audit` specifically checks:
- Verifies all claims are tagged as Verified, Estimated, or Hypothesis
- Checks the 50% Rule: flags if more than half of the math is hypothesis-grade
- Identifies any claims that should be re-graded based on available evidence
- Recommends specific research actions to upgrade hypothesis-grade claims

#### /skill-builder — Command Creation with Integrity

**Five-Phase Workflow:**

| Phase | Name | What Happens |
|-------|------|-------------|
| 1 | Intent | User describes what they want the new command to do. System captures the goal, target output, and expected inputs. |
| 2 | Commandment Check | System evaluates the proposed command against all 10 Commandments. Flags conflicts. Example: a command that takes 30 minutes violates Commandment 6 (10-Minute Rule). |
| 3 | Scaffold | System generates the command file structure: YAML frontmatter, purpose section, execution steps, output format, database operations. Follows existing command patterns. |
| 4 | Audit | Automatic `/audit` run on the scaffolded command. The 7-question battery checks for gaps, assumptions, and integrity issues in the design. |
| 5 | Install | Command file saved to commands/ directory. CLAUDE.md command index updated. System confirms the new command is available. |

#### /weekly — Weekly Pipeline Summary

**Process:**
1. Read all active deals from Notion Deals database
2. Compare current stage/values to last week (if tracking data exists)
3. Group by: Commit, Upside, Pipeline
4. Highlight: deals that moved stages, deals that stalled (>7 days no activity), new deals added, deals lost/won
5. Produce manager-ready summary with specific call-to-action for each flagged deal

**Output:** Structured text block that can be pasted into email, Slack, or CRM notes field.

#### /forecast-update — CRM-Ready Forecast

**Process:**
1. Read all active deals with Stage, Value, Close Date, Forecast Category
2. Apply weighted pipeline math: Stage probability × Deal value
3. Produce three scenarios: Commit (high confidence), Upside (medium), Best Case (all deals)
4. Format as Salesforce paste-ready text (or HubSpot, based on `{{CRM_SYSTEM}}`)

**Evidence Grading:** All numbers must be tagged. Deal values from Notion = Verified. Stage probabilities = Estimated (standard multipliers). Growth assumptions = Hypothesis.

#### /research [Company] — Deep Account Intelligence

**Process:**
1. Web search for company (news, press releases, leadership changes, funding, tech stack)
2. If connectors available: pull CRM data, LinkedIn data, tech stack data
3. Structure into: Company Overview, Recent News & Events, Key Personnel, Technology Stack, Competitive Landscape, Strategic Selling Angles
4. Evidence-grade every claim throughout
5. Save structured brief to Notion Companies database (Notes field)

**Graceful Degradation:** Works with web search alone. Connectors enhance but are not required.

---

## PHASE 3: THE 11 SKILLS

### Skill File Format

Each skill is a `SKILL.md` file inside a named subdirectory of `skills/`.

```markdown
# Skill Name

## Purpose
What knowledge this skill provides.

## When Referenced
Which commands use this skill and how.

## Core Framework
The actual methodology, rubric, or framework content.

## Personalization Notes
What changes during /setup (Tier 1/2/3 designation).
```

### Skill Build Specifications

#### Tier 1 — Universal (No personalization needed)

| Skill | Key Content |
|-------|-------------|
| `meddpicc/` | 8-element scoring rubric (Metrics, Economic Buyer, Decision Criteria, Decision Process, Identify Pain, Champion, Competition, Paper Process). Each scored R/Y/G with confidence H/M/L. Include specific questions to ask for each element. |
| `deal-strategy/` | Five-Lens Prism (Stakeholder Psychology, Political Capital, Competitive Dynamics, Hidden Leverage, Temporal Dynamics). Three Paths Framework (Velocity/Diagnostic/Protective). Strategy document template. |
| `call-processing/` | 6-Output Framework: (1) Call summary, (2) Action items with owners/dates, (3) MEDDPICC score updates, (4) Competitive intel extracted, (5) Coaching moment, (6) Next-step recommendation. |
| `notion/` | 6 database schemas (Companies 12 fields, Contacts 9, Deals 22, Tasks 8, Call Notes 10, LinkedIn Posts 9). Read/write patterns for each command. Relation field specifications. |
| `pptx/` | Presentation generation pipeline. Slide structure templates. Brand token integration (colors, fonts, logo placement from `brand-tokens.json`). |
| `gamma/` | Alternative presentation path via Gamma. When to use vs native PPTX. Prompt templates for Gamma. |

#### Tier 2 — Template Variables (Personalized during /setup)

| Skill | Key Content |
|-------|-------------|
| `posting-guide/` | LinkedIn 3-Type Framework: Type 1 Personal (story-to-lesson), Type 2 Business (insight-to-proof), Type 3 Educational (framework-to-takeaway). NOTE: These are 3 post types, NOT 3 personas. One persona, three content formats. Template variables for `{{LINKEDIN_TOPICS}}`, `{{LINKEDIN_AUDIENCE}}`. |

#### Tier 3 — Regenerated (Fully rewritten during /setup)

| Skill | Key Content |
|-------|-------------|
| `profile/` | AE identity file. Starts as template. During `/setup`, gets completely rewritten with user's name, title, company, territory, ICP, selling methodology preferences, personal context. |
| `brand-voice/` | Communication style. During `/setup`, user provides sample emails or selects dimensions (formal↔casual, data-driven↔narrative, concise↔detailed). System generates vocabulary rules, email formatting, banned phrases. |
| `company-intel/` | Was `mapbox-intel/` in prototype. During `/setup`, system researches user's company via web, generates product portfolio, market position, key differentiators, case studies, pricing context. |
| `battlecards/` | During `/setup`, user identifies 3-7 competitors. System researches each, generates displacement strategies, strengths/weaknesses, trap questions, landmine responses. |

---

## PHASE 4: PERSONALIZATION ENGINE

### The Three-Tier System

**Tier 1: Universal** — Frameworks and methodologies. MEDDPICC rubric is the same for everyone. Keep as-is.

**Tier 2: Template Variables** — 34 placeholder variables that get substituted during `/setup`:

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

**Tier 3: Regenerated Files** — These 4 skills get completely rewritten by AI during `/setup`:
- `skills/profile/SKILL.md`
- `skills/brand-voice/SKILL.md`
- `skills/company-intel/SKILL.md`
- `skills/battlecards/SKILL.md`

### /setup Command — The Personalization Wizard

This is the MOST IMPORTANT command. It transforms a generic plugin into a personalized sales AI.

**Layer 1: Global Setup (8-10 questions, asked during /setup, injected everywhere)**

1. What is your name and title?
2. What company do you work for? (triggers web research for company-intel)
3. What do you sell? (product/service description)
4. Who is your ideal customer? (ICP: industry, size, use case)
5. Who are your top 3-5 competitors? (triggers battlecard generation)
6. How formal is your communication style? (casual to enterprise-formal)
7. What CRM do you use? (Salesforce, HubSpot, other, none)
8. What is your territory size? (account count, greenfield vs renewal mix)
9. Which connectors are available? (auto-detected + confirmed)

**Layer 2: First-Use Calibration (per command, 1-2 questions on first run)**

| Command | First-Use Question | Why |
|---------|-------------------|-----|
| /outreach | What is your typical email tone? (paste a sample email) | Calibrates email voice beyond the global formal/casual setting |
| /draft-post | What LinkedIn topics resonate with your audience? | Focuses content on high-engagement themes |
| /deck | How do you prefer presentations? (data-heavy vs narrative vs visual) | Selects default template emphasis |
| /closeout | What call elements matter most to you? (MEDDPICC vs tasks vs coaching) | Weights the 6-Output Framework |
| /strategy | Do you prefer aggressive or conservative deal strategy recommendations? | Calibrates Three Paths threshold |

First-use answers get stored in `skills/profile/SKILL.md` and persist across sessions.

### Presentation Brand Capture (During /setup Phase 5)

1. **Screenshot Analysis:** User provides a screenshot of existing presentation or marketing material. System extracts primary colors, font families, layout patterns.
2. **Website URL Analysis:** User provides company website URL. System analyzes the site's color palette, typography, visual style.
3. **Brand Token Generation:** System generates a `brand-tokens.json` file with color hex codes, font specifications, logo placement rules.
4. **Sample Slide Verification:** System generates one sample slide using the extracted brand tokens and asks the user to confirm or adjust.

### /setup Execution Flow (7 Phases)

| Phase | Name | Duration | What Happens |
|-------|------|----------|-------------|
| 1 | Identity Setup | 5 min | Global setup questions (Layer 1). AE name, title, company, territory written to CLAUDE.md. CRM system and available connectors recorded. |
| 2 | Company Intelligence | 10-15 min | User provides company URL and product description. System researches and generates draft company-intel file. User reviews and refines key metrics, product portfolio, positioning. |
| 3 | Competitive Landscape | 10-15 min | User identifies top 3-7 competitors. System researches each competitor and generates draft battlecards. User validates strengths, weaknesses, displacement strategies. |
| 4 | Brand Voice | 5 min | System presents voice dimension options (formal vs casual, data-driven vs narrative, etc.). User selects preferences or provides sample emails for voice extraction. System generates brand-voice file with vocabulary rules and email formatting. |
| 5 | Presentation Brand | 5 min | Screenshot and/or website URL provided for brand capture. Brand tokens generated and verified via sample slide. |
| 6 | Connector Setup | 5-10 min | Guided Notion database creation (6 databases with exact schemas). MCP configuration for Notion API. Optional connector setup (Gmail, Calendar, Drive, Gamma). Graceful degradation rules confirmed for missing connectors. |
| 7 | Verification + Mandatory Audit | 5 min | System runs a smoke test: creates a test company record, writes and reads back. Mandatory `/audit` runs on the entire configuration. Confirms all commands are functional. Produces a "System Ready" confirmation with summary. |

---

## PHASE 5: CONNECTOR ARCHITECTURE

### Connector Tiers

| Tier | Connector | Purpose | Status |
|------|-----------|---------|--------|
| P0 Required | Notion | Single source of truth. 6 databases. All read/write operations. | Configured via .mcp.json |
| P1 High Value | Gmail | Email sending, thread reading, follow-up tracking | User configures during /setup |
| P1 High Value | Google Calendar | Meeting context for /today and /prep | User configures during /setup |
| P1 High Value | Google Drive | Document storage, transcript access for /closeout | User configures during /setup |
| P2 Optional | Gamma | Alternative presentation generation path | User selects during /setup |
| P2 Future | Salesforce (read-only) | Minimal fields: Open Opportunities, Account basics, Activity timestamps | Phase 2 — read-only connector |
| P2 Future | HubSpot (read-only) | Alternative CRM path for non-Salesforce users | Phase 2 — read-only connector |

### Graceful Degradation Table

**CRITICAL DESIGN PRINCIPLE: Missing connectors change behavior, not break it.**

| Missing Connector | What Changes | Fallback Behavior |
|-------------------|-------------|-------------------|
| No Calendar | /today and /prep cannot read meeting schedule | Asks "What meetings do you have today?" and incorporates the answer |
| No Gmail | /outreach and /closeout cannot send emails | Generates copy-paste formatted text with subject line and body |
| No Google Drive | /closeout cannot auto-discover transcripts | Asks user to paste the transcript directly or provide a Drive link |
| No CRM Connector | /forecast-update cannot write to Salesforce/HubSpot | Generates Salesforce paste-ready formatted output (export-first strategy) |
| No Gamma | /deck cannot use Gamma for presentation generation | Generates native .pptx via PptxGenJS (the default path) |

### CONNECTORS.md Content

This file should explain:
1. How MCP connectors work in Claude Cowork
2. Which connectors SalesSidekick supports
3. Step-by-step setup for each connector (Notion API key, Gmail OAuth, etc.)
4. How to verify each connector is working
5. What happens when a connector is missing (graceful degradation)

---

## PHASE 6: GITHUB DISTRIBUTION PACKAGE

### README.md — The GitHub Landing Page

This is the first thing potential users see. It must be:
- Visually compelling (badges, screenshots, clear sections)
- Scannable in 30 seconds
- Copy-paste installable

**Structure:**

```markdown
# 🎯 SalesSidekick — Your AI Chief of Staff for Enterprise Sales

> 22 commands. 11 skills. 6 databases. One setup. Your entire sales workflow, orchestrated by AI.

[badges: version, license, commands count, stars]

## What Is SalesSidekick?

[2-3 sentence description + value prop]

## ⚡ Quick Start (5 Minutes)

1. Download this repository
2. Open Claude Desktop App
3. Install the plugin (drag folder or use plugin installer)
4. Type `/setup` and answer 8-10 questions
5. You now have a personalized AI sales partner

## 🎬 See It In Action

[Screenshots or GIFs of key commands: /setup, /strategy, /closeout, /pipeline]

## 📋 All 22 Commands

[Grouped table: Core Workflow, Intelligence & Strategy, etc.]

## 🧠 How It Works

[Brief architecture explanation: CLAUDE.md brain, commands, skills, Notion backbone]

## 🔌 Connectors

[What's required vs optional, with graceful degradation note]

## 📦 Installation

[Link to INSTALLATION-GUIDE.md for detailed steps]

## 🗺️ Roadmap

[What's coming: CRM read-only, multi-agent, team features]

## License

MIT
```

### INSTALLATION-GUIDE.md

Detailed step-by-step:
1. Prerequisites (Claude Desktop App, Notion account)
2. Download options (git clone, ZIP download, or Cowork plugin installer)
3. Plugin installation into Claude
4. Notion setup (creating databases with exact field specifications)
5. MCP connector configuration
6. Running /setup
7. Verification checklist
8. Troubleshooting common issues

### QUICK-START.md

The "I have 5 minutes" version:
1. Install plugin
2. Run /setup
3. Try /today
4. Try /prep [company name]
5. You're live

### LICENSE

MIT License. Standard open-source.

---

## PHASE 7: GO-TO-MARKET MATERIALS

### 7.1 Landing Page (HTML)

Create a single-page HTML file (`docs/landing-page.html`) that could be hosted on GitHub Pages:
- Hero section with value prop
- "Before SalesSidekick / After SalesSidekick" comparison
- Command showcase (interactive or static)
- Testimonial placeholder
- CTA: "Get Started Free on GitHub"
- Built with Tailwind CDN for clean styling

### 7.2 Demo Script

Create `docs/demo-script.md` — a scripted walkthrough for showing SalesSidekick:
1. Fresh install → /setup flow (2 min condensed)
2. /today → show daily view
3. /prep [Company] → show pre-meeting prep
4. /strategy [Deal] → show Five-Lens analysis
5. /closeout → show post-call processing
6. /audit → show self-critique
7. /pipeline → show portfolio health

### 7.3 LinkedIn Announcement Post

Create `docs/launch-post.md` — ready-to-post LinkedIn content announcing SalesSidekick:
- Pipeline Rebel voice (Latif's style)
- Type 2 Business format (insight-to-proof)
- Hook + problem + what we built + numbers + CTA
- Under 1,300 characters

---

## PHASE 8: QUALITY ASSURANCE & FINAL AUDIT

### 8.1 File Inventory Check

Verify all 33+ files exist and are non-empty:
```bash
# Run this to verify
find . -name "*.md" -o -name "*.json" | sort
# Cross-reference against the inventory in this document
```

### 8.2 Cross-Reference Validation

For every command referenced in CLAUDE.md:
- ✅ Corresponding .md file exists in commands/
- ✅ YAML frontmatter is valid
- ✅ Command references correct skills
- ✅ Database fields mentioned actually exist in notion/SKILL.md schema

For every skill referenced by a command:
- ✅ Corresponding SKILL.md exists in skills/
- ✅ Referenced frameworks/rubrics are complete

### 8.3 Template Variable Audit

- ✅ All `{{VARIABLE}}` placeholders are documented in CLAUDE.md Section 8
- ✅ /setup asks a question that populates each variable
- ✅ No orphaned variables (referenced but never set)
- ✅ No hardcoded André/Mapbox references anywhere

### 8.4 Commandment Compliance

For each of the 22 commands, verify:
- ✅ Aligns with at least one Commandment
- ✅ Doesn't violate any Commandment (especially #3 Illuminate Don't Dictate, #6 10-Minute Rule)
- ✅ Evidence grading is specified where quantitative claims are generated

### 8.5 Graceful Degradation Test

For each command, verify:
- ✅ Documentation specifies behavior with AND without each connector
- ✅ No command completely breaks without an optional connector
- ✅ Error messages are helpful, not technical

### 8.6 README Quality Check

- ✅ Installation steps are copy-paste accurate
- ✅ All links work
- ✅ Screenshots exist (or placeholders noted)
- ✅ No typos in command names
- ✅ Badge links are valid

### 8.7 Git Hygiene

- ✅ Clean commit history (one commit per phase is ideal)
- ✅ .gitignore covers .env, node_modules, .DS_Store
- ✅ No secrets or API keys in any file
- ✅ No André-specific content leaked through

---

## THE 10 COMMANDMENTS

These must permeate EVERY file in the system. They're not just a list — they're the operating doctrine.

| # | Commandment | Primary Enforcement Point |
|---|-------------|--------------------------|
| 1 | Speed is Life | /today micro-tasks, /skill-builder rejects commands >10 min |
| 2 | Stop Digging, Start Orchestrating | Every command automates research (AI digs, human orchestrates) |
| 3 | Illuminate, Don't Dictate | /strategy Three Paths (always multiple options), /pov (options, not mandates) |
| 4 | Context is King | /outreach (never reach out naked), /prep (always loaded), /closeout (always contextual) |
| 5 | Voice First | Voice-to-text interpretation throughout, forgiving input parsing |
| 6 | The 10-Minute Rule | Task estimation in /closeout, micro-task breakdown in /today |
| 7 | One Source of Truth | Notion as backbone, CRM export-first (not dual-write) |
| 8 | The Laws of Karma | Evidence grading (never fabricate), /audit (never fake confidence) |
| 9 | Don't Pitch Features, Pitch Angles | /outreach angles, /pov blind spots, /strategy wedges |
| 10 | Be the Chief of Staff | /end-of-day (nothing falls through), /today (AE never asks "what's next?") |

---

## EVIDENCE GRADING SYSTEM

**CRITICAL: This is the system's integrity backbone.**

Every quantitative claim in strategy documents and POVs MUST be classified:

| Grade | Definition | Language Pattern | Example |
|-------|-----------|-----------------|---------|
| Verified | From case studies, public data, or customer-confirmed metrics. Can be cited with confidence. | Direct statement with source | "Customers report 88% improvement in urban route efficiency (Kraken/Octopus Energy case study)." |
| Estimated | Calculated from available data using reasonable assumptions. Must be labeled as estimate. | "Based on... we estimate" or "Our analysis suggests" | "Based on your current fleet size of 200 vehicles, we estimate annual savings of $2.4M." |
| Hypothesis | Educated projections based on industry patterns. Must be framed carefully. | "We typically see" or "Industry benchmarks suggest" | "Companies in your segment typically see 15-25% reduction in mapping infrastructure costs." |

**The 50% Rule:** If more than 50% of the math in a POV or strategy is hypothesis-grade, the document needs more research before delivery. The `/audit` command specifically checks for this threshold and flags it.

**Why This Is Non-Negotiable:** Enterprise buyers are sophisticated. They will fact-check your numbers. If an AI-generated POV contains fabricated metrics presented as verified facts, it destroys not just one deal — it destroys the AE's credibility permanently. Evidence grading is the mechanism that prevents this.

---

## REFERENCE: PROTOTYPE → PRODUCTIZED MAPPING

Use this to understand what changes from André's version:

| André Prototype | Productized Version | Change Type |
|----------------|---------------------|-------------|
| "André Gouyet's AI sales partner" | "{{AE_NAME}}'s AI sales partner" | Template variable |
| "Mapbox Enterprise AE" | "{{COMPANY}} {{AE_TITLE}}" | Template variable |
| skills/mapbox-intel/ | skills/company-intel/ | Renamed + Tier 3 regenerated |
| 17 commands | 22 commands | +5 new commands |
| Netflix, Bloomberg, CNN, Darden | Removed — populated during /setup | Tier 3 personalization |
| Hardcoded territory (55 accounts) | {{TERRITORY_SIZE}} variable | Template variable |
| "alcohol-free since October 2025" | Removed — personal context via /setup | Tier 3 personalization |
| Fixed battlecards (Esri, Google, HERE) | Regenerated during /setup | Tier 3 regenerated |
| No /audit | Full 7-question self-critique | New command |
| No /skill-builder | 5-phase command creation | New command |
| No graceful degradation docs | Full degradation table per command | New architecture |
| No first-use calibration | Layer 2 per-command questions | New personalization |
| No brand capture | Screenshot + URL analysis | New feature |
| No CRM strategy | Export-first with future read-only | New architecture |

---

## FINAL NOTES FOR THE BUILD AGENT

1. **Read the Product Bible v2.0 first.** It's the source of truth. This dev plan tells you how to structure the build; the Bible tells you what goes in each file.

2. **Build incrementally.** Each phase should be a clean commit. Don't try to write all 33 files at once.

3. **No André leakage.** Search every file for "André", "Mapbox", "Netflix", "Bloomberg" before committing. Zero hardcoded personal references.

4. **Template variables must be obvious.** Use `{{DOUBLE_CURLY_BRACES}}` so they're easy to find and replace.

5. **Every command must work on day one.** Even before /setup runs, the commands should produce useful output with generic defaults. After /setup, they become personalized.

6. **The 10 Commandments are structural, not decorative.** If a command violates a Commandment, redesign the command.

7. **Evidence grading is not optional.** Any command that generates quantitative claims MUST implement the three-grade system.

8. **Voice-to-text forgiveness everywhere.** Users are dictating, not typing. Parse intent, not grammar.

9. **Notion is the backbone, but nothing breaks without it.** Every command should have a fallback for when Notion isn't connected yet.

10. **The README sells the product.** Spend real effort on it. It's the first impression for every potential user.

---

*This development plan was generated alongside the SalesSidekick Product Bible v2.0. Together, they form the complete specification for building the productized SalesSidekick plugin.*

*— Pipeline Rebel, March 2026*

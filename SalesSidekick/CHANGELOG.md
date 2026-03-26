# Changelog

All notable changes to SalesSidekick are documented here.

Each release is marked: **✅ No action needed** or **⚠️ Action required** (with instructions).

---

## v4.0.0 — Local-First Architecture (2026-03-25)

**⚠️ Major version — see "Upgrading from v3" below.**

### New

**Everything runs locally — no external services required**
- Your deals, contacts, call notes, tasks, and all data now live as files in your SalesSidekick workspace folder. No database service needed to get started.
- Your identity is saved automatically during setup — no copy-paste step.
- Background agents check your pipeline every morning and generate a briefing before you open your laptop.

**Signal intelligence**
- 12 risk patterns monitored automatically: stalling deals, missing decision-makers, forecast conflicts, qualification gaps, and more.
- Findings ranked by severity and included in your morning briefing.
- Configurable thresholds based on your typical deal size.

**Selling style assessment**
- 7 optional questions during setup that calibrate how the system works for you.
- Affects email length, strategy aggressiveness, briefing format, and forecast framing.
- Skip it and the system figures it out from how you work over the first few sessions.

**Custom skills and knowledge bases**
- Customize how any capability works by placing a modified file in your workspace.
- Build knowledge bases from your documents — paste anything in and the system consolidates it.
- Bring custom GPTs into SalesSidekick: paste the system prompt and documents, get a working skill.
- Everything in your workspace survives plugin updates.

**Upgrade detection**
- The system detects when you've installed a new version and shows you what's changed.
- Schema migrations apply automatically — your data files are updated to the new format.
- If you've customized capabilities, the system lets you compare your version with the updated default.

### Changed

- Commandment #7 updated: "Your workspace is where real work happens" (previously referenced an external service)
- Connectors (Gmail, Calendar, Drive, Notion) are all optional — none required
- Onboarding rewritten: workspace setup, automatic identity save, style assessment, background briefing setup
- State detection uses identity markers in settings file instead of external service

### Upgrading from v3

1. Download the new zip and upload to Plugins (replacing the old version)
2. Create a SalesSidekick folder in your Documents
3. Create a Cowork Project pointing at that folder
4. Open a conversation — the system detects your existing identity and guides you through the migration
5. If you have data in Notion from v3, the system offers to copy it to your local workspace

Your existing identity is preserved. Your Notion data is preserved (and can still be accessed if you keep Notion connected).

---

## v3.0.2 — Onboarding, Global Settings, and Upgrade (2026-03-19)

**✅ No action needed** — upgrade by re-uploading the zip. Existing Notion data and Config page are untouched.

### Fixed

**Global settings block — always complete, never partial**
- System now always generates the COMPLETE replacement block when global settings need updating. Previously it could prompt "add X to your settings" — now it always provides the full copy-paste replacement block.
- Global settings update triggers added: after new competitor discovered, after brand voice calibration, after deep personalization, after any identity change.
- System now detects when global settings are empty or generic and guides the user to fix it — without saying "I don't know who you are."

**Duplicate database creation**
- Phase 6 now explicitly searches Notion by database name before creating anything. No assumption that FRESH state = no databases.
- Added handling for ambiguous database matches: asks user to confirm each found database before adopting it.
- If user says it's not theirs, creates a new one with a distinct name rather than guessing.

**Onboarding experience**
- First-run flow now opens with a warm, non-technical introduction before any logistics.
- Two entry points offered immediately after setup: share top 3 deals (paste CRM screenshot) or drop in a call transcript.
- Patience messaging added for first-time Notion database creation.
- Setup completion summary rewritten in plain language — no commands, no technical jargon.

**System self-explanation**
- New Section 14 "How to Explain Yourself" — Claude now has explicit guidance on how to explain SalesSidekick to users in plain, conversational language when asked "what can you do?" or "how do I use this?"

### Added
- QUICK-START.md: full rewrite with voice-first entry points, Notion skip explanation, no-jargon instructions
- INSTALLATION-GUIDE.md: global settings starter template annotated with what's missing and why; v2→v3 database ID recovery clarified

---

## v3.0.1 — Persistence & Onboarding Fixes (2026-03-18)

**✅ No action needed** — upgrade by re-uploading the zip. Existing Notion data and Config page are untouched.

### Fixed

**Persistence architecture**
- Plugin files (CLAUDE.md, skill files) are read-only in Cowork and cannot be written at runtime. All Tier 2 variables are now stored in a Notion Config page (`SalesSidekick — Config`) as the live persistence layer. Previously, captured identity data was silently discarded each session.
- State detection (FRESH / BASICS / LEARNING / CALIBRATED) now reads from the Notion Config page, not CLAUDE.md template placeholders.

**Lazy Config loading**
- Notion Config page now loads lazily on first database operation, not at session start. Sessions that don't touch Notion data have zero API overhead. Basic identity (name, company) comes from the Cowork global settings field — always available, no API call.

**First-run onboarding**
- Connector detection now uses a live Notion API test (attempt read, report result). Non-Notion connectors cannot be probed — users directed to Cowork Settings.
- After research is confirmed, the system generates a personalized global settings block for the user to paste into Claude Desktop > Settings > Cowork. This is the stable identity layer — loads before every session without reading Notion.

**Two-layer identity architecture**
- Global settings = stable context (identity, product, ICP, competitors, communication style) — paste once, available every session
- Notion Config = operational context (database IDs, deal stages, connector status, personalization state) — lazy loaded on first data operation
- Notion databases = refreshable content (battlecards, case studies, deal data) — updated as things change

### Added
- `PLUGIN_VERSION` field in Notion Config page — enables "you just upgraded" detection for future releases
- Upgrade documentation in INSTALLATION-GUIDE.md — full process, what's preserved, v2→v3 migration path

---

## v3.0.0 — Natural Language Architecture (2026-03-08)

### Changed

**Natural Language Interface**
- All capabilities now accessible through natural language — just tell SalesSidekick what you need
- Intent engine classifies 21 intent categories and routes to the right capabilities automatically
- Compound intent support — "wrap up my Acme call and prep for Globex" executes both in sequence
- Slash commands still work as power-user shortcuts but are never required or surfaced

**Progressive Personalization**
- No more 45-minute setup wizard required — system works from the first conversation
- Getting started: just name, company, and what you sell (under 2 minutes)
- Variables captured organically through use — competitors from calls, voice from emails, ICP from deal patterns
- Personalization state machine (FRESH → BASICS → LEARNING → CALIBRATED) adjusts behavior automatically

**Database-on-Demand**
- 6 Notion databases created when first needed, not upfront
- One-sentence confirmation per database — "Want me to save this? I'll set up a [database]."
- Declining doesn't break anything — output still generated, just not persisted

**Proactive Data Capture**
- After processing calls, research, or content creation, system offers to save relevant data
- Batch confirmation — one "yes" for multiple database writes
- System captures contacts, tasks, competitive intel, and deal updates automatically

**Deep Personalization (Optional)**
- Setup reimagined as optional deep personalization (~15 minutes instead of 45)
- Detects existing context and skips what's already known
- Each phase runs independently — "just do battlecards" or "calibrate my brand voice"

**Documentation**
- All user-facing docs rewritten for conversation-first framing
- No slash commands in primary documentation
- README leads with conversation examples, not feature lists
- Quick Start reduced to 3 steps: install, connect Notion, start talking

### Preserved
- All 22 capabilities from v2.0 — nothing removed, only invocation method changed
- All 11 skills with full framework content
- 6 Notion database schemas (70 fields total)
- Evidence grading, MEDDPICC, Five-Lens Prism, Three Paths, 6-Output Framework
- 10 Commandments governance
- Graceful degradation for all connectors

---

## v2.0.0 — Launch (2026-03-03)

### Added

**Core System**
- CLAUDE.md — 16-section brain with identity, 10 Commandments, frameworks, configuration
- CONNECTORS.md — Connector setup, graceful degradation reference, CRM strategy
- .mcp.json — Notion MCP server configuration
- plugin.json — Plugin manifest with metadata and connector requirements

**22 Commands**
- Daily Ops: `/today`, `/end-of-day` (alias `/eod`), `/weekly`
- Call Processing: `/closeout` (alias `/call`) — 6-Output Framework
- Meeting Prep: `/prep`
- Deal Strategy: `/strategy`, `/battle`, `/pov`
- Territory Management: `/pipeline`, `/forecast-update`, `/whitespace`
- Intelligence: `/research`
- Communication: `/outreach`, `/email`, `/draft-post`, `/deck`
- Account Management: `/add-company`, `/add-deal`, `/coaching`
- System: `/setup`, `/audit`, `/skill-builder`

**11 Skills**
- Universal (Tier 1): notion, meddpicc, call-processing, deal-strategy, pptx, gamma
- Template (Tier 2): posting-guide
- Regenerated (Tier 3): profile, brand-voice, company-intel, battlecards

**6 Notion Databases** (auto-created during /setup)
- Companies (12 fields), Contacts (9), Deals (22), Tasks (9), Call Notes (10), LinkedIn Posts (8)

**Key Frameworks**
- Evidence Grading: Verified / Estimated / Hypothesis with 50% Rule
- MEDDPICC: 8-element Red/Yellow/Green scoring with discovery questions
- Five-Lens Prism: Stakeholder Psychology, Political Capital, Competitive Dynamics, Hidden Leverage, Temporal Dynamics
- Three Paths: Velocity (Strike), Diagnostic (Go Deep), Protective (Pause)
- 6-Output Framework: MEDDPICC scoring, task extraction, coaching feedback, follow-up email, risk signals, competitive intel
- Context Wedge: Industry trend + specific pain + quantified impact
- 3-Type LinkedIn Framework: Personal, Business, Educational

**Personalization Engine**
- 3-tier personalization: Universal frameworks, template variables, regenerated skills
- 34 Tier 2 template variables + 5 connector status variables populated during /setup
- 5 first-use calibration commands for progressive personalization
- 104 personalization elements across 7 categories

**Distribution**
- README, Installation Guide, Quick Start, License (Personal Use), Changelog
- Supports: GitHub download, Claude Code CLI, Cowork upload, enterprise marketplace

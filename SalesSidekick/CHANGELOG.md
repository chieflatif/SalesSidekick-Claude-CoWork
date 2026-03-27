# Changelog

All notable changes to SalesSidekick are documented here.

---

## v4.1.0 — Compounding Intelligence (2026-03-27)

**Your AI gets smarter the more you use it.**

### New

**Quick Capture (`/note`)**
- Capture deal intel in under 10 seconds — hallway conversations, Slack messages, LinkedIn observations
- Auto-classifies to company, contact, and deal. Flags MEDDPICC changes. Offers task creation.
- Aliases: `/n`, `/brain`

**Pattern Memory**
- New skill that extracts reusable patterns from your deals — what works, what doesn't, timing insights
- After every call debrief, 0-2 patterns are extracted and stored
- On deal close (won or lost), a full win/loss synthesis runs with 3-5 lessons saved
- Patterns compound over time — after 20+ deals, you have a personalized coaching engine

**Bulk Transcript Ingestion (`/ingest-calls`)**
- Drop in a month or quarter of call transcripts — system maps your entire territory
- Scans all transcripts, clusters by company, identifies opportunities, then processes chronologically
- Extracts signals (not raw text): people, MEDDPICC, competitive, risk, decision, action, relationship
- Handles 50-100+ transcripts by processing one at a time and storing signal-dense summaries
- The "cold start killer" — go from zero to fully populated pipeline in one session

**Community & Support Intent**
- System now responds to "how do I get help?", "referral program", "pricing", "updates", "office hours"
- Routes to the right answer from Section 20 based on what you asked

**Capability Usage Tracking**
- System tracks which capabilities each user has used (simple counters)
- Tips become contextual — prioritize tips about capabilities the user hasn't discovered yet
- Drives smarter onboarding: if you've never run a meeting prep, the prep tip surfaces next

**Connector Workaround Guidance**
- When Calendar, Gmail, Drive, or CRM aren't connected, system proactively offers workarounds
- Screenshots, exports, personal email invites, transcript downloads — practical alternatives explained

### Improved

**Smarter Context Loading**
- `/prep` now loads ALL call notes (including quick captures), pattern history, and cross-deal insights
- `/today` surfaces intel from the last 48 hours that hasn't been reflected in deal scores yet
- `/weekly` detects recurring themes across deals and flags matches against historical patterns
- `/closeout` extracts pattern notes after processing and offers win/loss synthesis on deal close

**Tips Rotation**
- 18 tips (up from 15) — 3 new community/referral tips mixed naturally across weeks
- Community trial mentioned at end of onboarding

### Schema Changes
- New entity type: `pattern` (stored in `data/patterns/`)
- New `call_type` value: `note` (for quick captures)
- New directory: `data/patterns/` (created during workspace setup)

---

## v4.0.0 — Local-First Architecture (2026-03-25)

**Everything runs locally. No external services required.**

### New

**Local workspace**
- All your data lives in your SalesSidekick workspace folder — deals, contacts, call notes, tasks
- Identity saved automatically during setup. No copy-paste steps.
- Background agents check your pipeline every morning and generate a briefing

**Signal intelligence**
- 12 risk patterns monitored: stalling deals, missing decision-makers, forecast conflicts, qualification gaps
- Findings ranked by severity, included in your morning briefing
- Thresholds calibrated to your typical deal size

**Selling style assessment**
- 7 optional questions that calibrate how the system works for you
- Affects email length, strategy aggressiveness, briefing format, forecast framing
- Skip it and the system learns from how you work

**Custom skills and knowledge bases**
- Customize any capability by placing a modified file in your workspace
- Build knowledge bases from your documents — paste anything, the system consolidates it
- Bring custom GPTs into SalesSidekick: paste the system prompt and documents
- Everything in your workspace survives plugin updates

**Upgrade detection**
- System detects when you've installed a new version and shows what's changed
- Data files updated to new format automatically
- Custom skill conflicts flagged with option to compare and merge

### Changed
- All connectors (Gmail, Calendar, Drive) are optional — none required
- Onboarding rewritten: workspace setup, automatic identity save, style assessment, background briefings
- "Work partner" framing: SalesSidekick positioned as a work colleague for strategic sellers

### Upgrading from earlier versions

1. Download the new zip and upload to Plugins (replacing the old version)
2. Create a SalesSidekick folder in your Documents
3. Create a Cowork Project pointing at that folder
4. Open a conversation — the system detects your existing identity and guides you through

Your identity and any existing data are preserved.

---

## Earlier versions

v3.0 introduced natural language interaction and progressive personalization.
v2.0 was the initial release with 22 capabilities, 11 skills, and structured frameworks.

Full history available in the [repository](https://github.com/chieflatif/SalesSidekick-Claude-CoWork).

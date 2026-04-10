# Changelog

All notable changes to SalesSidekick are documented here.

---

## v4.2.1 — Intelligence Trail Hardening (2026-04-09)

**Conversations now leave a recoverable trail without casually overwriting canonical business records.**

Full release notes: `docs/release-notes-v4.2.1.md`

### New

**Intelligence Trail (`data/trail/`)**
- Substantive conversation context can now be captured passively during the conversation, instead of relying on users to remember to run a command before starting a new chat
- Trail entries preserve what was discussed, which entities were referenced, what decisions were made, and what intelligence was filed
- Morning briefing now summarizes recent trail activity in "What You've Been Working On" so users can pick up where they left off

### Improved

**Conservative capture policy**
- Passive capture now uses an explicit decision policy: `NOOP`, `TRAIL_ONLY`, `TRAIL_PLUS_ENTITY_UPDATE`, or `ASK_USER`
- Trail-only is the default when the intelligence is conversational, inferred, ambiguous, or not ready for canonical records
- Entity files are updated only for explicit, high-confidence facts tied to existing company/contact/deal records
- Passive trail capture uses a quiet "Captured:" one-liner; command-based writes still use the full FILES UPDATED block

**Upgrade and layout migration**
- New workspaces include `data/trail/` during setup
- Existing `v4.2.0` workspaces get a non-destructive layout migration from `workspace_layout_version: 1` to `2`
- The upgrade audit now treats missing `data/trail/` as layout drift and repairs it before declaring the workspace current

### Upgrading from v4.2.0
Upload the new zip and open your workspace. SalesSidekick creates `data/trail/`, updates workspace layout metadata, and leaves existing deals, contacts, call notes, research docs, patterns, and custom skills untouched.

---

## v4.2.0 — Data Integrity & Evidence Provenance (2026-04-09)

**Every write is verified. Every fact has a source.**

### New

**Write-verification read-back**
- Every data write now includes a verification step: after writing the entity file and updating the index, the system reads back the index row and confirms key fields match
- If mismatch detected: retries the index update once. If retry fails: logs VERIFY_FAIL and informs the user immediately
- Catches partial writes at the moment they happen instead of waiting for the weekly audit

**Evidence provenance on company records**
- Company records now track `evidence_sources` — a structured log of what source informed each key field (industry, size, tech stack)
- Each entry records: the field, the value, the evidence grade (verified/estimated/hypothesis), the source, and the date
- Downstream capabilities can check freshness and grade before citing company data
- Fields populated before provenance tracking are treated as ungraded until the next research run

### Improved

**Onboarding directory structure**
- `data/research/` now included in the standard workspace layout created during onboarding
- Previously created on demand by the research command; now part of the initial setup

### Schema Changes
- Company schema: `_schema` 1 → 2. New field: `evidence_sources` (list, default empty)
- Migration is automatic and non-destructive. Existing company files get `evidence_sources: []` added
- No fields removed or renamed. Schema 2 is a strict superset of schema 1

### Upgrading from v4.1.x
Upload the new zip and open your workspace. The system detects the schema change and migrates your company files automatically. Nothing is lost or overwritten.

---

## v4.1.1 — Upgrade Intelligence Hardening (2026-04-08)

**Safer upgrades for older workspaces and older installs.**

Full release notes: `docs/release-notes-v4.1.1.md`

### Improved

**Automatic upgrade detection**
- The plugin now classifies the user's environment before claiming an upgrade is complete
- Detects legacy identity-only installs, missing workspace config, stale schema metadata, layout drift, and normal version upgrades
- Falls back conservatively when version metadata is missing instead of assuming a newer layout

**Post-upgrade workspace audit**
- Upgrade flow now checks Global CLAUDE markers, Project CLAUDE presence, required directories, index presence, and version metadata
- If the workspace is partial or stale, SalesSidekick now repairs the structure before returning to normal work
- Upgrade success is no longer declared until the workspace passes a basic health scan

**Version tracking**
- Project CLAUDE metadata now tracks workspace layout state and upgrade audit state more explicitly
- Migration history is recorded so future releases can reason about older environments more safely

**Release docs**
- Backfilled the staged `/research` Platform Document upgrade into the release docs
- Installation and upgrade docs now explain that the plugin automatically inspects and upgrades older local workspaces after a new zip is installed

---

## v4.1.0 — Compounding Intelligence (2026-03-27)

**Your AI gets smarter the more you use it.**

### New

**Platform Document Research (`/research`)**
- `/research` now runs as a staged intelligence pipeline instead of a single-pass summary
- Four stages: Collect, Extract, Prism, Assemble
- Produces an 11-section Platform Document with evidence grading on every claim
- Saves reusable account intelligence to `data/research/{company-slug}-platform.md`
- Downstream capabilities now have a richer intelligence layer for prep, strategy, outreach, business cases, and contextual email

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

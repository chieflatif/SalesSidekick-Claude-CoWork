# Changelog

All notable changes to SalesSidekick are documented here.

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

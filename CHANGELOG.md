# Changelog

All notable changes to SalesSidekick are documented here.

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

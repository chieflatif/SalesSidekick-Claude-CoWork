# Changelog

All notable changes to SalesSidekick are documented here.

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

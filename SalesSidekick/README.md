# SalesSidekick — Your AI Sales Partner

**SalesSidekick is an AI sales partner that learns how you sell and gets better the more you use it.** Process your calls, prep for meetings, manage your pipeline, build competitive playbooks, write outreach — just tell it what you need in plain language.

It's not a prompt library or a template collection. It's a working system that thinks about your deals, remembers your conversations, and drives your territory forward while you focus on building relationships.

---

## What It Feels Like

You talk to it like a colleague who's always on top of your book of business.

**Start your day:**
> "What's on my plate today?"
>
> SalesSidekick shows your tasks grouped by account, deals needing attention, and meetings to prep for.

**After a call:**
> "Just got off a call with Acme. Here's the transcript."
>
> It scores the deal on MEDDPICC, extracts action items, writes a follow-up email, flags competitive intel, and offers to save everything to your pipeline.

**Before a meeting:**
> "Get me ready for my Globex meeting at 2pm."
>
> You get an intelligence brief with talking points, MEDDPICC gaps to probe, and stakeholder context.

**When you're stuck:**
> "This deal is stalling. How should I approach it?"
>
> Five-lens strategic analysis with three distinct paths forward — each evidence-graded so you know what's fact and what's assumption.

---

## What It Does

**Daily operations** — Morning briefings, end-of-day wrap-ups, weekly pipeline reviews. Everything organized by account so nothing falls through.

**Call processing** — Paste a transcript and get MEDDPICC scoring, tasks, coaching feedback, a follow-up email, risk signals, and competitive intel. All written to your pipeline automatically.

**Deal strategy** — Five-Lens Prism analysis, competitive displacement playbooks, business cases. Real frameworks, not generic advice.

**Pipeline management** — Territory health checks, forecast updates, whitespace analysis. MEDDPICC confidence scoring across your entire book.

**Content creation** — Prospecting emails, follow-ups, LinkedIn posts, presentations. All in your voice, backed by account intelligence.

**Account research** — Deep company research with structured intel briefs. Evidence-graded so you know what's verified and what's estimated.

**Progressive personalization** — It starts working immediately. The more you use it, the sharper it gets — it learns your competitors, your communication style, your deal patterns, and your territory dynamics through natural use.

---

## Getting Started

1. **Install** — Download the `.zip` from [GitHub releases](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases), upload to Claude Desktop (Settings > Plugins > Add Plugin)
2. **Create a workspace** — Make a folder called `SalesSidekick` in your Documents. Create a Project in Claude Desktop pointing at it.
3. **Start talking** — Open a conversation in your project and say hi. SalesSidekick introduces itself, asks for the basics, researches your company, and sets everything up automatically.

That's it. You're working in under 5 minutes. No external accounts or services required.

See [QUICK-START.md](QUICK-START.md) for the full onboarding guide.

---

## How It Learns

SalesSidekick uses progressive personalization — it gets smarter through use, not through a long configuration process.

**First conversation:** It asks for three things (name, company, what you sell) and starts working.

**Through use:** It picks up your competitors from call transcripts, learns your communication style from email edits, infers your ICP from the companies you work with, and builds context from every interaction.

**Data builds as you go:** The first time you process a call, it saves the intelligence. The first time you add a deal, it starts tracking. Your workspace builds itself naturally — no upfront database creation needed.

**Optional deep personalization:** When you're ready to go all-in, ask for a deep personalization session (~15 minutes). It captures the things organic use can't easily infer — structured competitive battlecards, calibrated brand voice from writing samples, explicit quota and territory numbers.

---

## Integrations

| Integration | Required? | What It Enables |
|-------------|-----------|----------------|
| Gmail | No | Send emails directly instead of copy-paste |
| Google Calendar | No | Meeting-aware briefings and automatic prep triggers |
| Google Drive | No | Auto-discover call transcripts and store documents |
| Notion | No | Structured database views for power users and cross-device access |

Everything works without any connectors. Each one adds convenience. Missing one changes behavior, never breaks it. See [CONNECTORS.md](CONNECTORS.md) for setup details.

---

## Key Frameworks

SalesSidekick isn't prompt tricks — it's built on real sales methodologies:

- **MEDDPICC** — 8-element qualification scoring (Red/Yellow/Green) with discovery questions
- **Five-Lens Prism** — Deal analysis through Stakeholder Psychology, Political Capital, Competitive Dynamics, Hidden Leverage, and Temporal Dynamics
- **Three Paths** — Every strategy offers Velocity (Strike), Diagnostic (Go Deep), and Protective (Pause) options
- **Evidence Grading** — Every claim tagged Verified, Estimated, or Hypothesis. The 50% Rule warns you when outputs are assumption-heavy.
- **6-Output Framework** — Call processing that produces MEDDPICC scoring, tasks, coaching, follow-up email, risk signals, and competitive intel in one pass

---

<details>
<summary><strong>Architecture (for technical browsers)</strong></summary>

### How It Works

SalesSidekick is a Claude Cowork plugin — an all-markdown system that gives Claude deep sales domain knowledge, structured workflows, and local-first data persistence.

**Four-layer architecture:**
- **Global CLAUDE.md** — Your stable identity (name, company, product, style). Loads before every session, everywhere.
- **Project CLAUDE.md** — Workspace config (data schemas, signal rules, agent schedules). Loads in your SalesSidekick project.
- **Plugin brain** — Frameworks, commandments, intent engine, capability definitions. Ships with the plugin.
- **Skills** — Domain expertise (MEDDPICC, deal strategy, call processing, etc.). Fire automatically based on intent.

**Local-first data:**
All your deals, contacts, call notes, and tasks are stored as structured markdown files in your workspace folder. An index file provides fast querying. Background agents run signal intelligence (12 risk patterns) and generate views (forecast, pipeline, tasks) on a schedule.

**Signal intelligence:** The system monitors your deals for stage stalls, missing decision-makers, forecast conflicts, qualification gaps, and 8 other risk patterns. Findings surface in your morning briefing.

**Custom skills:** Customize how any capability works by placing a modified skill file in your workspace. Your customizations survive plugin updates.

**Upgrade-safe:** Plugin = the brain (replaced on update). Workspace = your data (never touched). Identity = your settings (preserved automatically).

</details>

---

## License

Personal Use License — you may use and modify for personal/internal business use. You may not distribute or sell. See [LICENSE](LICENSE) for full terms.

---

## Author

Built by [Pipeline Rebel (Latif Horst)](https://github.com/chieflatif).

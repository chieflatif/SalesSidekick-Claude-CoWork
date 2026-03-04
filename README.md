# SalesSidekick — Your AI Chief of Staff for Enterprise Sales

**SalesSidekick is a self-personalizing AI sales operating system for Claude.** Run `/setup` once and it becomes YOUR sales AI — it knows what you sell, who you compete with, how you communicate, and where every deal stands.

Not another AI template collection. Not a prompt library. A fully operational system that processes your calls, manages your pipeline, generates your decks, and drives your deals forward while you focus on building relationships.

---

## Why SalesSidekick?

| Problem | SalesSidekick Solution |
|---------|----------------------|
| Post-call intelligence gets lost in notes | `/closeout` processes every call through a 6-Output Framework — MEDDPICC scoring, tasks, coaching, follow-up email, risk signals, competitive intel |
| Deal strategy is gut-feel | `/strategy` analyzes deals through a Five-Lens Prism with evidence-graded Three Paths recommendations |
| CRM updates take 30+ minutes | `/forecast-update` generates paste-ready CRM output in seconds |
| Outreach emails sound generic | `/outreach` uses your brand voice, account intelligence, and Financial/Technical/Strategic angles |
| Presentation prep takes hours | `/deck` auto-selects from 5 templates (8 slides each) based on deal stage |
| Pipeline reviews are chaos | `/pipeline` produces a weighted health view with MEDDPICC confidence scores |

---

## What You Get

- **22 Commands** — Daily ops, call processing, deal strategy, territory management, communication, account management, system utilities
- **11 Skills** — Reference frameworks that fire automatically (MEDDPICC, deal strategy, call processing, brand voice, competitive battlecards, and more)
- **6 Notion Databases** — Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts (70 fields total, auto-created during setup)
- **Evidence Grading** — Every claim tagged Verified, Estimated, or Hypothesis. If >50% is unverified, you get warned.
- **Graceful Degradation** — Works with zero optional connectors. Missing connectors change behavior, never break it.
- **Self-Personalization** — `/setup` captures your identity, researches your company, builds competitive battlecards, calibrates your voice, and configures your databases in ~45 minutes

---

## Quick Start

```
1. Install SalesSidekick (see Installation below)
2. Run /setup — the personalization wizard (~45 min, one time)
3. Try /today for your morning briefing
4. Try /prep [Company] before your next meeting
5. Try /closeout after your next call
```

See [QUICK-START.md](QUICK-START.md) for the onboarding guide.

---

## Installation

### Option 1: GitHub Download + Claude Cowork Upload

```bash
git clone https://github.com/chieflatif/SalesSidekick-Claude-CoWork.git
```

Then upload the folder to Claude Cowork:
1. Open Claude Cowork
2. Go to Settings > Plugins
3. Upload the `SalesSidekick-Claude-CoWork` folder
4. Run `/setup` to personalize

### Option 2: Claude Code CLI

```bash
claude plugin install chieflatif/SalesSidekick-Claude-CoWork
```

### Option 3: Enterprise Deployment

See [INSTALLATION-GUIDE.md](INSTALLATION-GUIDE.md) for enterprise private marketplace deployment and team-wide configuration.

---

## The 22 Commands

### Daily Ops
| Command | What It Does |
|---------|-------------|
| `/today` | Morning briefing — tasks, deals needing attention, meeting prep |
| `/end-of-day` | Evening wrap — completed tasks, overdue flags, tomorrow preview |
| `/weekly` | Pipeline movement, MEDDPICC changes, manager 1:1 format |

### Call Processing
| Command | What It Does |
|---------|-------------|
| `/closeout` | 6-Output Framework: MEDDPICC, tasks, coaching, email, risks, competitive intel |

### Meeting Prep
| Command | What It Does |
|---------|-------------|
| `/prep [Company]` | Pre-meeting intelligence brief with talking points and gaps to probe |

### Deal Strategy
| Command | What It Does |
|---------|-------------|
| `/strategy [Company]` | Five-Lens Prism analysis with Three Paths recommendation |
| `/battle [Company]` | Competitive displacement analysis with win probability |
| `/pov [Company]` | Point of View document — 5-Component Model |

### Territory Management
| Command | What It Does |
|---------|-------------|
| `/pipeline` | Weighted pipeline health with MEDDPICC confidence |
| `/forecast-update` | CRM paste-ready forecast output |
| `/whitespace` | Product penetration analysis across territory |

### Intelligence
| Command | What It Does |
|---------|-------------|
| `/research [Company]` | Deep company research — financials, tech stack, leadership, ICP fit |

### Communication
| Command | What It Does |
|---------|-------------|
| `/outreach [Company]` | Prospecting email — Financial/Technical/Strategic angles |
| `/email` | Quick contextual email for existing relationships |
| `/draft-post` | LinkedIn post — 3-Type Framework with pre-publish checklist |
| `/deck [Company]` | Presentation — 5 templates, .pptx or Gamma |

### Account Management
| Command | What It Does |
|---------|-------------|
| `/add-company` | Guided company record creation with qualification |
| `/add-deal` | Deal record creation linked to company + contacts |
| `/coaching` | Call pattern analysis across logged calls |

### System
| Command | What It Does |
|---------|-------------|
| `/setup` | First-run personalization wizard |
| `/audit` | 7-question adversarial integrity check |
| `/skill-builder` | Create custom commands that follow the 10 Commandments |

---

## How It's Different

SalesSidekick is NOT a generic AI sales assistant. Here's what makes it different:

**Self-Personalizing.** It doesn't just use templates — it rewrites itself for you. After `/setup`, four skill files are regenerated with YOUR company intel, YOUR competitive landscape, YOUR brand voice, and YOUR professional profile.

**Evidence-Graded.** Every output tags claims as Verified, Estimated, or Hypothesis. The 50% Rule warns you when outputs are assumption-heavy. No AI hallucination disguised as fact.

**Framework-Driven.** MEDDPICC qualification, Five-Lens Prism strategy, Three Paths recommendations, Context Wedge positioning, 6-Output Framework call processing. Real methodologies, not prompt tricks.

**Notion-Native.** 6 databases, 70 fields, full read/write. Your data lives in Notion where you control it. CRM integration is export-first — no permission dependencies.

**Graceful Degradation.** Works with just Notion. Works with zero connectors. Every command adapts to what's available.

---

## Connectors

| Connector | Required? | What It Enables |
|-----------|-----------|----------------|
| **Notion** | Yes | 6 databases — your single source of truth |
| Gmail | No | Direct email sending from /outreach, /email, /closeout |
| Google Calendar | No | Meeting-aware context in /today, /end-of-day, and /prep |
| Google Drive | No | Document storage and auto-discovery of call transcripts for /closeout |
| Gamma | No | Alternative web-based presentation path for /deck |

See [CONNECTORS.md](CONNECTORS.md) for setup instructions and graceful degradation details.

---

## Architecture

```
SalesSidekick/
├── CLAUDE.md              # Brain — identity, commandments, frameworks, config
├── CONNECTORS.md          # Connector setup and degradation reference
├── README.md              # This file
├── INSTALLATION-GUIDE.md  # Detailed setup for all installation methods
├── QUICK-START.md         # 5-minute onboarding guide
├── CHANGELOG.md           # Version history
├── LICENSE                # Personal Use License
├── .mcp.json              # Notion MCP server configuration
├── .claude-plugin/
│   └── plugin.json        # Plugin manifest and metadata
├── commands/              # 22 command files (.md)
│   ├── today.md
│   ├── closeout.md
│   ├── strategy.md
│   └── ... (19 more)
└── skills/                # 11 skill directories
    ├── meddpicc/SKILL.md
    ├── deal-strategy/SKILL.md
    ├── brand-voice/SKILL.md
    └── ... (8 more)
```

---

## License

Personal Use License — you may use and modify for personal/internal business use. You may not distribute or sell. See [LICENSE](LICENSE) for full terms.

---

## Author

Built by [Pipeline Rebel (Latif Horst)](https://github.com/chieflatif).

# SalesSidekick — Quick Start

Get up and running in under 2 minutes.

---

## Step 1: Install the Plugin

1. Download `SalesSidekick.zip` from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases)
2. Open Claude Desktop > **Cowork** > **Customize** > **Plugins**
3. Click **+** > **Add personal plugin** > select the `.zip` file

---

## Step 2: Connect Notion

1. Create a Notion integration at [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Copy your API key (starts with `ntn_`)
3. Replace `{{NOTION_API_KEY}}` in the plugin's `.mcp.json` file with your actual key

---

## Step 3: Start Talking

Open a Cowork conversation and just say hi. SalesSidekick will introduce itself, ask for the basics (your name, company, and what you sell), and offer to research your company right away.

From there, just tell it what you need:

- **"What's on my plate today?"** — morning briefing with tasks, deals, and meetings
- **"I just got off a call with Acme"** — processes your call through MEDDPICC, extracts tasks, writes a follow-up
- **"Get me ready for my Globex meeting"** — intel brief with talking points and gaps to probe
- **"How should I approach the Initech deal?"** — strategic analysis with three paths forward
- **"Write a follow-up to Sarah at Acme"** — contextual email in your voice
- **"How's my pipeline?"** — territory health check with deal confidence scoring

---

## Tips

**Create a working folder** for file output (presentations, documents):
```
~/Documents/SalesSidekick/
```
When starting a conversation, click **"Work in a folder"** at the bottom left and select this folder.

**Global settings** for quality rules: Go to **Settings > Cowork** and paste the quality rules from [INSTALLATION-GUIDE.md](INSTALLATION-GUIDE.md). This sets a quality floor across all your AI interactions — anti-hallucination, anti-slop, evidence grading.

**Go deeper when you're ready:** After a few sessions, ask "let's do a deep personalization" to build competitive battlecards, calibrate your brand voice, and capture territory details (~15 minutes). This is optional — the system already learns through use.

**Optional integrations:** Add Gmail (send emails directly), Calendar (meeting-aware briefings), Drive (auto-discover transcripts), or Gamma (web presentations). See [CONNECTORS.md](CONNECTORS.md) for setup.

---

## Need Help?

- Say **"what can you do?"** and SalesSidekick will describe its capabilities conversationally
- See [CONNECTORS.md](CONNECTORS.md) for integration setup and troubleshooting
- See [INSTALLATION-GUIDE.md](INSTALLATION-GUIDE.md) for detailed setup instructions and enterprise deployment
- Full details in [README.md](README.md)

# SalesSidekick — Installation Guide

## Prerequisites

- Claude Desktop with Cowork enabled
- A Notion account (free or paid)

---

## Installation Methods

### Method 1: Download ZIP + Upload to Cowork (Recommended)

**Step 1: Download the plugin**

Download the `.zip` file from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases).

Or clone and zip manually:
```bash
git clone https://github.com/chieflatif/SalesSidekick-Claude-CoWork.git
cd SalesSidekick-Claude-CoWork
zip -r SalesSidekick.zip . -x ".git/*" ".gitignore"
```

**Step 2: Upload to Claude Cowork**

1. Open Claude Desktop and switch to **Cowork**
2. Click **Customize** in the left sidebar
3. Click **Plugins**, then click the **+** (Add) button
4. Select **Add personal plugin**
5. Choose the `SalesSidekick.zip` file you downloaded
6. Confirm the plugin loads — you should see "SalesSidekick" in your plugins list

**Step 3: Configure Notion**

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New Integration"
3. Name it "SalesSidekick"
4. Select your workspace
5. Under "Capabilities", ensure "Read content" and "Insert content" are enabled
6. Copy the Internal Integration Secret (starts with `ntn_`)

> **Important:** SalesSidekick's `.mcp.json` contains a `{{NOTION_API_KEY}}` placeholder. You need to replace this with your actual key. After uploading the plugin, locate the `.mcp.json` file in the plugin's directory, open it in a text editor, find `{{NOTION_API_KEY}}`, and replace it with your key. The line should look like: `"Bearer ntn_your_actual_key_here"` (keep the surrounding quotes intact).

**Step 4: Create a working folder**

Create a folder on your machine for SalesSidekick output (presentations, documents, exports):

```
~/Documents/SalesSidekick/
```

When you start a Cowork conversation, click **"Work in a folder"** at the bottom left and select this folder. Your persistent data (deals, contacts, tasks) lives in Notion — this folder is for file output like presentations and documents.

**Step 5: Configure Cowork Global Settings**

Go to **Claude Desktop > Settings > Cowork** and paste into the custom instructions field.

**Why this matters:** The global settings field loads before any plugin fires — it's Layer 1 of a cascading architecture: Global Settings → Plugin CLAUDE.md → Skills → Capabilities. Anything here is available to every session, zero API calls. This is where your stable identity lives: who you are, what you sell, who you compete against. It means every session knows you from the first message, even before touching Notion.

**Two-layer architecture:**
- **Global settings** = stable context that rarely changes (identity, product, ICP, competitors, communication style)
- **Notion** = refreshable operational content (battlecards updated monthly, case studies as new wins happen, all deal data)

**Getting your personalized block:** SalesSidekick generates a personalized global settings block at the end of your first conversation — after it's asked the basics and researched your company and competitive landscape. That's what you'll paste here. It's much richer than a generic template because it's built from real research.

**Before your first conversation** (optional starter template — replace with personalized version after):

```
I am [Your Name], a [Your Title] at [Your Company] ([company website]).

What I sell: [one sentence on your product/service]
I sell to: [industry] companies, [size], focused on [use case]
Communication style: [formal / casual / mixed]. Sign-off: "[e.g., Best, or Cheers,]"

I use voice-to-text — interpret my intent, not my grammar.

QUALITY RULES — apply to everything you produce:

No hallucination. If you don't have a verified source, say "I don't know" or "I'd need to verify." Grade every factual claim: Verified (sourced), Estimated (calculated with stated assumptions), or Hypothesis (pattern-based guess).

No slop. Every sentence must earn its place. Match MY voice, not a corporate template. Before delivering any written content, ask: "Would this person actually write this?"

Do the research first. Before generating content about a company, person, deal, or topic — look it up. Check Notion. Search the web. Notion is my single source of truth for deal and account data.

Give me options, not decisions. Always present 2-3 paths with trade-offs. Never prescribe a single answer.

Respect my time. Lead with the answer, not the reasoning. Break complex work into 10-minute chunks.
```

**After your first conversation:** Replace the above with the personalized block SalesSidekick generates — it includes your confirmed competitors, differentiation angles, and communication style calibrated to you. That block is what makes every future session fully context-aware from message one.

**Step 6: Start a conversation**

Open a new Cowork conversation, select your SalesSidekick folder, and just start talking. SalesSidekick will check your Notion connection, ask the basics, research your company and competitive landscape automatically, then hand you a personalized global settings block to paste into Step 5.

From that point on, every session knows who you are before you say a word.

---

### Method 2: Claude Code CLI

If you use Claude Code (the CLI tool), you can install via a plugin marketplace:

```bash
/plugin marketplace add chieflatif/SalesSidekick-Claude-CoWork
/plugin install salessidekick@chieflatif-SalesSidekick-Claude-CoWork
```

Then configure Notion:
1. Create a Notion integration (see Method 1, Step 3)
2. Open `.mcp.json` and replace `{{NOTION_API_KEY}}` with your actual key (do NOT commit your real key to version control)

Start a conversation and SalesSidekick handles the rest.

---

### Method 3: Enterprise Deployment

For team-wide deployment, admins can upload the plugin ZIP to an organization-managed marketplace:

1. Go to **Organization settings > Plugins** in Claude Desktop
2. Click **Add plugins** > **Upload a file**
3. Upload the `SalesSidekick.zip` file
4. Set visibility to your sales team or organization

Each AE then installs from the org marketplace and starts a conversation to personalize.

See [enterprise-deployment.md](docs/enterprise-deployment.md) for detailed team configuration, pre-configured settings, and onboarding workflows.

---

## Optional Integrations

After connecting Notion, you can add optional integrations for additional convenience:

| Integration | How to Connect | What It Adds |
|-------------|---------------|-------------|
| Gmail | Claude Cowork > Customize > Connectors > Gmail | Send emails directly instead of copy-paste |
| Google Calendar | Claude Cowork > Customize > Connectors > Calendar | Meeting-aware briefings and automatic meeting prep |
| Google Drive | Claude Cowork > Customize > Connectors > Drive | Auto-discover call transcripts and store documents |
| Gamma | Claude Cowork > Customize > Connectors > Gamma | Web-based presentations as alternative to .pptx |

All integrations are optional. Missing one changes behavior, never breaks it. See [CONNECTORS.md](CONNECTORS.md) for details.

---

## Verifying Your Installation

After your first few conversations, verify everything works:

| Test | What to Say | Expected Result |
|------|-------------|----------------|
| Basic function | "What's on my plate today?" | Shows your tasks and deals |
| Data persistence | "Tell me about [company you've discussed]" | Recalls company context from Notion |
| Call processing | "Here's a call transcript..." | Processes it, offers to save to databases |
| Strategy | "How should I approach [deal]?" | Five-Lens analysis with three paths |

If something isn't working, check [CONNECTORS.md](CONNECTORS.md) troubleshooting section.

---

## Updating

**Download a new version** of the `.zip` from the releases page and re-upload it through Cowork > Customize > Plugins. Your Notion databases and data are preserved — only capability and skill definitions update.

**Enterprise users:** Admin uploads the new `.zip` to the organization marketplace. AEs update through their normal plugin management flow.

---

## Uninstalling

1. Remove the plugin from Cowork > Customize > Plugins
2. Your Notion databases remain — SalesSidekick doesn't delete your data
3. If you want to clean up Notion, manually delete the 6 SalesSidekick databases

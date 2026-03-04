# SalesSidekick — Installation Guide

## Prerequisites

- Claude Desktop with Cowork enabled
- A Notion account (free or paid) for database creation
- ~45 minutes for initial `/setup` personalization

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

> **Important:** SalesSidekick's `.mcp.json` contains a `{{NOTION_API_KEY}}` placeholder. You need to replace this with your actual key before `/setup` can connect to Notion. After uploading the plugin, locate the `.mcp.json` file in the plugin's directory, open it in a text editor, find `{{NOTION_API_KEY}}`, and replace it with your key. The line should look like: `"Bearer ntn_your_actual_key_here"` (keep the surrounding quotes intact).

**Step 4: Create a working folder**

Create a folder on your machine for SalesSidekick output (presentations, documents, exports):

```
~/Documents/SalesSidekick/
```

When you start a Cowork conversation, click **"Work in a folder"** at the bottom left and select this folder. Commands like `/deck` (presentations) and `/pov` (documents) will save generated files here. Your persistent data (deals, contacts, tasks) still lives in Notion.

**Step 5: Run /setup**

Start a new conversation in Cowork, select your SalesSidekick folder, and type `/salessidekick:setup`. The personalization wizard will guide you through 7 phases (~45 minutes total):
1. Identity Setup
2. Company Intelligence (AI-assisted research)
3. Competitive Landscape (AI-assisted battlecard generation)
4. Brand Voice (communication calibration)
5. Presentation Brand (brand token capture)
6. Connector Setup (database creation + connector detection)
7. Verification + Audit

After setup, you're ready. Try `/salessidekick:today` for your first morning briefing.

> **Note on command names:** Plugin commands are namespaced. All SalesSidekick commands are prefixed with `salessidekick:` (e.g., `/salessidekick:today`, `/salessidekick:prep`). Claude may also invoke them automatically based on context.

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

Run `/salessidekick:setup` to personalize.

---

### Method 3: Enterprise Deployment

For team-wide deployment, admins can upload the plugin ZIP to an organization-managed marketplace:

1. Go to **Organization settings > Plugins** in Claude Desktop
2. Click **Add plugins** > **Upload a file**
3. Upload the `SalesSidekick.zip` file
4. Set visibility to your sales team or organization

Each AE then installs from the org marketplace and runs `/salessidekick:setup` to personalize.

See [enterprise-deployment.md](docs/enterprise-deployment.md) for detailed team configuration, pre-configured settings, and onboarding workflows.

---

## Optional Connectors

After initial setup with Notion, you can add optional connectors for additional capabilities:

| Connector | How to Connect | What It Adds |
|-----------|---------------|-------------|
| Gmail | Claude Cowork > Customize > Connectors > Gmail | Direct email sending from /outreach, /email, /closeout |
| Google Calendar | Claude Cowork > Customize > Connectors > Calendar | Meeting-aware context in /today, /end-of-day, /prep |
| Google Drive | Claude Cowork > Customize > Connectors > Drive | Auto-discovery of call transcripts for /closeout |
| Gamma | Claude Cowork > Customize > Connectors > Gamma | Web-based presentations as alternative to .pptx |

All connectors are optional. Missing connectors change behavior, never break it. See [CONNECTORS.md](CONNECTORS.md) for details.

---

## Verifying Your Installation

After `/salessidekick:setup` completes, verify everything works:

| Test | Command | Expected Result |
|------|---------|----------------|
| Notion read | `/salessidekick:today` | Shows your tasks and deals |
| Notion write | `/salessidekick:add-company` | Creates a test company record |
| Full pipeline | `/salessidekick:closeout` | Processes a call and writes to 4 databases |
| Strategy | `/salessidekick:strategy [Company]` | Produces Five-Lens analysis |

If any test fails, check [CONNECTORS.md](CONNECTORS.md) troubleshooting section.

---

## Updating

**Download a new version** of the `.zip` from the releases page and re-upload it through Cowork > Customize > Plugins. Your Notion databases and data are preserved — only command and skill definitions update.

**Enterprise users:** Admin uploads the new `.zip` to the organization marketplace. AEs update through their normal plugin management flow.

---

## Uninstalling

1. Remove the plugin from Cowork > Customize > Plugins
2. Your Notion databases remain — SalesSidekick doesn't delete your data
3. If you want to clean up Notion, manually delete the 6 SalesSidekick databases

# SalesSidekick — Installation Guide

## Prerequisites

- A Claude account with Cowork or Claude Code access
- A Notion account (free or paid) for database creation
- ~45 minutes for initial `/setup` personalization

---

## Installation Methods

### Method 1: GitHub Download + Claude Cowork Upload (Recommended)

**Step 1: Download the plugin**

```bash
git clone https://github.com/chieflatif/SalesSidekick-Claude-CoWork.git
```

Or download the ZIP from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases).

**Step 2: Upload to Claude Cowork**

1. Open Claude Cowork
2. Navigate to Settings > Plugins
3. Click "Upload Plugin" or "Add Custom Plugin"
4. Select the `SalesSidekick-Claude-CoWork` folder
5. Confirm the plugin loads — you should see "SalesSidekick" in your active plugins

**Step 3: Configure Notion**

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New Integration"
3. Name it "SalesSidekick"
4. Select your workspace
5. Copy the Internal Integration Secret (starts with `ntn_`)
6. Open the `.mcp.json` file in any text editor (it may be hidden — enable "show hidden files" in your file browser)
7. Find `{{NOTION_API_KEY}}` and replace it with your actual key, keeping the surrounding quotes intact. The line should look like: `"Bearer ntn_your_actual_key_here"`
8. Save the file

**Step 4: Run /setup**

Type `/setup` in your Claude conversation. The personalization wizard will guide you through 7 phases (~45 minutes total):
1. Identity Setup
2. Company Intelligence (AI-assisted research)
3. Competitive Landscape (AI-assisted battlecard generation)
4. Brand Voice (communication calibration)
5. Presentation Brand (brand token capture)
6. Connector Setup (database creation + connector detection)
7. Verification + Audit

After setup, you're ready. Try `/today` for your first morning briefing.

---

### Method 2: Claude Code CLI

```bash
claude plugin install chieflatif/SalesSidekick-Claude-CoWork
```

Then configure Notion:
1. Create a Notion integration (see Method 1, Step 3)
2. Open `.mcp.json` and replace `{{NOTION_API_KEY}}` with your actual key (do NOT commit your real key to version control)

Run `/setup` to personalize.

---

### Method 3: Enterprise Private Marketplace

For team-wide deployment:

**Step 1: Admin prepares the plugin**

1. Clone the repository
2. Optionally pre-configure shared settings (company name, product description, competitors) in CLAUDE.md
3. Publish to your organization's private plugin marketplace

**Step 2: Each AE installs and personalizes**

1. Install from the private marketplace
2. Configure their own Notion integration (each AE needs their own databases)
3. Run `/setup` — shared company settings are pre-filled, personal settings are collected

**Step 3: Ongoing management**

- Plugin updates: admin pushes new version to marketplace, AEs update
- AE offboarding: AE's Notion databases are theirs — no shared data to clean up
- New AE onboarding: install + `/setup` takes ~45 minutes

---

## Optional Connectors

After initial setup with Notion, you can add optional connectors for additional capabilities:

| Connector | How to Connect | What It Adds |
|-----------|---------------|-------------|
| Gmail | Claude Cowork Settings > Connectors > Gmail | Direct email sending from /outreach, /email, /closeout |
| Google Calendar | Claude Cowork Settings > Connectors > Calendar | Meeting-aware context in /today, /end-of-day, /prep |
| Google Drive | Claude Cowork Settings > Connectors > Drive | Auto-discovery of call transcripts for /closeout |
| Gamma | Claude Cowork Settings > Connectors > Gamma | Web-based presentations as alternative to .pptx |

All connectors are optional. Missing connectors change behavior, never break it. See [CONNECTORS.md](CONNECTORS.md) for details.

---

## Verifying Your Installation

After `/setup` completes, verify everything works:

| Test | Command | Expected Result |
|------|---------|----------------|
| Notion read | `/today` | Shows your tasks and deals |
| Notion write | `/add-company` | Creates a test company record |
| Full pipeline | `/closeout` | Processes a call and writes to 4 databases |
| Strategy | `/strategy [Company]` | Produces Five-Lens analysis |

If any test fails, check [CONNECTORS.md](CONNECTORS.md) troubleshooting section.

---

## Updating

**GitHub users:**
```bash
cd SalesSidekick-Claude-CoWork
git pull
```

Re-upload the updated folder to Claude Cowork. Your CLAUDE.md personalization and Notion databases are preserved — only command and skill definitions update.

**CLI users:**
```bash
claude plugin update salessidekick
```

**Enterprise users:** Admin pushes the updated version to the private marketplace. AEs update through their normal plugin management flow.

---

## Uninstalling

1. Remove the plugin from Claude Cowork Settings > Plugins
2. Your Notion databases remain — SalesSidekick doesn't delete your data
3. If you want to clean up Notion, manually delete the 6 SalesSidekick databases

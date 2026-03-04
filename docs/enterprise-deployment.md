# SalesSidekick — Enterprise Deployment Guide

Instructions for sales team leads and admins deploying SalesSidekick across a team.

---

## Overview

SalesSidekick is designed for individual AE use, but it deploys cleanly across teams. Each AE gets their own personalized instance with their own Notion databases. No shared data, no permission conflicts, no admin bottleneck after initial setup.

**Deployment model:** Admin prepares and distributes the plugin. Each AE personalizes independently.

---

## Step 1: Admin Preparation

### 1.1 Get the Plugin Files

Download the `.zip` from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases) and unzip it, or clone the repository:

```bash
git clone https://github.com/chieflatif/SalesSidekick-Claude-CoWork.git
```

### 1.2 Pre-Configure Shared Settings (Optional)

Open `CLAUDE.md` and pre-fill company-wide settings. This saves each AE from entering the same information during `/setup`:

**Section 1 (Identity) — shared fields:**
- `{{COMPANY}}` — Your company name
- `{{COMPANY_URL}}` — Company website
- `{{PRODUCT_DESCRIPTION}}` — What you sell
- `{{PRIMARY_PRODUCT}}` — Main product name
- `{{SECONDARY_PRODUCTS}}` — Additional products

**Section 1 — competition (if standardized across team):**
- `{{TOP_COMPETITOR_1}}`, `{{TOP_COMPETITOR_2}}`, `{{TOP_COMPETITOR_3}}`

**Section 13 (Personalization Variables) — sales process:**
- `{{CRM_SYSTEM}}` — Salesforce, HubSpot, etc.
- `{{DEAL_STAGES}}` — Your standard stage names
- `{{FISCAL_YEAR_START}}` — Month fiscal year begins

Leave AE-specific fields empty (name, title, territory, quota, communication style). These are collected per-AE during `/setup`.

### 1.3 Pre-Generate Shared Skills (Optional)

For consistency across the team, you can pre-generate Tier 3 skills:

- **company-intel** — Run `/setup` through Phase 2 (Company Intelligence) once and copy the generated `skills/company-intel/SKILL.md` into the distribution package. Every AE gets the same company research baseline.
- **battlecards** — Run `/setup` through Phase 3 (Competitive Landscape) and copy `skills/battlecards/SKILL.md`. Standardizes competitive positioning.

Leave **brand-voice** and **profile** empty — these must be individual per AE.

### 1.4 Configure Notion API Key Handling

**Option A: Each AE manages their own key**
- Leave `.mcp.json` with the `{{NOTION_API_KEY}}` placeholder
- Each AE creates their own Notion integration during `/setup`
- Simplest approach, recommended for most teams

**Option B: Admin-provisioned Notion workspace**
- Create a shared Notion workspace for the sales team
- Create one integration with access to the workspace
- Distribute the API key separately (not in the plugin files)
- Each AE still gets their own databases within the shared workspace

### 1.5 Package and Publish

**Package as ZIP:**

```bash
cd SalesSidekick-Claude-CoWork
zip -r SalesSidekick.zip . -x ".git/*" ".gitignore"
```

**Upload to organization marketplace:**

1. Open Claude Desktop > **Organization settings** > **Plugins**
2. Click **Add plugins** > **Upload a file**
3. Upload the `SalesSidekick.zip` file
4. Set visibility to your sales team or organization
5. Add installation notes (e.g., "Run /salessidekick:setup after installing — takes ~45 minutes")

---

## Step 2: AE Installation and Setup

Send this to each AE:

---

**Getting Started with SalesSidekick**

1. Install SalesSidekick from your organization's plugin marketplace in Claude Desktop
2. Create a Notion integration at [notion.so/my-integrations](https://www.notion.so/my-integrations):
   - Name it "SalesSidekick"
   - Select your workspace
   - Copy the API key (starts with `ntn_`)
3. Replace `{{NOTION_API_KEY}}` in the plugin's `.mcp.json` with your key
4. Run `/salessidekick:setup` in a Cowork conversation
5. The setup wizard will guide you through 7 phases (~45 minutes, but some company info may be pre-filled)
6. After setup, try `/salessidekick:today` for your first morning briefing

---

## Step 3: Team Configuration Patterns

### Standard Team Setup

| Setting | Pre-Configured by Admin | Set by Each AE |
|---------|------------------------|----------------|
| Company name, URL, product | Yes | No |
| Competitors | Yes (if standardized) | Override if needed |
| CRM system, deal stages | Yes | No |
| AE name, title, territory | No | Yes |
| Quota, region | No | Yes |
| Communication style, email sign-off | No | Yes |
| Brand voice calibration | No | Yes |
| Notion databases | No | Yes (auto-created) |

### Manager Visibility

SalesSidekick does not provide cross-AE dashboards. Each AE's data lives in their own Notion databases. For manager visibility:

- **Weekly roll-ups:** Each AE runs `/weekly` and shares the output in your team channel or 1:1 doc
- **Pipeline reviews:** Each AE runs `/pipeline` and screen-shares or pastes the output
- **Forecast updates:** Each AE runs `/forecast-update` and pastes into CRM — manager sees CRM as usual

This is intentional. SalesSidekick is an AE productivity tool, not a management surveillance tool. Trust your reps.

---

## Step 4: Ongoing Management

### Plugin Updates

When a new version is available:

1. Admin downloads the latest version from GitHub
2. Re-applies any pre-configured settings (company name, competitors, etc.)
3. Re-packages as `.zip` and uploads to the organization marketplace
4. AEs update through their normal plugin management flow

**What updates preserve:**
- AE's personalized CLAUDE.md (identity, variables, calibration data)
- All Notion databases and data
- Generated Tier 3 skill files (profile, brand-voice, company-intel, battlecards)

**What updates may change:**
- Command files (new features, bug fixes)
- Universal skill files (Tier 1 — meddpicc, deal-strategy, etc.)
- System configuration

### AE Offboarding

1. AE removes the plugin from their Claude Cowork settings
2. AE's Notion databases remain — they own their data
3. No shared data to clean up, no admin action required

### New AE Onboarding

1. New AE installs from the private marketplace
2. Runs `/setup` — pre-configured company settings are already in place
3. Personalizes their individual settings (~30 minutes with pre-configured fields)
4. Ready to sell

---

## Troubleshooting

**AE can't find the plugin:**
- Verify the plugin is published to the correct team/org scope
- Check that the AE has marketplace access permissions

**Notion databases not creating:**
- Each AE needs their own Notion integration with workspace access
- The integration must have "Insert content" and "Read content" capabilities
- Run `/setup` again — it will retry database creation

**Pre-configured settings not appearing:**
- Verify CLAUDE.md was saved correctly before publishing
- Check that template variable syntax is exact: `{{VARIABLE_NAME}}`
- AE can manually enter any missing values during `/setup`

**Different teams need different competitors/products:**
- Create separate plugin distributions per team
- Or leave competitor fields empty and let each AE configure during `/setup`

---

## Security Considerations

- **No shared data:** Each AE's Notion databases are independent. No cross-AE data access.
- **API keys:** Notion API keys should never be committed to version control or shared in plain text. Each AE manages their own key.
- **No external data transmission:** SalesSidekick does not send data to any external service beyond the configured connectors (Notion, Gmail, Google Calendar, Google Drive, Gamma). All connectors use the AE's own authenticated accounts.
- **Plugin files:** The plugin contains no executable code — only markdown files, a JSON manifest, and a JSON MCP configuration. All logic runs through Claude's native capabilities.

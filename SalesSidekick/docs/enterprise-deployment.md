# SalesSidekick — Enterprise Deployment Guide

Instructions for sales team leads and admins deploying SalesSidekick across a team.

---

## Overview

SalesSidekick is designed for individual AE use, but it deploys cleanly across teams. Each AE gets their own workspace with their own data. No shared databases, no permission conflicts, no admin bottleneck after initial setup.

**Deployment model:** Admin distributes the plugin. Each AE sets up their own workspace and personalizes independently.

---

## Step 1: Admin Preparation

### 1.1 Get the Plugin

Download `SalesSidekick.zip` from [pipelinerebel.com/download](https://pipelinerebel.com/download).

### 1.2 Pre-Configure Shared Settings (Optional)

You can pre-fill company-wide information in the plugin's `CLAUDE.md` before distributing. This saves each AE from entering the same basics:

- Company name, website, product description
- Top competitors (if standardized across the team)
- CRM system and deal stage names
- Fiscal year start

Leave AE-specific fields empty: name, title, territory, quota, communication style. These are captured per-AE during their first conversation.

### 1.3 Pre-Build Shared Knowledge Bases (Optional)

For consistency across the team, you can include pre-built knowledge bases in the plugin's `templates/` directory:

- **Company intel** — run a deep personalization session once, then distribute the generated `knowledge-bases/company-intel.md`
- **Battlecards** — generate competitive battlecards once, distribute as `knowledge-bases/battlecards.md`

These give every AE the same competitive positioning baseline from day one.

### 1.4 Publish to Organization Marketplace

1. Open Claude Desktop > **Organization settings** > **Plugins**
2. Click **Add plugins** > **Upload a file**
3. Upload the `SalesSidekick.zip` file
4. Set visibility to your sales team
5. Add installation notes: "After installing, create a SalesSidekick folder in your Documents, set up a Project in Claude Desktop pointing at it, and start talking. Takes 5 minutes."

---

## Step 2: AE Installation

Send this to each AE:

---

**Getting Started with SalesSidekick**

1. Install SalesSidekick from your organization's plugin marketplace in Claude Desktop
2. Create a folder called `SalesSidekick` in your Documents folder
3. In Claude Desktop, click the + next to Projects, name it "SalesSidekick", and point it at your new folder
4. Open a conversation in your project and say hello
5. SalesSidekick will ask for your name and a few basics — takes about 5 minutes
6. Everything works immediately. It gets smarter the more you use it

**Optional:** Connect Gmail (send emails directly), Google Calendar (meeting-aware briefings), or Google Drive (auto-find transcripts) in Claude Desktop Settings > Connectors.

---

## Step 3: Team Configuration

| Setting | Pre-configured by admin | Set by each AE |
|---------|------------------------|----------------|
| Company name, URL, product | Yes | No |
| Competitors | Yes (if standardized) | Override if needed |
| CRM system, deal stages | Yes | No |
| AE name, title, territory | No | Yes |
| Quota, region | No | Yes |
| Selling style (7 questions) | No | Yes |
| Communication style, email sign-off | No | Yes |
| Workspace data (deals, contacts) | No | Yes (builds through use) |

### Manager Visibility

SalesSidekick is an AE productivity tool, not a management surveillance tool. Each AE's data lives in their own local workspace. For manager visibility:

- **Weekly updates:** Each AE asks "give me my weekly summary" and shares the output
- **Pipeline reviews:** Each AE asks "show me my pipeline" and screen-shares or pastes
- **Forecast:** Each AE asks "what's my forecast" and pastes into CRM — manager sees CRM as usual

**Future:** Enterprise signal export — each AE's system generates a weekly signal summary file that can be shared to a team folder for manager aggregation. Architecture is designed but not yet implemented.

---

## Step 4: Ongoing Management

### Plugin Updates

1. Admin downloads the latest version from pipelinerebel.com/download
2. Re-applies any pre-configured settings if needed
3. Uploads to the organization marketplace
4. AEs update through Settings > Plugins (remove old, upload new)

**What updates preserve:** Each AE's workspace data (deals, contacts, tasks, call notes, custom skills, knowledge bases) and their identity settings. Only the plugin brain gets updated.

### New AE Onboarding

1. New AE installs from the private marketplace
2. Creates their workspace folder and Project
3. Opens a conversation — pre-configured company settings are already loaded
4. Personalizes their individual settings (5 minutes)
5. Ready to sell

### AE Offboarding

1. AE removes the plugin from Claude Desktop
2. AE's workspace folder remains on their machine — they own their data
3. No shared data to clean up, no admin action required

---

## Troubleshooting

**AE can't find the plugin:**
- Verify the plugin is published to the correct team scope
- Check that the AE has marketplace access permissions

**Settings not appearing for AE:**
- Verify CLAUDE.md was saved correctly before publishing
- AE can enter any missing information during their first conversation

**Different teams need different competitors/products:**
- Create separate plugin distributions per team
- Or leave competitor fields empty and let each AE's auto-research fill them in

---

## Security Considerations

- **No shared data:** Each AE's workspace is independent. No cross-AE data access.
- **All data is local:** Deal data, contacts, call notes live on the AE's machine. Nothing is sent to external services unless the AE explicitly connects a service (Gmail, Calendar, etc.)
- **No executable code:** The plugin contains only markdown files and JSON configuration. All logic runs through Claude's native capabilities.
- **Connectors use AE's own accounts:** Gmail, Calendar, and Drive all authenticate through the AE's personal account. No admin credentials needed.
- **Personal use license:** SalesSidekick is licensed to Rebel HQ Inc. AEs may use and modify for personal/internal business use. Distribution or resale is prohibited.

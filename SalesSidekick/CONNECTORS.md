# SalesSidekick — Integrations & Connectors

SalesSidekick works without any external services. All your data lives in your local workspace. Connectors add convenience — they let the system interact with your email, calendar, and other tools directly.

**Core principle: Missing connectors change behavior, not break it.** Every capability works without connectors. You lose convenience, not functionality.

---

## Available Connectors

All connectors are optional. Connect them through Claude Desktop Settings > Connectors.

| Connector | What it adds | Without it |
|-----------|-------------|------------|
| Gmail | Send emails directly from outreach and follow-ups | Emails are formatted for copy-paste |
| Google Calendar | Meeting-aware briefings and auto-prep | System asks about your meetings instead of reading them |
| Google Drive | Auto-discover call transcripts and documents | You paste transcripts when asked |
| Notion | Structured database views, cross-device access | Everything works locally — Notion adds a browsable database layer for power users |

---

## Setup Instructions

### Gmail

Enables direct email sending from outreach, follow-ups, and post-call communication.

**To connect:** Go to Claude Desktop Settings > Connectors > Gmail. Follow the sign-in flow.

**Without Gmail:** All email capabilities generate formatted text with a subject line and body. You paste it into your email client.

### Google Calendar

Enables meeting-aware context in morning briefings and meeting prep.

**To connect:** Go to Claude Desktop Settings > Connectors > Google Calendar.

**Without Calendar:** Morning briefings show tasks and deals but ask "What meetings do you have today?" Meeting prep asks "Who is the meeting with?"

### Google Drive

Enables auto-discovery of call transcripts and document storage.

**To connect:** Go to Claude Desktop Settings > Connectors > Google Drive.

**Without Drive:** Call processing asks you to paste the transcript. Everything else works normally.

### Notion

Adds structured database views for users who want to browse their data in a table format, run cross-device queries, or share data with other tools.

**This is entirely optional.** Your deals, contacts, tasks, and call notes already live in your local workspace. Notion adds a second copy in structured database form.

**To connect:**
1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New Integration" — name it "SalesSidekick", select your workspace
3. Under "Capabilities", make sure "Read content" and "Insert content" are enabled
4. Copy the integration secret (starts with `ntn_`)
5. In the plugin's `.mcp.json` file, replace `{{NOTION_API_KEY}}` with your actual key

**Without Notion:** Everything works. All data persists in your local workspace. You don't lose any capability — you only miss the structured database browsing and cross-device access that Notion provides.

---

## What Changes Without Each Connector

| Missing | What changes | Fallback |
|---------|-------------|----------|
| No Gmail | Can't send emails directly | Formatted copy-paste text |
| No Calendar | No meeting schedule awareness | Asks about your meetings |
| No Drive | Can't auto-find transcripts | Asks you to paste them |
| No Notion | No structured database views | Local workspace handles all data |
| No web search | Can't research companies | Relies on what you share |

---

## Web Search

Web search is a built-in Claude capability — not a connector. No setup required. Used for: company research, competitive landscape, account intelligence. If web search is unavailable, the system asks you to provide the information directly.

---

## CRM Integration

**Current approach: Export-First.**

Your SalesSidekick workspace is where you work. Your CRM is the system of record your company requires. SalesSidekick generates CRM-formatted output (forecast numbers, deal updates, activity logs) that you paste into your CRM.

This approach:
- Requires zero CRM admin involvement or permissions
- Works with any CRM (Salesforce, HubSpot, or anything else)
- Avoids data integrity risks from dual-writing

---

## Troubleshooting

**Connector not working?**
- Check Claude Desktop Settings > Connectors — is it connected?
- Re-authenticate if the sign-in expired
- Ask SalesSidekick: "Check my connections" — it'll test what's available

**Notion connection fails?**
- Verify your integration secret is correct (starts with `ntn_`)
- Check that the integration has access to the right workspace
- Make sure `.mcp.json` has your actual key (not the placeholder)

**Everything works without connectors.**
If you're just getting started and don't want to configure anything, that's fine. SalesSidekick works with zero connectors — you provide context by pasting and talking, and everything saves to your local workspace.

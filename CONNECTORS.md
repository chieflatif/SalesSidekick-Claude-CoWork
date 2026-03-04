# SalesSidekick — Connector Configuration

SalesSidekick uses MCP (Model Context Protocol) connectors to read and write external data. Connectors are the "hands" of the system — they let SalesSidekick interact with your real tools and data.

**Core principle: Missing connectors change behavior, not break it.** Every command works without optional connectors. You lose convenience, not capability.

---

## Connector Tiers

| Tier | Connector | Purpose | Required? |
|------|-----------|---------|-----------|
| P0 | **Notion** | Single source of truth. 6 databases (Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts). All persistent data lives here. | **Yes** |
| P1 | Gmail | Email sending from /outreach, /email, /closeout follow-ups. Thread reading for context. | No |
| P1 | Google Calendar | Meeting schedule for /today and /prep. Provides time-aware context. | No |
| P1 | Google Drive | Document storage. Transcript auto-discovery for /closeout. | No |
| P2 | Gamma | Alternative presentation generation path for /deck. | No |

---

## Setup Instructions

### Notion (Required)

Notion is the backbone. Without it, SalesSidekick operates in "offline mode" — it can still generate content, but nothing persists between sessions.

**Step 1: Create a Notion Integration**
1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New Integration"
3. Name it "SalesSidekick"
4. Select your workspace
5. Copy the Internal Integration Secret (starts with `ntn_`)

**Step 2: Run /setup**
The `/setup` command will:
- Create all 6 databases in your Notion workspace
- Share them with your integration automatically
- Store the database IDs in CLAUDE.md Section 10
- Verify read/write access

**Step 3: Verify**
After `/setup`, run `/today` — if it reads your tasks and deals, Notion is working.

### Gmail (Optional — P1)

Enables direct email sending from /outreach, /email, and /closeout follow-up emails.

**Without Gmail:** All email commands generate copy-paste formatted text with subject line and body. You paste into your email client manually.

**To connect:** Configure the Gmail MCP connector in your Claude Cowork settings under Connectors > Gmail. Follow the OAuth flow to grant read/send permissions.

### Google Calendar (Optional — P1)

Enables meeting-aware context in /today (morning briefing) and /prep (pre-meeting intel).

**Without Calendar:** /today shows tasks and deals but asks "What meetings do you have today?" instead of auto-detecting them. /prep asks "Who is the meeting with?" instead of pulling the calendar entry.

**To connect:** Configure the Google Calendar MCP connector in your Claude Cowork settings under Connectors > Google Calendar.

### Google Drive (Optional — P1)

Enables auto-discovery of call transcripts for /closeout and document storage.

**Without Drive:** /closeout asks "Paste the call transcript" instead of auto-discovering it. All other commands work normally.

**To connect:** Configure the Google Drive MCP connector in your Claude Cowork settings under Connectors > Google Drive.

### Gamma (Optional — P2)

Alternative presentation generation path for /deck. Produces web-based presentations instead of .pptx files.

**Without Gamma:** /deck generates native .pptx presentations using PptxGenJS. This is the default path and works without any additional setup.

**To connect:** During /setup Phase 5, you'll be asked if you want to use Gamma for presentations. If yes, follow the Gamma connector setup flow.

---

## Graceful Degradation Reference

Every command in SalesSidekick specifies its behavior with and without each relevant connector. This table summarizes the system-wide degradation patterns.

| Missing Connector | Commands Affected | What Changes | Fallback |
|-------------------|-------------------|-------------|----------|
| No Notion | All commands | No persistent data. Session-only intelligence. | Commands still generate output but nothing saves between sessions. User works from manual context. |
| No Calendar | /today, /prep | No meeting schedule awareness | Asks "What meetings do you have today?" or "Who is the meeting with?" |
| No Gmail | /outreach, /email, /closeout | Cannot send emails directly | Generates copy-paste formatted text with subject line and body |
| No Drive | /closeout | Cannot auto-discover transcripts | Asks user to paste the transcript directly |
| No Gamma | /deck | Cannot use Gamma presentation path | Generates native .pptx via PptxGenJS (default) |
**Note on CRM:** SalesSidekick uses an export-first CRM strategy. There is no CRM connector. `/forecast-update` generates CRM paste-ready formatted output by default — this is normal behavior, not degradation.

---

## Web Search

Commands like `/research` use Claude's built-in web search capability. This is a native platform feature available in Claude Cowork and Claude Code — not a connector. No additional setup is required.

---

## MCP Configuration (.mcp.json)

Only Notion uses a custom MCP server configuration (defined in `.mcp.json`). Other connectors (Gmail, Calendar, Drive, Gamma) are managed through the Claude Cowork platform UI and do not require `.mcp.json` entries.

The `.mcp.json` file contains `{{NOTION_API_KEY}}` as a placeholder. During `/setup`, the Notion API key is configured through the MCP server's environment variables. **The API key is never written into CLAUDE.md or any other markdown file** — it lives only in the MCP configuration layer.

For Claude Code CLI users: configure the Notion API key in your environment variables or by updating `.mcp.json` directly. If you update `.mcp.json` with your real key, do not commit it to version control.

---

## Connector Detection

Connector status variables (`{{NOTION_CONNECTED}}`, `{{GMAIL_CONNECTED}}`, etc.) are set during `/setup`. If you add or remove a connector after initial setup:

1. Re-run `/setup` to update connector status, or
2. Manually update the connector status variables in CLAUDE.md Section 10

The system also adapts gracefully at runtime — if a connector operation fails (e.g., trying to send an email without Gmail), SalesSidekick falls back to the appropriate offline behavior automatically.

---

## CRM Integration Strategy

**Current approach: Export-First.**

Notion is your working database. SalesSidekick does NOT dual-write to your CRM. Instead, commands like /forecast-update generate CRM-formatted output (Salesforce or HubSpot format, based on your {{CRM_SYSTEM}} setting) that you paste into your CRM.

This approach:
- Eliminates CRM permission dependencies
- Avoids data integrity risks from dual-writing
- Works with any CRM system
- Requires zero CRM admin involvement

**Future:** Read-only CRM connectors (Salesforce, HubSpot) for minimal field access are on the roadmap.

---

## Troubleshooting

**Notion connection fails:**
- Verify your integration secret is correct (starts with `ntn_`)
- Ensure the integration has access to the databases (shared during /setup)
- Run `/setup` again to re-verify database access

**Connector not detected:**
- Check your Claude Cowork settings under Connectors
- Re-authenticate if the OAuth token expired
- The system will tell you which connectors it detects when you run `/setup`

**Everything works without connectors:**
If you're getting started and don't want to configure anything yet, that's fine. SalesSidekick works with zero connectors — you just provide context manually instead of automatically. Run `/setup` with just Notion to get the core experience, then add connectors later.

# SalesSidekick — Integrations & Connectors

SalesSidekick uses MCP (Model Context Protocol) connectors to read and write external data. Connectors are the "hands" of the system — they let SalesSidekick interact with your real tools and data.

**Core principle: Missing connectors change behavior, not break it.** Every capability works without optional connectors. You lose convenience, not functionality.

---

## Connector Tiers

| Tier | Connector | Purpose | Required? |
|------|-----------|---------|-----------|
| P0 | **Notion** | Single source of truth. 6 databases (Companies, Contacts, Deals, Tasks, Call Notes, LinkedIn Posts). All persistent data lives here. | **Yes** |
| P1 | Gmail | Direct email sending from outreach, follow-ups, and post-call communication. Thread reading for context. | No |
| P1 | Google Calendar | Meeting schedule awareness for morning briefings and pre-meeting intelligence. Provides time-aware context. | No |
| P1 | Google Drive | Document storage. Auto-discovery of call transcripts for processing. | No |
| P2 | Gamma | Alternative web-based presentation generation path. | No |

---

## Setup Instructions

### Notion (Required)

Notion is the backbone. Without it, SalesSidekick operates in "offline mode" — it can still generate content, but nothing persists between sessions.

**Step 1: Create a Notion Integration**
1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click "New Integration"
3. Name it "SalesSidekick"
4. Select your workspace
5. Under "Capabilities", ensure "Read content" and "Insert content" are enabled
6. Copy the Internal Integration Secret (starts with `ntn_`)

**Step 2: Configure the API Key**
The plugin's `.mcp.json` file contains a `{{NOTION_API_KEY}}` placeholder. **You must replace this placeholder with your actual Notion API key.** Open `.mcp.json` in a text editor, find `{{NOTION_API_KEY}}`, and replace it with your key. The line should look like: `"Bearer ntn_your_actual_key_here"` (keep the surrounding quotes intact).

**Step 3: Databases are created on demand**
SalesSidekick creates Notion databases when you first need them — not upfront. The first time you process a call, it offers to create a Call Notes database. The first time you research a company, it offers to save it. Each creation is a one-sentence confirmation.

If you prefer to create all 6 databases at once, ask for a deep personalization session and it will set them all up.

**Step 4: Verify**
After connecting Notion, try asking "What's on my plate today?" — if SalesSidekick can read your databases, Notion is working.

### Gmail (Optional — P1)

Enables direct email sending from outreach, follow-ups, and post-call communication.

**Without Gmail:** All email capabilities generate copy-paste formatted text with subject line and body. You paste into your email client manually.

**To connect:** Configure the Gmail MCP connector in your Claude Cowork settings under Connectors > Gmail. Follow the OAuth flow to grant read/send permissions.

### Google Calendar (Optional — P1)

Enables meeting-aware context in morning briefings and pre-meeting intelligence.

**Without Calendar:** Morning briefings show tasks and deals but ask "What meetings do you have today?" instead of auto-detecting them. Meeting prep asks "Who is the meeting with?" instead of pulling the calendar entry.

**To connect:** Configure the Google Calendar MCP connector in your Claude Cowork settings under Connectors > Google Calendar.

### Google Drive (Optional — P1)

Enables auto-discovery of call transcripts and document storage.

**Without Drive:** Call processing asks "Paste the call transcript" instead of auto-discovering it. All other capabilities work normally.

**To connect:** Configure the Google Drive MCP connector in your Claude Cowork settings under Connectors > Google Drive.

### Gamma (Optional — P2)

Alternative presentation generation path. Produces web-based presentations instead of .pptx files.

**Without Gamma:** Presentations are generated as native .pptx files using PptxGenJS. This is the default path and works without any additional setup.

**To connect:** When doing a deep personalization session, you'll be asked if you want to use Gamma for presentations. If yes, follow the Gamma connector setup flow.

---

## What Changes Without Each Integration

Every capability in SalesSidekick adapts to available connectors. This table summarizes what changes when an integration is missing.

| Missing Connector | Capabilities Affected | What Changes | Fallback |
|-------------------|-----------------------|-------------|----------|
| No Notion | All capabilities | No persistent data. Session-only intelligence. | Capabilities still generate output but nothing saves between sessions. User works from manual context. |
| No Calendar | Morning briefings, end-of-day, meeting prep | No meeting schedule awareness | Asks "What meetings do you have today/tomorrow?" or "Who is the meeting with?" |
| No Gmail | Outreach, contextual email, post-call follow-up | Cannot send emails directly | Generates copy-paste formatted text with subject line and body |
| No Drive | Call processing | Cannot auto-discover transcripts | Asks user to paste the transcript directly |
| No Gamma | Presentations | Cannot use Gamma presentation path | Generates native .pptx via PptxGenJS (default) |
| No web search | Company research, account creation, business cases | Cannot conduct web research | Relies on user-provided information only. Briefs and content are thinner. |

**Note on CRM:** SalesSidekick uses an export-first CRM strategy. There is no CRM connector. Forecast updates generate CRM paste-ready formatted output by default — this is normal behavior, not degradation.

---

## Web Search

Capabilities that use web search: company research (primary), adding new companies (supplemental info), deep personalization (company intel and competitor research). Business cases may also use web search when company intelligence is incomplete. Web search is a native platform feature available in Claude Cowork and Claude Code — not a connector. No additional setup is required.

---

## MCP Configuration (.mcp.json)

Only Notion uses a custom MCP server configuration (defined in `.mcp.json`). Other connectors (Gmail, Calendar, Drive, Gamma) are managed through the Claude Cowork platform UI and do not require `.mcp.json` entries.

The `.mcp.json` file contains `{{NOTION_API_KEY}}` as a placeholder. **You must replace this placeholder with your actual Notion API key.** Open `.mcp.json` in a text editor, find `{{NOTION_API_KEY}}`, and replace it with your integration secret (starts with `ntn_`). The API key is never written into CLAUDE.md or any other markdown file — it lives only in the MCP configuration layer.

If you update `.mcp.json` with your real key, do not commit it to version control.

---

## Connector Detection

Connector status is managed progressively — the system detects available connectors at runtime and adapts accordingly. If you add a connector after your initial setup, SalesSidekick will detect it on next use and update its behavior automatically.

You can also ask for a deep personalization session to verify all connectors and update their status explicitly.

The system also adapts gracefully at runtime — if a connector operation fails (e.g., trying to send an email without Gmail), SalesSidekick falls back to the appropriate offline behavior automatically.

---

## CRM Integration Strategy

**Current approach: Export-First.**

Notion is your working database. SalesSidekick does NOT dual-write to your CRM. Instead, forecast updates generate CRM-formatted output (Salesforce or HubSpot format, based on your CRM setting) that you paste into your CRM.

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
- Ensure the integration has access to the databases
- Ask SalesSidekick to verify your Notion connection

**Connector not detected:**
- Check your Claude Cowork settings under Connectors
- Re-authenticate if the OAuth token expired
- Ask for a deep personalization session to re-verify all connectors

**Everything works without connectors:**
If you're getting started and don't want to configure anything yet, that's fine. SalesSidekick works with zero connectors — you just provide context manually instead of automatically. Connect Notion for persistence, then add other connectors whenever convenient.

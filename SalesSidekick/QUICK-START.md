# SalesSidekick — Quick Start

Get up and running in under 5 minutes.

---

## Step 1: Install the Plugin

1. Download `SalesSidekick.zip` from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases)
2. Open Claude Desktop and switch to **Cowork**
3. Click **Customize** in the left sidebar > **Plugins** > **+** > **Add personal plugin**
4. Select the `SalesSidekick.zip` file you downloaded

---

## Step 2: Connect Notion

SalesSidekick saves your deals, contacts, tasks, and call notes to Notion automatically. You need to connect it once.

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **New Integration** — name it "SalesSidekick", select your workspace
3. Under Capabilities, make sure **Read content** and **Insert content** are enabled
4. Copy the integration secret (it starts with `ntn_`)
5. Find the `.mcp.json` file in the plugin folder, open it in any text editor, and replace `{{NOTION_API_KEY}}` with your actual key

**What happens if you skip Notion?** SalesSidekick still works — you can still prep for calls, debrief transcripts, draft emails, and get deal strategy. But nothing saves between sessions. Every conversation starts fresh, with no memory of your accounts, deals, or previous work. Notion is what makes it persistent. See [CONNECTORS.md](CONNECTORS.md) if you need help setting it up.

---

## Step 3: Create Your Workspace Folder

Create a folder on your machine — name it `SalesSidekick` or whatever you like:

```
~/Documents/SalesSidekick/
```

When you start a Cowork conversation, click **"Work in a folder"** at the bottom left and select this folder. **Always use the same folder** — SalesSidekick writes a local state file here after your first session, so every future conversation loads your context instantly with zero API calls.

---

## Step 4: Start Your First Conversation

Open a new Cowork conversation and just say hi. SalesSidekick will introduce itself, check what's connected, ask the basics (name, company, what you sell), and research your company and competitive landscape automatically.

**At the end of that first conversation, you'll receive a personalized settings block to paste into your Cowork settings** — that's the step that makes every future session instantly know who you are. Don't skip it.

---

## Your First Five Minutes

SalesSidekick is voice-first and plain-language. You don't need to learn commands or special syntax — just talk to it like you'd talk to a colleague.

**Two great ways to start getting value immediately:**

**1. Share your top active deals**
> "Here are my top 3 deals right now: [deal names and where they stand]"

Or paste a screenshot of your CRM or forecast. SalesSidekick will build context around each deal and tell you what needs attention.

**2. Drop in a recent call transcript**
> "Here's a transcript from my call with [Company] today"

Paste the text. SalesSidekick will score the deal against MEDDPICC, pull out every action item, write your follow-up email, and flag any risks — all at once.

---

## What You Can Ask At Any Time

Not sure what to do? Just ask:
- **"What can you do?"** — SalesSidekick will explain its capabilities in plain English
- **"How do I get started with [something]?"** — it'll walk you through it
- **"What do you know about me?"** — it'll tell you what context it has and what's missing

**Some things it knows how to do:**
- Prep you for any meeting with an intel brief and talking points
- Debrief a call: scoring, action items, follow-up email, risk flags — all from a transcript
- Research any company or contact before you reach out
- Write outreach and follow-up emails in your voice
- Build a business case or point of view for an executive presentation
- Analyze deal strategy and give you three paths forward
- Give you a morning briefing of what needs attention today
- Review your pipeline and flag which deals are at risk

---

## Tips

**A note on the first session:** When SalesSidekick creates your Notion databases for the first time, it takes a minute or two — it's setting up 6 linked databases with custom schemas. Normal. You only pay this cost once.

**You can always tweak how it works.** If an email sounds off, tell it. If you want a different format, ask for it. SalesSidekick learns from feedback — the more you correct and adjust, the better it matches your style.

**You don't need to paste content in any special format.** A CRM screenshot, a copied email, a raw call transcript, a company one-pager — just drop it in. SalesSidekick figures out what it is and uses it.

**Optional integrations:** Add Gmail (send emails directly), Google Calendar (meeting-aware briefings), Google Drive (auto-discover transcripts), or Gamma (web presentations). See [CONNECTORS.md](CONNECTORS.md) for setup. None of these are required — if they're missing, SalesSidekick adapts.

---

## Need Help?

- Ask SalesSidekick directly — **"How do I use you?"** or **"What can you help me with?"**
- See [CONNECTORS.md](CONNECTORS.md) for integration setup and troubleshooting
- See [INSTALLATION-GUIDE.md](INSTALLATION-GUIDE.md) for detailed setup instructions

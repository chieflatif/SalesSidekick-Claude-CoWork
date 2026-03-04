# SalesSidekick — Quick Start

You've installed SalesSidekick. Here's how to get started.

---

## Step 1: Configure Notion (required)

If you haven't already:
1. Create a Notion integration at [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Copy your API key (starts with `ntn_`)
3. Replace `{{NOTION_API_KEY}}` in the plugin's `.mcp.json` file with your actual key

---

## Step 2: Create a working folder

Create a folder for SalesSidekick output (presentations, documents, exports):

```
~/Documents/SalesSidekick/
```

When starting a Cowork conversation, click **"Work in a folder"** at the bottom left and select this folder. Your database data lives in Notion — this folder is for file output like decks and POVs.

---

## Step 3: Run /setup (required, one-time)

```
/salessidekick:setup
```

This is the most important command. It personalizes everything — your identity, company intel, competitive battlecards, brand voice, databases, and connectors. Takes ~45 minutes the first time. After that, every command knows who you are.

---

## Step 4: Try These 5 Commands

Once `/salessidekick:setup` is complete, try these in order:

### Morning Briefing

```
/salessidekick:today
```

See your tasks, deals needing attention, and meeting prep for the day.

### Before a Meeting

```
/salessidekick:prep Acme Corp
```

Get an intelligence brief with talking points, MEDDPICC gaps to probe, and stakeholder context.

### After a Call

```
/salessidekick:closeout
```

Paste your call transcript. Get MEDDPICC scoring, action items, coaching feedback, a follow-up email, risk signals, and competitive intel — all written to Notion automatically.

### Check Your Pipeline

```
/salessidekick:pipeline
```

See your full territory pipeline with deal health, MEDDPICC confidence, and weighted values.

### Plan Your Strategy

```
/salessidekick:strategy Acme Corp
```

Deep strategic analysis through five lenses with three recommended paths forward.

---

## What's Next?

- `/salessidekick:outreach [Company]` — Write a prospecting email backed by intelligence
- `/salessidekick:draft-post` — Create a LinkedIn post using the 3-Type Framework
- `/salessidekick:deck [Company]` — Generate a presentation (auto-selects template by deal stage)
- `/salessidekick:weekly` — Prepare your weekly pipeline summary for your manager 1:1
- `/salessidekick:research [Company]` — Deep company research before reaching out

---

## Need Help?

- Run `/salessidekick:audit` to check your configuration integrity
- See [CONNECTORS.md](CONNECTORS.md) for connector setup and troubleshooting
- See [INSTALLATION-GUIDE.md](INSTALLATION-GUIDE.md) for detailed setup instructions
- Full command reference in [README.md](README.md)

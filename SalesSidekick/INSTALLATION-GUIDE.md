# SalesSidekick — Installation Guide

## What You Need

- Claude Desktop (Mac or Windows) with a Pro or Max subscription
- About 5 minutes

That's it. No other accounts, services, or technical setup required.

---

## Install the Plugin

1. Download `SalesSidekick.zip` from the [releases page](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases/latest/download/SalesSidekick.zip)
2. Open Claude Desktop
3. Go to **Settings** > **Plugins** > **Add Plugin** > **Upload a File**
4. Select the `SalesSidekick.zip` file you downloaded
5. You should see "SalesSidekick" appear in your plugins list

---

## Set Up Your Workspace

SalesSidekick saves your deals, contacts, call notes, and everything else to a folder on your machine. Think of it as your personal sales command center.

1. Create a folder called `SalesSidekick` in your Documents folder
2. In Claude Desktop, click the **+** icon next to **Projects** in the left sidebar
3. Name the project **SalesSidekick** and point it at the folder you just created

**Why Documents?** Most cloud backup services (iCloud, Google Drive, OneDrive) automatically back up your Documents folder. That means your SalesSidekick data is protected without doing anything extra.

**Why a Project?** A Cowork Project gives SalesSidekick a permanent workspace. It remembers your context between sessions, saves your deal files, and can run background checks on your pipeline automatically. Without a Project, SalesSidekick still works but nothing saves between conversations.

---

## Start Your First Conversation

Open a conversation inside your SalesSidekick project and say hello.

SalesSidekick will walk you through everything:

1. **Check what's connected** — Gmail, Calendar, etc. (all optional)
2. **Ask the basics** — your name, company, what you sell, your territory
3. **Offer style calibration** — 7 quick questions about how you sell (optional, takes 2 minutes)
4. **Research your company and competitors** — automatic, no input needed
5. **Save your identity** — writes your profile so every future session knows you instantly
6. **Set up your workspace** — creates the folders and files for deal tracking
7. **Offer background briefings** — an automatic morning pipeline check (optional)
8. **Invite you to start** — share your top deals, drop in a transcript, or just tell it what you're working on

**Total time: about 5 minutes.** After that, you're live.

---

## Optional: Connect Services

These are all optional. SalesSidekick works without any of them. Each one adds convenience:

| Service | What it adds | How to connect |
|---------|-------------|---------------|
| Gmail | Send emails directly instead of copy-paste | Settings > Connectors > Gmail |
| Google Calendar | Meeting-aware briefings and auto-prep | Settings > Connectors > Calendar |
| Google Drive | Auto-find call transcripts | Settings > Connectors > Drive |
| Notion | Structured database views for power users | See [CONNECTORS.md](CONNECTORS.md) |

If a service isn't connected, SalesSidekick adapts — it'll ask you to paste transcripts instead of finding them, generate copy-paste emails instead of sending, and ask about your schedule instead of reading your calendar.

---

## Upgrading

When a new version is available:

1. Download the new `.zip` from the [same link](https://github.com/chieflatif/SalesSidekick-Claude-CoWork/releases/latest/download/SalesSidekick.zip)
2. In Claude Desktop, go to **Settings** > **Plugins**
3. Remove the old SalesSidekick plugin
4. Upload the new zip file
5. Open your SalesSidekick project — the system will tell you what's new

**Your data is untouched.** Deals, contacts, call notes, custom skills, knowledge bases — everything in your workspace survives the upgrade. Only the plugin brain gets updated.

If you've customized how any capabilities work, SalesSidekick will let you know if the update includes improvements to those defaults so you can decide whether to incorporate them.

---

## Enterprise Deployment

For team-wide deployment, admins can upload the plugin to an organization-managed marketplace:

1. Go to **Organization settings** > **Plugins** in Claude Desktop
2. Upload the `SalesSidekick.zip` file
3. Set visibility to your sales team
4. Add installation notes: "After installing, open a Cowork conversation and just start talking — the system works immediately and gets smarter as you use it"

Each AE installs from the org marketplace and personalizes through their first conversation.

See [enterprise-deployment.md](docs/enterprise-deployment.md) for team configuration and onboarding workflows.

---

## Verifying Your Installation

After your first conversation, verify everything is working:

| What to say | What should happen |
|-------------|-------------------|
| "What's on my plate today?" | Shows your tasks and deals (or invites you to add some if empty) |
| "Tell me about [company you've discussed]" | Recalls company context from your workspace |
| Paste a call transcript | Processes it: MEDDPICC scoring, action items, follow-up email, risk flags |
| "How should I approach [deal]?" | Strategic analysis with three paths forward |

---

## Uninstalling

1. Remove the plugin from Settings > Plugins
2. Your workspace folder and all data remain on your machine — SalesSidekick never deletes your data
3. Your identity in Global CLAUDE.md remains — remove the section between `<!-- SALESSIDEKICK-IDENTITY-START -->` and `<!-- SALESSIDEKICK-IDENTITY-END -->` markers if you want a clean slate

---

## Need Help?

- Ask SalesSidekick: **"How do I use you?"** or **"What's not working?"**
- See [CONNECTORS.md](CONNECTORS.md) for connector setup
- See [QUICK-START.md](QUICK-START.md) for a shorter getting-started guide

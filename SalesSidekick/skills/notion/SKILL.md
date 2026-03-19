---
name: notion
description: Central reference for all Notion database schemas, read/write patterns, account resolution logic, and write failure handling
tier: 1 (universal)
auto-fire:
  intents: [all data operations]
  context: "When any interaction requires reading from or writing to Notion databases"
user-invocable: false
---

# Notion — Database Operations Skill

## Purpose

Central reference for all Notion database operations. Contains the complete schemas for all 6 databases (70 fields total), read/write patterns per command, account resolution logic, and write failure handling. This is the single source of truth for how SalesSidekick interacts with Notion.

## When Referenced

- **Any interaction reading from Notion** (all data operations) — schema lookup, field validation, and account resolution logic for database queries
- **Any interaction writing to Notion** (all data operations) — field mapping, write patterns, and write failure handling for database updates
- **Deep personalization sessions** (customize-system intent) — creates all 6 databases with these schemas during initial configuration
- **Adding new accounts and deals** (add-account intent) — record creation patterns for Companies, Contacts, and Deals databases
- **Processing calls** (process-call intent) — writes to 4 databases simultaneously (Call Notes, Tasks, Deals, Companies)

## Core Framework

### Database Schemas

#### Companies (12 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Company Name | Title | — |
| 2 | Territory Status | Select | Active, Prospect, Inactive, Former Customer |
| 3 | Industry | Select | Options populated during deep personalization per user's market |
| 4 | Account Size | Select | Enterprise, Mid-Market, SMB |
| 5 | Hell Yes/Hell No | Select | Hell Yes, Evaluating, Hell No |
| 6 | Primary Contact | Relation | →Contacts |
| 7 | Tech Stack | Multi-select | Populated per account |
| 8 | Current Products | Multi-select | User's product line, set during deep personalization |
| 9 | Annual Revenue | Number | — |
| 10 | Last Activity | Date | Auto-updated on any interaction |
| 11 | Notes | Text | Research briefs, general context |
| 12 | Website | URL | — |

#### Contacts (9 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Name | Title | — |
| 2 | Company | Relation | →Companies |
| 3 | Title | Text | Job title |
| 4 | MEDDPICC Role | Multi-select | Champion, Economic Buyer, Technical Buyer, Influencer, Blocker, User |
| 5 | Email | Email | — |
| 6 | LinkedIn | URL | — |
| 7 | Sentiment | Select | Champion, Supportive, Neutral, Skeptical, Blocker |
| 8 | Last Contacted | Date | — |
| 9 | Notes | Text | — |

#### Deals (22 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Deal Name | Title | — |
| 2 | Company | Relation | →Companies |
| 3 | Contacts | Relation | →Contacts (linked during add deal) |
| 4 | Stage | Select | Prospecting, Discovery, Qualification, Proposal, Negotiation, Closed Won, Closed Lost |
| 5 | Deal Value | Number | — |
| 6 | Close Date | Date | — |
| 7 | Forecast Category | Select | Commit, Best Case, Pipeline, Omitted |
| 8 | Primary Competitor | Select | Options populated during deep personalization Phase 3 |
| 9 | Next Step | Text | — |
| 10 | Deal Risk | Select | High, Medium, Low |
| 11 | MEDDPICC Confidence | Select | High, Medium, Low |
| 12 | M-Metrics | Select | Red, Yellow, Green |
| 13 | E-Economic Buyer | Select | Red, Yellow, Green |
| 14 | D-Decision Criteria | Select | Red, Yellow, Green |
| 15 | D-Decision Process | Select | Red, Yellow, Green |
| 16 | P-Paper Process | Select | Red, Yellow, Green |
| 17 | I-Identify Pain | Select | Red, Yellow, Green |
| 18 | C-Champion | Select | Red, Yellow, Green |
| 19 | C-Competition | Select | Red, Yellow, Green |
| 20 | Created Date | Date | Set on creation |
| 21 | Last Activity | Date | Auto-updated on any interaction |
| 22 | Notes | Text | — |

**Default Stage Probability Weights** (used by forecast update):
| Stage | Default Probability |
|-------|-------------------|
| Prospecting | 10% |
| Discovery | 20% |
| Qualification | 40% |
| Proposal | 60% |
| Negotiation | 80% |
| Closed Won | 100% |
| Closed Lost | 0% |

These are defaults. Deep personalization asks if the user's org uses different stage names or probabilities, and stores customized values in {{DEAL_STAGES}}.

#### Tasks (9 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Task | Title | — |
| 2 | Company | Relation | →Companies |
| 3 | Deal | Relation | →Deals |
| 4 | Owner | Select | AE, Prospect, Internal |
| 5 | Priority | Select | High, Medium, Low |
| 6 | Due Date | Date | — |
| 7 | Status | Select | Not Started, In Progress, Done, Blocked |
| 8 | Source | Select | Call, Strategy, Manual |
| 9 | Notes | Text | — |

#### Call Notes (10 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Call Title | Title | — |
| 2 | Company | Relation | →Companies |
| 3 | Deal | Relation | →Deals |
| 4 | Call Date | Date | — |
| 5 | Summary | Text | AI-generated summary |
| 6 | Action Items | Text | Extracted tasks (also written to Tasks DB) |
| 7 | Risk Level | Select | High, Medium, Low |
| 8 | Competitive Intel | Text | Competitor mentions and analysis |
| 9 | Coaching Score | Number | 1-5 (average of 5 coaching dimensions) |
| 10 | MEDDPICC Changes | Text | Elements that changed R/Y/G this call |

#### LinkedIn Posts (8 fields)
| # | Field | Type | Options/Notes |
|---|-------|------|---------------|
| 1 | Post Title | Title | — |
| 2 | Post Type | Select | Personal, Business, Educational |
| 3 | Status | Select | Idea, Draft, Ready, Scheduled, Posted |
| 4 | Scheduled/Posted Date | Date | — |
| 5 | Hook | Text | First 2 lines (scroll-stopper) |
| 6 | Content | Text | Full post body |
| 7 | Related Company | Relation | →Companies |
| 8 | Notes | Text | — |

**Total: 6 databases, 70 fields.**

### Account Resolution Logic

When a user references a company by name, resolve using this priority order:

1. **Exact match** on Company Name field (case-insensitive)
2. **Starts-with match** if no exact match (e.g., "Acme" matches "Acme Corporation")
3. **Contains match** as fallback (e.g., "Acme" matches "The Acme Group")
4. **Multiple matches** → ask user to clarify: "I found multiple matches: [list]. Which one?"
5. **No match** → "I don't have [Company] in Notion. Want me to add [Company] to your pipeline?"

When resolving for deal-specific capabilities (deal strategy, competitive analysis, meeting prep):
- After finding the company, look for active deals (Stage ≠ Closed Won, Closed Lost)
- If multiple active deals → ask: "You have [X] active deals at [Company]: [list]. Which one?"
- If one active deal → use it automatically
- If no active deals → "No active deal at [Company]. Want to add a deal for [Company]?"

### Read/Write Patterns by Command

| Capability | Reads | Writes |
|------------|-------|--------|
| Morning Briefing | Tasks (due/overdue), Deals (needing attention) | — |
| Evening Wrap | Tasks (today's), Deals (movement) | — |
| Weekly Summary | Deals (all active), Tasks (week), Call Notes (week) | — |
| Call Processing | Companies, Contacts, Deals, Call Notes (history) | Call Notes (new), Tasks (new), Deals (MEDDPICC update), Companies (Last Activity) |
| Meeting Prep | Companies, Contacts, Deals, Call Notes (recent), Tasks | — |
| Deal Strategy | Companies, Deals, Contacts, Call Notes, Tasks | — |
| Competitive Analysis | Companies, Deals, Call Notes, Contacts | — |
| Business Case | Companies, Deals, Call Notes | — |
| Pipeline Review | Deals (all active), Companies | — |
| Whitespace Analysis | Companies, Deals | — |
| Forecast Update | Deals (by period) | — |
| Account Research | Companies (if exists), Deals, Call Notes | Companies (Notes, Tech Stack, Industry, Account Size) |
| Outreach | Companies, Contacts, Deals | — |
| Contextual Email | Companies, Contacts, Deals, Call Notes | — |
| LinkedIn Post | LinkedIn Posts (recent) | LinkedIn Posts (new draft, on confirm) |
| Presentation | Companies, Deals, Contacts, Call Notes | — |
| Add Company | Companies (duplicate check) | Companies (new record) |
| Add Deal | Companies, Contacts, Deals (duplicate check) | Deals (new record) |
| Call Coaching | Call Notes, Deals | — |

### Write Failure Handling

When a Notion write fails:

1. **Never silently drop data.** Always inform the user what failed to save.
2. **Present the data that would have been written** in a formatted block the user can manually enter.
3. **Diagnose the likely cause:**
   - "Database not found" → Database ID may be wrong. Suggest: "Check that your Notion databases are set up. Try a deep personalization session to reconfigure."
   - "Permission denied" → API key may lack write access. Suggest: "Check your Notion integration permissions — it needs read AND write access to the databases."
   - "Validation error" → Field type mismatch. Suggest: "The [field] in your [database] may have different options than expected. Check the field configuration in Notion."
   - "Rate limited" → Too many requests. Suggest: "Notion is rate-limiting requests. I'll try again in a moment."
4. **Retry once** on transient errors (rate limit, timeout). If retry fails, fall back to manual mode.
5. **Log the failure context** so self-audit can check for patterns.

### Three-Tier Persistence Architecture

SalesSidekick uses three distinct persistence layers. Understanding the hierarchy is critical for correct session behavior.

| Tier | Where | What lives here | When it's read | API calls |
|------|-------|----------------|----------------|-----------|
| **Slow lane** | Cowork global settings field | Stable identity: name, company, product, ICP, competitors, communication style. Set once, rarely changes. | Automatically by Cowork before any plugin fires | Zero |
| **Local fallback** | `sidekick-state.md` in SalesSidekick folder | Mirror of global settings block — written automatically after personalization. Used when global settings haven't been updated yet, or as a richer fallback. | On session start if folder is active and file exists | Zero |
| **Operational** | Notion Config page + 6 databases | Dynamic data: deal status, contacts, tasks, DB IDs, full personalization variables, connector status, call notes | Lazily, on first operation requiring stored data | Per operation |

**Why this matters for session startup:**
- Basic identity (who the user is, what they sell) is available from tiers 1 and 2 with zero API overhead
- Notion is only called when the session actually needs database data
- A session spent drafting an email or discussing deal strategy may never call Notion at all
- If Notion is unavailable, tiers 1 and 2 keep the system functional for non-database work

**sidekick-state.md format:** Markdown file, same content as the global settings block — plain prose, not structured key:value data. **Read as freeform context, same as global settings text: parse for identity signals (name, company, product, competitors, communication style), no structured parsing required.** Written automatically to the active folder after any personalization session (deep personalization or first-run). Never needs to be manually created or edited by the user.

**Staleness:** Tier 1 (global settings) always supersedes tier 2 (sidekick-state.md). A stale `sidekick-state.md` is harmless by design — the more current global settings win. The file updates whenever a new personalization session completes.

**Missing file:** If the active folder has no `sidekick-state.md`, skip tier 2 silently and fall through to tier 3. Never error or warn the user about an absent file.

**When to write sidekick-state.md:** At the end of any session that captures or updates identity data — both the first-run getting-started flow (CLAUDE.md Section 15.2) and deep personalization sessions (setup.md). The file should always reflect the most current personalization state.

### Config Page (7th persistence target — created first, always)

The Config page is a single Notion page (not a database) that stores all identity and personalization variables. It is the persistent replacement for CLAUDE.md template variables, which are read-only in Cowork and cannot be written back by the plugin.

**Why this exists:** Plugin files (including CLAUDE.md) are read-only in Claude Cowork. Variables like `{{AE_NAME}}`, `{{COMPANY}}` etc. cannot be written at runtime. Without the Config page, every session would restart as FRESH with no memory of identity or personalization. The Config page solves this — it's the live source of truth for all Tier 2 variables.

**Page title:** `SalesSidekick — Config`

**Structure:** A Notion page with a two-column table of variable names and values:

```
| Variable | Value |
|----------|-------|
| AE_NAME | [name] |
| AE_TITLE | [title] |
| COMPANY | [company] |
| COMPANY_URL | [url] |
| PRODUCT_DESCRIPTION | [description] |
| PRIMARY_PRODUCT | [product] |
| SECONDARY_PRODUCTS | [products] |
| ICP_INDUSTRY | [industry] |
| ICP_SIZE | [size] |
| ICP_USE_CASE | [use case] |
| TERRITORY_SIZE | [count] |
| TERRITORY_TYPE | [type] |
| REGION | [region] |
| QUOTA_AMOUNT | [quota] |
| FISCAL_YEAR_START | [month] |
| AVERAGE_DEAL_SIZE | [amount] |
| SALES_CYCLE_LENGTH | [duration] |
| TOP_COMPETITOR_1 | [competitor] |
| TOP_COMPETITOR_2 | [competitor] |
| TOP_COMPETITOR_3 | [competitor] |
| COMMUNICATION_STYLE | [style] |
| EMAIL_SIGN_OFF | [sign-off] |
| LINKEDIN_TOPICS | [topics] |
| LINKEDIN_AUDIENCE | [audience] |
| CRM_SYSTEM | [crm] |
| DEAL_STAGES | [stages] |
| MANAGER_NAME | [name] |
| TEAM_NAME | [name] |
| NOTION_CONNECTED | true |
| GMAIL_CONNECTED | [true/false] |
| CALENDAR_CONNECTED | [true/false] |
| DRIVE_CONNECTED | [true/false] |
| GAMMA_CONNECTED | [true/false] |
| NOTION_COMPANIES_DB_ID | [id] |
| NOTION_CONTACTS_DB_ID | [id] |
| NOTION_DEALS_DB_ID | [id] |
| NOTION_TASKS_DB_ID | [id] |
| NOTION_CALL_NOTES_DB_ID | [id] |
| NOTION_LINKEDIN_POSTS_DB_ID | [id] |
| PLUGIN_VERSION | [e.g. 3.0.0] |
```

**Version tracking:** The `PLUGIN_VERSION` field stores the plugin version that last wrote to this Config page. On first Config read each session, compare PLUGIN_VERSION to the current plugin version (found in plugin.json). If they differ, the user has upgraded since their last session — surface a brief "what's new" note and update PLUGIN_VERSION to current. If the field is empty, write the current version on first save.

**Config page access pattern:** Read the Config page lazily — not at session start, but on the first operation that requires stored data (database query, DB ID lookup, full personalization state check). This means:
- Sessions that only use basic identity (name, company from Cowork global settings) have zero Notion API overhead
- Sessions that touch databases read Config once on first use, then cache all values for the rest of the session
- If no Notion operation occurs in a session, Config is never read

**State detection:** On first Config read, check populated fields to determine FRESH / BASICS / LEARNING / CALIBRATED. If the Config page does not exist → FRESH state.

**Write behavior:** Any variable captured during the session (name, company, territory, competitor, connector status) is written to the Config page immediately. Never silently drop a variable — if the Notion write fails, present the value in a copy-paste block.

### Database Creation (used by deep personalization Phase 6)

When creating databases during deep personalization:
1. Create Config page first — this is required before anything else persists
2. Create databases in dependency order: Companies → Contacts → Deals → Tasks → Call Notes → LinkedIn Posts
3. Companies and Contacts must exist before Deals (for relations)
4. Set up all relations after databases exist
5. Populate Select field options with defaults listed above
6. Store resulting database IDs in the Config page (not in CLAUDE.md — plugin files are read-only in Cowork)
7. Verify creation by reading back each database

## Personalization Notes

- Select field options for Industry, Primary Competitor, and Current Products are customized during deep personalization
- Stage names and probability weights may be customized (stored as DEAL_STAGES in Config page)
- Database IDs are stored in the Config page after creation
- Account resolution logic is universal — no personalization needed
- CLAUDE.md template variables ({{AE_NAME}}, etc.) are read-only defaults — the Config page overrides them on first Config read

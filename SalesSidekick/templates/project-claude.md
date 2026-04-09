# SalesSidekick — Workspace Config

This file is auto-generated during SalesSidekick setup. It tells the system how to manage your data, what signals to watch for, and when to run background checks. You can edit it, but most users never need to.

---

## Data Schemas

Every file in `data/` has a structured header (YAML frontmatter) followed by free-text notes. The header is the data. The notes are context.

**Schema version:** 1

### Deal

```yaml
---
_schema: 1
type: deal
id: company-slug-deal-slug         # unique identifier
company: company-slug               # links to a company file
contacts: [contact-slug-1]          # links to contact files
stage: Discovery                    # Discovery | Qualification | Technical Evaluation | Negotiation | Closed Won | Closed Lost
status: active                      # active | closed-won | closed-lost
amount: 250000                      # deal value (integer)
currency: USD                       # ISO currency code
close_date: 2026-06-30
probability: 30                     # 0-100
meddpicc:
  M: red       # Metrics
  E: red       # Economic Buyer
  D1: red      # Decision Criteria
  D2: red      # Decision Process
  P: red       # Paper Process
  I: red       # Identify Pain
  C1: red      # Champion
  C2: red      # Competition
health: 0                           # 0-100, computed by signal agent
last_activity: 2026-03-25           # computed from most recent call note or task
signals: []                         # computed by signal agent
next_step: ""
closed_reason: ""                   # set when deal closes
created: 2026-03-25
updated: 2026-03-25
---
```

Required: type, id, company, stage, status, amount, close_date, meddpicc, created
Computed (set by system, not user): health, last_activity, signals

### Company

```yaml
---
_schema: 2
type: company
id: company-slug
name: Company Name
url: company.com
industry: ""
size: 0                             # employee count
region: ""
deals: []                           # computed from deal files
contacts: []                        # computed from contact files
evidence_sources: []                # provenance: what sources informed key fields
last_researched: 2026-03-25
created: 2026-03-25
updated: 2026-03-25
---
```

Required: type, id, name, created
Computed: deals, contacts

**evidence_sources format:** Each entry records what source informed a field update. Appended by `/research` and other intelligence-gathering capabilities.

```yaml
evidence_sources:
  - field: industry
    value: "Financial Services"
    grade: verified
    source: "SEC IAPD registration"
    date: 2026-04-09
  - field: size
    value: 6
    grade: verified
    source: "Company website team page"
    date: 2026-04-09
  - field: tech_stack
    value: "Microsoft 365"
    grade: estimated
    source: "Job postings on LinkedIn"
    date: 2026-04-09
```

**Grade values:** `verified` (official source — SEC, website, press release), `estimated` (inferred from signals — job postings, tech detection), `hypothesis` (industry pattern or assumption).

**Usage:** Downstream capabilities can check evidence freshness and grade before citing company data. If a field was last sourced >90 days ago, the system can flag it for re-research. If a field is hypothesis-grade, the system knows to present it cautiously.

**Migration from schema 1:** Add `evidence_sources: []` to existing company files. Non-destructive — empty list means "no provenance tracked yet." Fields populated before provenance tracking are treated as ungraded.

### Contact

```yaml
---
_schema: 1
type: contact
id: first-last
name: First Last
company: company-slug
deals: []                           # computed from deal files
title: ""
role: unknown                       # champion | technical | economic-buyer | coach | blocker | unknown
engagement: unknown                 # active | warm | cold | unknown
last_contact: 2026-03-25            # computed from call notes
email: ""
phone: ""
linkedin: ""
created: 2026-03-25
updated: 2026-03-25
---
```

Required: type, id, name, company, created
Computed: deals, last_contact

### Call Note

```yaml
---
_schema: 1
type: call-note
id: call-2026-03-25-company-slug
company: company-slug
deal: deal-slug                     # optional — not all calls are deal-specific
contacts: [contact-slug]
date: 2026-03-25
duration: ""
call_type: other                    # discovery | demo | negotiation | check-in | internal | note | other
meddpicc_changes: {}                # only elements that changed
signals_detected: []
tasks_extracted: 0
created: 2026-03-25
---
```

Required: type, id, company, contacts, date, created
Body contains extracted intelligence — key findings, quotes, action items. NOT the raw transcript.

### Task

```yaml
---
_schema: 1
type: task
id: task-001
deal: deal-slug                     # optional
company: company-slug               # optional
description: "What needs to be done"
due_date: 2026-03-26
status: open                        # open | completed | overdue | cancelled
source: call-2026-03-25-company     # where this task came from
completed_date:                     # set when completed
created: 2026-03-25
updated: 2026-03-25
---
```

Required: type, id, description, due_date, status, created

### Pattern

```yaml
---
_schema: 1
type: pattern
id: pattern-2026-03-27-001
pattern_type: signal-pattern         # signal-pattern | win-pattern | loss-pattern | competitive-pattern | timing-pattern
confidence: low                      # low | medium | high
source_deals: [deal-slug]
source_notes: [call-note-id]
tags: [competitor-name, deal-stage, meddpicc-element]
occurrences: 1
created: 2026-03-27
updated: 2026-03-27
---
```

Required: type, id, pattern_type, confidence, created
Body contains: one clear sentence describing the pattern, **Evidence:** section with specific references, **Implication:** section with recommended action.
Patterns are stored in `data/patterns/`. Before creating a new pattern, check for semantic overlap with existing patterns — if similar, increment occurrences instead.

---

## Index.md Format

`data/index.md` is a flattened view of ALL entity data in markdown tables. It exists so the system can answer questions about your pipeline by reading one file instead of opening every deal file.

**Critical rule:** index.md is a CACHE. Entity files in `data/` are the source of truth. If they ever disagree, the entity files win. The system can rebuild index.md from entity files at any time.

### Table Structure

```markdown
## Deals (Active)
| id | company | contacts | stage | amount | close_date | probability | health | meddpicc | signals | last_activity | next_step |

## Deals (Closed)
| id | company | stage | amount | close_date | status | closed_reason |

## Companies
| id | name | industry | size | region | deals | contacts | last_researched |

## Contacts
| id | name | company | deals | title | role | engagement | last_contact | email |

## Signals Active
| deal | signal | severity | detected | evidence |

## Tasks Active
| id | deal | company | description | due_date | status |
```

### MEDDPICC Encoding

In tables, MEDDPICC is an 8-character string: R=Red, Y=Yellow, G=Green
Order: M, E, D1, D2, P, I, C1, C2
Example: `RRRRRRYR` = all Red except Champion (Yellow)

### Rebuild Protocol

To rebuild index.md from scratch:
1. Read every file in data/companies/, data/deals/, data/contacts/, data/call-notes/, data/tasks/
2. Extract frontmatter from each file
3. Rebuild all tables (active deals separate from closed)
4. Recount entities in the index header
5. Write new index.md with current timestamp

This is resource-intensive and only runs during the weekly deep audit or when you ask for it ("rebuild my index").

---

## Cross-Reference Rules

- All references use the `id` field (the slug) of the target entity
- References are bidirectional: a deal references its company, and the company references the deal
- Both sides are updated together in the same write operation
- If cross-references drift (e.g., a deal references a contact the contact doesn't reference back), the weekly audit auto-repairs them

---

## Write Protocol

Every time data is created or updated, follow this sequence:

1. Write or update the entity file (e.g., `data/deals/acme-enterprise.md`)
2. Update the corresponding row in `data/index.md`
3. Update any cross-referenced entity files (e.g., add the deal to the company's deals list)
4. Update those entities' rows in index.md too
5. Append one line to `data/ops.log`
6. **Verify** — Read back the index.md row for the primary entity. Confirm the key fields match the entity file (id, stage, amount, close_date for deals; id, name, industry for companies; id, name, company for contacts). If mismatch: retry the index update once. If retry fails: append a VERIFY_FAIL line to ops.log and inform the user: "I wrote the [entity] file but the index may be out of sync. The weekly audit will fix this, or you can say 'rebuild my index' now."

**Directory creation-on-demand:** If the target directory doesn't exist, create it first.

**When a write gets interrupted:** The next session's health check detects the inconsistency and offers to fix it. The verify step (step 6) catches most inconsistencies immediately, but if the session ends mid-write, the weekly audit is the backstop.

### Operation Log Format

`data/ops.log` — one line per write operation:
```
2026-03-25T14:30:00 | CREATE | data/deals/acme-enterprise.md | deal acme-enterprise created
2026-03-25T14:30:00 | UPDATE | data/companies/acme-corp.md | added deal reference
2026-03-25T14:30:00 | UPDATE | data/index.md | added deal row, updated company row
```

### Read-Time Validation

Every time the system reads a file, it checks that the header is valid. If it's not:
- Auto-repair if possible (add missing fields with defaults)
- Flag in views/health-check.md if repair isn't possible
- Never crash on bad data — degrade gracefully

---

## Signal Thresholds

high_value_deal_threshold: {{HIGH_VALUE_DEAL_THRESHOLD}}

Used by signal rules that reference deal amounts. Default: $100,000. Set to 2x your average deal size during setup.

---

## Scheduled Agents

morning_briefing_time: {{MORNING_BRIEFING_TIME}}
morning_briefing_days: weekdays
weekly_audit_day: Sunday
weekly_audit_time: 06:00

**Rules:**
- Background agents are READ-ONLY on `data/`. They write ONLY to `views/`.
- Morning briefing reads `data/index.md`, runs signal detection, regenerates all views, writes `views/morning-briefing.md`.
- Weekly deep audit reads ALL entity files, validates schemas, checks cross-references, rebuilds index.md, writes `views/health-check.md`.
- All times are in your local device timezone.
- If your computer is off when a scheduled check was supposed to run, it catches up when you open your laptop.

---

## Tips & Messaging

tips_enabled: true
tips_shown: []
referral_enabled: true
onboarding_week: 1

**Tips:** Include one tip per morning briefing for the first 3 weeks (see `templates/tips.md`). Track shown tips in `tips_shown` array. Stop when all 18 tips shown OR `onboarding_week` > 3. If the user says "stop showing tips" → set `tips_enabled: false`.

**Contextual tip selection:** After onboarding week 1, prefer tips based on `capability_usage` below. Prioritize tips about capabilities the user has NOT used yet. If the user has never run a meeting prep, surface the prep tip next. If they've never captured a quick note, surface the note tip. This ensures tips teach the user about capabilities they don't know exist — not ones they've already discovered.

**Referral:** Include a one-line referral mention in the weekly summary (not daily briefing), starting after week 1. Example: "Enjoying SalesSidekick? Refer another AE and get a free month ($99 value). Details and your referral link at pipelinerebel.com/community." If the user says "stop the referral stuff" or similar → set `referral_enabled: false`. Never show again.

**Increment `onboarding_week`** each Monday during the morning briefing (compare workspace_created date to today).

---

## Capability Usage Tracking

Track which capabilities the user has used. Updated automatically after each capability fires. This data drives contextual tip selection and helps the system identify what the user hasn't discovered yet.

```yaml
capability_usage:
  today: 0
  end-of-day: 0
  weekly: 0
  closeout: 0
  prep: 0
  strategy: 0
  battle: 0
  pov: 0
  pipeline: 0
  whitespace: 0
  forecast-update: 0
  deck: 0
  outreach: 0
  email: 0
  draft-post: 0
  research: 0
  add-company: 0
  add-deal: 0
  note: 0
  coaching: 0
  audit: 0
  setup: 0
  skill-builder: 0
last_updated: {{CREATED_DATE}}
```

**Update rule:** After every capability execution, increment the count for that capability and update `last_updated`. This is a simple counter — no timestamps per use.

**How tips use this data:** When selecting the next tip to show, check `capability_usage`. Map tips to capabilities:

| Tip | Related capability | Show when |
|-----|-------------------|-----------|
| 1 (CRM screenshot) | add-company, add-deal | add-company = 0 |
| 2 (Call transcript) | closeout | closeout = 0 |
| 3 (What do you know) | research | research = 0 |
| 4 (Natural language) | (general) | Always eligible in week 1 |
| 5 (Drop in docs) | (context ingestion) | Always eligible in week 1 |
| 6 (Community trial) | (community) | Always eligible |
| 7 (What needs attention) | pipeline | pipeline = 0 |
| 8 (Prep for meeting) | prep | prep = 0 |
| 9 (Customize capabilities) | skill-builder | skill-builder = 0 |
| 10 (Knowledge bases) | (context ingestion) | Always eligible in week 2 |
| 11 (Referral program) | (community) | Always eligible |
| 12 (Custom GPT migration) | skill-builder | Always eligible in week 2 |
| 13 (Forecast) | forecast-update | forecast-update = 0 |
| 14 (Morning briefing) | today | today < 3 |
| 15 (Coaching) | coaching | coaching = 0 |
| 16 (Office hours) | (community) | Always eligible |
| 17 (Competitive analysis) | battle | battle = 0 |
| 18 (Business case) | pov | pov = 0 |

**Selection priority:** Unused capabilities first (count = 0), then least-used, then general/community tips. Never repeat a tip already in `tips_shown`.

---

## Version

plugin_version: {{PLUGIN_VERSION}}
schema_version: 2
workspace_layout_version: 1
last_version_check: {{CREATED_DATE}}
workspace_created: {{CREATED_DATE}}
last_migration: none
last_upgrade_audit: {{CREATED_DATE}}
upgrade_state: current
migration_history: []

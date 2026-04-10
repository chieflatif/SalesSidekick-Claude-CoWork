# SalesSidekick v4.2.1 Release Notes

Released: 2026-04-09

## What Changed

v4.2.1 adds the Intelligence Trail, a passive capture layer for useful context that otherwise disappears when users start a new Claude conversation.

Claude Desktop conversations are isolated. If a user casually mentions a competitor, stakeholder relationship, strategic decision, or deal context outside a structured command, that context does not persist unless SalesSidekick writes it to the workspace during the conversation. The Intelligence Trail gives SalesSidekick a safe place to preserve that context without forcing the user to remember to run a save command.

## Intelligence Trail

New trail files live in:

```text
data/trail/
```

Trail entries capture:

- what the user was discussing and why
- companies, contacts, and deals referenced
- intelligence learned in passing
- decisions or strategic direction
- whether anything was filed to an entity record

Morning briefing now scans the last 7 days of trail files and surfaces a short "What You've Been Working On" summary so users can pick up where they left off.

## Conservative Capture Policy

v4.2.1 hardens the passive capture behavior before broad rollout.

SalesSidekick now classifies each potential capture as:

- `NOOP` — no useful new business intelligence
- `TRAIL_ONLY` — useful conversational context, but not safe as canonical truth yet
- `TRAIL_PLUS_ENTITY_UPDATE` — explicit, high-confidence user-stated fact tied to an existing company/contact/deal
- `ASK_USER` — ambiguous, contradictory, sensitive, or would create/overwrite canonical records

The default is trail-only when uncertain. Casual remarks and inferred context should not overwrite canonical business records.

## Upgrade Behavior

v4.2.1 is a non-destructive layout upgrade.

Existing v4.2.0 users get:

- `data/trail/` created if missing
- `workspace_layout_version` updated from `1` to `2`
- a migration history entry for the layout change
- no changes to existing deals, contacts, call notes, research docs, patterns, or custom skills

The company schema remains version `2`; this release changes the workspace layout, not the entity schema.

## User-Facing Impact

Users may occasionally see a quiet line like:

```text
Captured: Acme competitive intel + Dan role context -> trail updated.
```

Command-driven writes still use the full FILES UPDATED block. Passive trail-only capture uses the shorter one-line confirmation so the conversation does not get noisy.

## Why This Matters

This release addresses a common complaint: "SalesSidekick doesn't remember what we talked about."

The root cause is that unstructured conversation context disappears between Claude sessions unless it is written into the workspace. The Intelligence Trail gives SalesSidekick a lightweight, local-first, reviewable memory path for context that does not fit neatly into a call note, research doc, or entity field.

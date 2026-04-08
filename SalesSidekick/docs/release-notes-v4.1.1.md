# SalesSidekick Release Notes — v4.1.1

**Release date:** 2026-04-08

This patch release does two things:

1. hardens upgrade detection and workspace migration for users coming from older installs
2. formally rolls the staged `/research` upgrade into the release narrative so the docs match what the plugin already ships

---

## What Changed In v4.1.1

### Upgrade intelligence is now much stronger

After a user installs the new plugin zip, SalesSidekick now audits the environment before declaring the upgrade complete.

It now checks:

- whether the user is in a real SalesSidekick workspace or just running standalone
- whether Global CLAUDE identity content exists but still uses the old pre-marker format
- whether `.claude/CLAUDE.md` exists in the workspace
- whether workspace version metadata is missing or stale
- whether required directories for the current version exist
- whether `data/index.md` exists and can still support the current workspace
- whether custom skills or scheduled agents need follow-up after plugin replacement

### Legacy and partial environments are handled more deliberately

The plugin now distinguishes between:

- identity-only legacy installs
- missing workspace config
- layout drift
- schema drift
- normal version upgrades
- fully current environments

That means older users are less likely to get a vague “you’re upgraded” message when what they really need is a config repair, folder creation, or migration pass.

### Workspace version tracking is more explicit

Project CLAUDE metadata now includes clearer upgrade and workspace state fields, so future releases can reason about older environments more safely.

New tracked metadata includes:

- `workspace_layout_version`
- `last_upgrade_audit`
- `upgrade_state`
- `migration_history`

### Release docs now match the real plugin

This release also cleans up the public-facing docs so they accurately describe the current plugin:

- upgraded `/research` flow is now called out explicitly
- upgrade behavior is documented more clearly
- install/build language is aligned with the current Claude Desktop flow

---

## Included From The Recent Research Upgrade

The staged `/research` capability is now formally documented as part of the release story.

### `/research` is no longer a simple summary pass

It now runs as a structured intelligence pipeline:

1. **Collect** — gather raw company, leadership, recent activity, competitive, hiring, and technology signals
2. **Extract** — reason over the collected material through four lenses
3. **Prism** — run multi-perspective analysis to surface non-obvious selling insights
4. **Assemble** — build an evidence-graded Platform Document

### Research now creates a reusable intelligence asset

When the user is in a SalesSidekick workspace, `/research` saves a Platform Document to:

`data/research/{company-slug}-platform.md`

That research then feeds:

- meeting prep
- deal strategy
- outreach
- business cases
- contextual email

### Research is now part of the compounding memory layer

Instead of dying as a one-off answer in chat, research becomes durable workspace intelligence that downstream capabilities can pull forward later.

---

## Why This Release Matters

The biggest practical risk in a local-first plugin is not only “can it save data,” but “can it safely carry older users forward when the system changes.”

This release reduces that risk by making upgrades more explicit, more inspectable, and less dependent on fragile assumptions about what the user already has on disk.

At the same time, it makes sure the release docs finally reflect the real value of the newer research workflow.

---

## Upgrade Notes For Existing Users

If you already have a SalesSidekick workspace:

1. install the new zip
2. open your SalesSidekick project
3. tell SalesSidekick: `I have the new version`
4. let it inspect the workspace and apply any needed repairs or migrations

Your workspace data should remain intact:

- deals
- contacts
- call notes
- tasks
- knowledge bases
- custom skills

The plugin brain updates. The workspace is inspected and brought forward.

---

## Recommended Smoke Checks After Upgrade

After upgrading, these are the fastest checks:

- `What's on my plate today?`
- `Research Acme`
- `Tell me about Globex`
- paste a recent call transcript
- `How should I approach this deal?`

If the workspace needs repair, the plugin should surface that before pretending everything is current.

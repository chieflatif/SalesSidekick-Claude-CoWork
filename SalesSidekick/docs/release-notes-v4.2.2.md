# SalesSidekick v4.2.2 Release Notes

Released: 2026-04-10

## What Changed

v4.2.2 packages a stronger Global CLAUDE identity layer directly into SalesSidekick.

Before this release, SalesSidekick already had the mechanics for global identity, workspace upgrades, and the Intelligence Trail. What it did not have was a richer packaged Tier 1 template equivalent to the stronger global instruction layer used in Pipeline Rebel.

This release fixes that.

## Packaged Global Identity Template

SalesSidekick now ships with:

```text
templates/global-identity.md
```

This template is the source of truth for the Global CLAUDE block written inside:

```text
/mnt/.claude/CLAUDE.md
```

within:

```text
<!-- SALESSIDEKICK-IDENTITY-START -->
<!-- SALESSIDEKICK-IDENTITY-END -->
```

## What The Global Layer Now Includes

The packaged Tier 1 block now includes:

- who the user is and what they sell
- the cascading context architecture
- operating-system state and memory protection
- setup drift and upgrade drift guardrails
- evidence grading rules
- anti-slop and communication rules
- customer-language guidance
- decision authority and personality defaults

## Why This Matters

SalesSidekick already had:
- persistent workspace memory
- upgrade audits
- marker-based global identity
- the Intelligence Trail

What was missing was a stronger, packaged global instruction layer that could be written consistently during onboarding and refreshes.

v4.2.2 makes that layer explicit and repeatable.

## User Impact

For new users:
- setup now writes a stronger Tier 1 global block from day one

For existing users:
- rerunning setup or asking SalesSidekick to refresh settings will rewrite the global block using the packaged template

## Upgrade Behavior

This release does not change schema or workspace layout.

- schema version stays `2`
- workspace layout version stays `2`
- no changes to existing deals, contacts, call notes, research docs, patterns, or trail files

This is a packaging and guardrail release, not a data migration release.

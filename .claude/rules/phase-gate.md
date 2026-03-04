# Phase Gate Checklist

Use this checklist at the end of every build phase. ALL items must pass before committing.

## File Inventory
- [ ] All files specified for this phase exist
- [ ] All files are non-empty and contain real content (no placeholders)
- [ ] File count matches phase target from dev plan

## Content Integrity
- [ ] Command files: YAML frontmatter present and valid (description, argument-hint)
- [ ] Command files: All 9 standard sections present (Purpose, When to Use, Inputs, Execution Steps, Output Format, Database Read/Write, Commandment Alignment, Evidence Grading, Graceful Degradation)
- [ ] Skill files: YAML frontmatter present and valid (name, description)
- [ ] Skill files: All 5 body sections present (Auto-Fire Trigger, Purpose, When Referenced, Core Framework, Personalization Notes)
- [ ] Consistent markdown heading hierarchy across all files

## Zero Leakage Scan
Run these searches across ALL project files:
- [ ] `grep -ri "andré" .` — ZERO results
- [ ] `grep -ri "gouyet" .` — ZERO results
- [ ] `grep -ri "mapbox" .` — ZERO results (except in template pattern examples)
- [ ] `grep -ri "netflix\|bloomberg\|cnn\|darden" .` — ZERO results
- [ ] No hardcoded territory sizes, account names, personal context

## Template Variable Consistency
- [ ] All template variables use `{{DOUBLE_CURLY_BRACES}}` syntax
- [ ] Variable names are consistent across files (no `{{COMPANY}}` vs `{{COMPANY_NAME}}` conflicts)
- [ ] Every variable used is documented in CLAUDE.md Section 13 (Personalization Variables)
- [ ] Every variable has a corresponding `/setup` question that populates it

## Cross-Reference Integrity
- [ ] Every command referenced in CLAUDE.md has a corresponding .md file in commands/
- [ ] Every skill referenced by a command has a corresponding SKILL.md in skills/
- [ ] Every Notion database field mentioned is documented in notion/SKILL.md
- [ ] No orphaned files (every file is referenced by something)

## Commandment Compliance
- [ ] Every command documents which Commandments it serves
- [ ] No command violates any Commandment
- [ ] Evidence grading specified on commands that generate quantitative claims
- [ ] The 50% Rule is referenced where applicable

## Graceful Degradation
- [ ] Every command documents behavior WITH and WITHOUT each relevant connector
- [ ] No command completely breaks without an optional connector
- [ ] Fallback behaviors are useful, not just error messages
- [ ] System works with ZERO connectors on day one

## Audit
- [ ] Audit subagent has been run on all phase files
- [ ] All P1 findings resolved
- [ ] All P2 findings resolved (or deferred with documented justification)
- [ ] P3 findings logged in project memory

## Ready to Commit
- [ ] All above checks pass
- [ ] Phase deliverables confirmed with user
- [ ] Commit message follows format: `Phase N: [description]`

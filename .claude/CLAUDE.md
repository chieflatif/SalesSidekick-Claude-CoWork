# SalesSidekick — Development Rules & Governance

## Project Overview

- **Product:** SalesSidekick v2.0 — a self-personalizing AI sales plugin for Claude Cowork
- **Owner:** Latif Horst (Pipeline Rebel / chieflatif)
- **Repository:** `https://github.com/chieflatif/SalesSidekick-Claude-CoWork`
- **Nature:** All-markdown plugin (no traditional code). Quality = content integrity, not compilation.
- **Source of Truth:** `VALIDATED-BUILD-SPEC.md` is the CANONICAL reference. It supersedes both the Product Bible and Dev Plan where they conflict.
- **Reference docs:** `SALESSIDEKICK-DEV-PLAN.md` (original build guide) + `SalesSidekick-Product-Bible-v2.docx` (original product spec)

## Repo Strategy

- **This workspace** (`/Users/latifhorst/Cowork-Sidekick/`) = development/planning environment. NOT published.
- **Plugin build directory:** `SalesSidekick/` subdirectory within this workspace. All plugin files go here.
- **Clean public repo:** `github.com/chieflatif/SalesSidekick-Claude-CoWork` — receives ONLY the contents of `SalesSidekick/`.
- **Planning artifacts NEVER enter the plugin directory:** Dev plan, Product Bible, validated build spec, `.claude/` governance, memory files — all stay in the workspace root.
- **The user downloads a clean plugin folder.** No dev tooling, no audit logs, no planning docs.

---

## Build Phase Lifecycle

Every phase follows five steps. Do not skip any.

### 1. Review Spec (BEFORE writing any files)

- Re-read the relevant section of `VALIDATED-BUILD-SPEC.md` for the current phase, supplemented by `SALESSIDEKICK-DEV-PLAN.md` for additional detail where needed.
- Identify all files to be created/modified and their dependencies.
- List what exists, what will be created, and what assumptions you are making.
- Confirm the phase scope with the user before writing.

### 2. Build

- Create files in dependency order (referenced files before referencing files).
- Follow the exact file format specified in `VALIDATED-BUILD-SPEC.md` (YAML frontmatter for commands, 6-section structure for skills).
- Use `{{TEMPLATE_VARIABLES}}` for all personalizable content — never hardcode.
- Every command must work with generic defaults BEFORE `/setup` runs.
- Every command must specify graceful degradation for missing connectors.

### 3. Validate (MANDATORY per-phase checks)

After completing a phase, run these checks before committing:

**Content Integrity:**
- [ ] All files specified for this phase exist and are non-empty
- [ ] YAML frontmatter is valid on all command files
- [ ] All cross-references resolve (commands reference real skills, skills exist)
- [ ] No orphaned references (nothing points to files that don't exist yet — use "Phase N" notes if forward-referencing)

**Zero Leakage:**
- [ ] No "André" or "Gouyet" anywhere in any file
- [ ] No "Mapbox" references (except as example in template patterns)
- [ ] No "Netflix", "Bloomberg", "CNN", "Darden" hardcoded references
- [ ] No hardcoded territory sizes, account names, or personal details
- [ ] All personalizable content uses `{{VARIABLE}}` syntax

**Commandment Compliance:**
- [ ] Every command aligns with at least one of the 10 Commandments
- [ ] No command violates any Commandment (especially #3 Illuminate Don't Dictate, #6 10-Minute Rule)
- [ ] Evidence grading is specified on every command that generates quantitative claims

**Structural Consistency:**
- [ ] Command files follow the standard template (Purpose, When to Use, Inputs, Execution Steps, Output Format, Database Read/Write, Commandment Alignment, Evidence Grading, Graceful Degradation)
- [ ] Skill files follow the standard template (YAML frontmatter, Auto-Fire Trigger, Purpose, When Referenced, Core Framework, Personalization Notes)
- [ ] CLAUDE.md command registry stays in sync with actual command files

### 4. Audit (MANDATORY — do not skip)

After validation passes, spawn an audit subagent before committing. The audit protocol is defined in `.claude/rules/audit-protocol.md`.

The audit must answer these questions from an adversarial perspective:
1. What did I miss or leave incomplete?
2. What assumptions could bite us?
3. Are there internal contradictions between files?
4. Did I introduce any André/prototype leakage?
5. Are cross-references and dependencies all valid?
6. Does every command degrade gracefully without connectors?
7. What should be improved — and what must be fixed NOW vs. tracked?

Priority classification:
- **P1 (Fix now)**: Missing files, broken references, André leakage, Commandment violations
- **P2 (Fix now if reasonable)**: Incomplete sections, weak graceful degradation, missing evidence grading
- **P3 (Track)**: Polish items, enhanced examples, optimization opportunities

**DEFAULT BIAS: Fix it. Only defer P3 if it's outside the current phase scope.**

After the audit:
- Fix all P1 and P2 findings immediately.
- Log P3 findings in memory for later phases.
- Re-run validation checks after fixes.

### 5. Commit & Closeout

- Commit with descriptive message referencing the phase: `Phase N: [description]`
- Update project memory with phase completion status.
- Confirm all phase deliverables with the user before moving to next phase.

---

## The 10 Commandments (Structural, Not Decorative)

These are the operating doctrine. Every file must respect them.

| # | Commandment | Enforcement |
|---|-------------|-------------|
| 1 | Speed is Life | Commands produce output fast. /skill-builder rejects >10 min commands. |
| 2 | Stop Digging, Start Orchestrating | AI does the research, human orchestrates decisions. |
| 3 | Illuminate, Don't Dictate | Always offer options (Three Paths). Never make strategic decisions for the user. |
| 4 | Context is King | Never reach out cold. Always load context first. |
| 5 | Voice First | Assume voice-dictated input. Parse intent, not grammar. |
| 6 | The 10-Minute Rule | Every task breakable into ≤10 min chunks. |
| 7 | One Source of Truth | Notion is backbone. No dual-write. Export-first for CRM. |
| 8 | The Laws of Karma | Never fabricate. Evidence grade everything. /audit enforces integrity. |
| 9 | Don't Pitch Features, Pitch Angles | Outreach = angles, POVs = blind spots, strategy = wedges. |
| 10 | Be the Chief of Staff | Nothing falls through. AE never asks "what's next?" |

---

## Evidence Grading (Non-Negotiable)

Any command that generates quantitative claims MUST implement:

| Grade | Definition | Language Pattern |
|-------|-----------|-----------------|
| Verified | From case studies, public data, customer-confirmed | Direct statement with source |
| Estimated | Calculated from available data with assumptions | "Based on... we estimate" |
| Hypothesis | Projections based on industry patterns | "We typically see" / "Industry suggests" |

**The 50% Rule:** If >50% of math in a POV/strategy is hypothesis-grade, flag it for more research.

---

## Safety Rules

1. **No secrets or API keys in any file.** Use `{{NOTION_API_KEY}}` template variables.
2. **No personal data leakage.** Everything personalizable goes through template variables.
3. **Template variables use `{{DOUBLE_CURLY_BRACES}}`** — obvious and easy to find/replace.
4. **Every command works at three levels:** (a) generic defaults before /setup, (b) personalized after /setup, (c) degraded without connectors.
5. **The README sells the product.** Treat it as a first impression, not an afterthought.

---

## Coding Standards (Adapted for Markdown Plugin)

1. Prefer small, phase-scoped increments. One phase = one commit.
2. Preserve consistency across files — if you change a pattern, change it everywhere.
3. Run validation after every file change. Fix issues immediately.
4. No placeholder content ("TBD", "TODO", "Coming soon") in committed files — every section must have real content or be explicitly noted as a forward reference to a specific phase.
5. No dead references, no orphaned files, no commented-out sections.
6. YAML frontmatter must be valid and complete on every command file.
7. Consistent heading hierarchy (H1 for title, H2 for sections, H3 for subsections).

---

## File Inventory Tracking

Maintain a running count against the target: **33+ files across 4 categories.**

| Category | Target | Files |
|----------|--------|-------|
| Core Identity | 4 | CLAUDE.md, .mcp.json, CONNECTORS.md, plugin.json |
| Commands | 22 | setup.md through skill-builder.md |
| Skills | 11 | profile/ through posting-guide/ |
| Distribution | 5+ | README.md, INSTALLATION-GUIDE.md, QUICK-START.md, LICENSE, docs/ |

After each phase, verify: files created vs. files expected.

---

## Git Discipline

- One commit per completed phase (after validation + audit pass).
- Commit message format: `Phase N: [brief description of deliverables]`
- `.gitignore` covers: `.env`, `node_modules/`, `.DS_Store`, `*.log`
- No secrets, API keys, or personal data in commit history.
- Before each commit: scan all files for André/Mapbox/prototype leakage.

---

## Key Files Reference

| File | Purpose |
|------|---------|
| `SALESSIDEKICK-DEV-PLAN.md` | Build instructions — how to structure everything |
| `SalesSidekick-Product-Bible-v2.docx` | Product specification — what goes in each file |
| `.claude/CLAUDE.md` | THIS FILE — development rules and governance |
| `.claude/rules/audit-protocol.md` | Audit subagent prompt for phase reviews |
| `.claude/rules/phase-gate.md` | Phase completion checklist template |

---

## User Preferences

- No-code vibe coder using speech-to-text (expect typos in instructions)
- Minimal technical debt — fix P1/P2 immediately
- Audit at milestones, not blocking every step
- Systematic execution with clear phase gates
- NEVER fabricate content. Every framework, rubric, and methodology must be grounded.
- Documentation and process are first-class priorities
- Present results and get confirmation before moving phases

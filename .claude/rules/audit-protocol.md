# Post-Phase Audit Protocol

After EVERY phase completion, you MUST spawn an audit subagent before committing. This is non-negotiable.

## When to Run

- After completing all files for a build phase
- Before committing phase work to git
- When the user requests an ad-hoc audit on specific files

## How to Run

Use the Agent tool with `subagent_type: "general-purpose"` and the prompt below. Pass it the list of files created/modified in the phase.

## Audit Subagent Prompt

Copy this prompt into the Agent tool, replacing `{PHASE_NUMBER}`, `{PHASE_NAME}`, and `{FILES}` with actuals:

```
You are a content audit agent for the SalesSidekick Claude Cowork plugin build. Your job is to find problems the implementing agent missed. You are adversarial — assume the content has gaps until proven otherwise.

This is a markdown-based plugin (no traditional code). Quality means: content integrity, cross-reference accuracy, template variable consistency, Commandment compliance, and evidence grading enforcement.

## Phase Under Review: Phase {PHASE_NUMBER} — {PHASE_NAME}

## Files to Audit: {FILES}

Read EVERY file listed above completely. Also read CLAUDE.md and VALIDATED-BUILD-SPEC.md for context (supplemented by SALESSIDEKICK-DEV-PLAN.md for additional detail).

For EACH file, answer ALL of these questions:

### 1. What is missing or incomplete?
- Sections that are empty, placeholder, or lack real content
- Required template sections missing (Purpose, Process, Output Format, etc.)
- YAML frontmatter fields missing or invalid (for command files)
- Graceful degradation not specified
- Evidence grading not addressed where quantitative claims exist

### 2. What assumptions could bite us?
- Assumptions about connector availability
- Assumptions about data existing in Notion before it's been created
- Assumptions about user knowledge or context
- Forward references to files/phases that don't exist yet (acceptable if noted)

### 3. Are there internal contradictions?
- Command says it uses a skill that doesn't match the skill's "When Referenced" list
- CLAUDE.md command registry doesn't match actual command files
- Template variable names inconsistent across files (e.g., {{COMPANY}} vs {{COMPANY_NAME}})
- Conflicting instructions between command process steps and skill frameworks

### 4. André/Prototype Leakage Check
- Search every file for: "André", "Gouyet", "Mapbox", "Netflix", "Bloomberg", "CNN", "Darden"
- Search for: hardcoded territory numbers, specific account names, personal details
- Search for: any content that assumes a specific user, company, or industry
- Flag ANY instance. Zero tolerance.

### 5. Cross-Reference Validation
- Every skill referenced by a command → does that skill file exist (or is it in a future phase)?
- Every command listed in CLAUDE.md → does the command file exist?
- Every Notion database field mentioned → is it documented in the notion/SKILL.md schema?
- Every template variable used → is it listed in CLAUDE.md Section 13 (Personalization Variables)?

### 6. Graceful Degradation Review
- Does every command specify what happens WITHOUT Notion?
- Does every command specify what happens WITHOUT Calendar/Gmail/Drive?
- Are fallback behaviors useful (not just "feature unavailable")?
- Does the system work with ZERO connectors on day one?

### 7. Improvement Opportunities
For each finding, classify as:
- **P1 (Fix now)**: Missing files, broken references, André leakage, Commandment violations, empty required sections
- **P2 (Fix now if reasonable)**: Weak graceful degradation, incomplete evidence grading, inconsistent template variables
- **P3 (Track for later)**: Polish, enhanced examples, deeper framework content, optimization

DEFAULT BIAS: Fix it now. Only defer to P3 if it's outside the current phase scope.

## Output Format

For each finding:
- File name
- What the problem is (specific, not vague)
- Why it matters for the plugin
- Priority (P1/P2/P3)
- Suggested fix

End with a summary: total findings by priority, and whether the phase is safe to commit or needs fixes first.
```

## What to Do With Audit Results

1. **P1 findings**: Fix immediately. Do not commit until resolved.
2. **P2 findings**: Fix now unless it requires work clearly scoped to a different phase. If deferring, note it in project memory with justification.
3. **P3 findings**: Add to project memory for the relevant future phase.
4. Re-run validation checks after fixes.
5. Only commit after all P1 and P2 findings are resolved.

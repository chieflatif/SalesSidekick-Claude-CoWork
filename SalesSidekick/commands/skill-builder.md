---
description: Custom command creation — 5-phase scaffold with Commandment validation and audit
argument-hint: "[description of command to create]"
---

# /skill-builder — Custom Command Creation

## Purpose

Enables the AE to create new custom commands that extend SalesSidekick's capabilities. Uses a 5-phase process: Intent → Commandment Check → Scaffold → Audit → Install. Ensures every new command meets the system's structural requirements, follows the 10 Commandments, and integrates properly with existing commands and skills.

## When to Use

- AE has a repeating workflow that should be automated
- A unique process not covered by the 22 built-in commands
- Manager or team has a specific reporting format needed
- Industry-specific workflow that needs a structured command

## Inputs

**User provides:** Description of what the new command should do (required). Examples:
- `/skill-builder I need a command that generates a mutual action plan for late-stage deals`
- `/skill-builder create a command for weekly competitor monitoring`
- `/skill-builder build me a QBR prep command`

## Execution Steps

### Phase 1: Intent Capture
1. Parse the user's description to understand:
   - What the command should do
   - When it would be used
   - What inputs it needs
   - What outputs it should produce
   - What data it needs from Notion
2. Confirm understanding: "Here's what I think you want: [summary]. Is that right?"

### Phase 2: Commandment Check
3. Validate the proposed command against all 10 Commandments:

| # | Commandment | Check |
|---|-------------|-------|
| 1 | Speed is Life | Can this complete in <10 minutes? If not, break it into sub-commands. |
| 2 | Stop Digging | Does AI do the work, not the AE? |
| 3 | Illuminate, Don't Dictate | Does it present options, not prescribe a single path? |
| 4 | Context is King | Does it load context before acting? No naked actions? |
| 5 | Voice First | Can it handle voice-to-text input? |
| 6 | 10-Minute Rule | Is the output consumable in under 10 minutes? |
| 7 | One Source of Truth | Does it read/write to Notion (not create parallel systems)? |
| 8 | Laws of Karma | Does it evidence-grade claims? No fabrication? |
| 9 | Pitch Angles | If customer-facing, does it use angles not feature lists? |
| 10 | Be the Chief of Staff | Does it anticipate next steps? |

4. If any Commandment is violated, flag it:
   - "This command would violate Commandment [X] because [reason]. Here's how to fix it: [suggestion]."
   - Adjust the design before proceeding
5. If all pass: "All 10 Commandments pass. Proceeding to scaffold."

### Phase 3: Scaffold
6. Generate the command file using the canonical 9-section format:
   - YAML frontmatter (description, argument-hint if applicable)
   - Purpose
   - When to Use
   - Inputs (user provides + system reads)
   - Execution Steps
   - Output Format
   - Database Read/Write
   - Commandment Alignment (which commandments it serves)
   - Evidence Grading (how claims are graded)
   - Graceful Degradation (what happens with missing connectors)
7. Present the full scaffold to the user for review

### Phase 4: Audit
8. Run an automatic /audit on the scaffolded command:
   - Check for internal consistency
   - Verify database fields referenced actually exist in schemas
   - Verify skills referenced actually exist
   - Check that evidence grading is specified for quantitative outputs
   - Validate graceful degradation covers Notion (P0) at minimum
   - Check for conflicts with existing commands (overlapping purpose)
9. Present audit results: "Audit found [X] issues: [list]. Fixing..."
10. Fix any issues found

### Phase 5: Install
11. Confirm final version with user: "Ready to install. Here's the final command: [summary]. Install it?"
12. Save the command file to `commands/[command-name].md`
13. Update the Command Index in CLAUDE.md (Section 14) to include the new command
14. Confirm: "✅ /[command-name] is now installed and ready to use."

## Output Format

```
🔧 SKILL BUILDER — New Command

PHASE 1: INTENT
Proposed command: /[name]
Purpose: [1-2 sentence summary]
Category: [Daily Ops / Call Processing / Meeting Prep / Deal Strategy / Territory / Content / Communication / Intelligence / Data / Performance / Quality / System]

PHASE 2: COMMANDMENT CHECK
| # | Commandment | Status |
|---|-------------|--------|
| 1 | Speed is Life | ✅/❌ [note] |
| 2 | Stop Digging | ✅/❌ [note] |
| 3 | Illuminate, Don't Dictate | ✅/❌ [note] |
| 4 | Context is King | ✅/❌ [note] |
| 5 | Voice First | ✅/❌ [note] |
| 6 | 10-Minute Rule | ✅/❌ [note] |
| 7 | One Source of Truth | ✅/❌ [note] |
| 8 | Laws of Karma | ✅/❌ [note] |
| 9 | Pitch Angles | ✅/N/A [note] |
| 10 | Be the Chief of Staff | ✅/❌ [note] |

Result: [X/10 passed] — [proceed / needs adjustment]

PHASE 3: SCAFFOLD
[Full command file content in the 9-section format]

PHASE 4: AUDIT
- Internal consistency: ✅/❌
- Database field validity: ✅/❌
- Skill references: ✅/❌
- Evidence grading: ✅/❌
- Graceful degradation: ✅/❌
- Conflict check: ✅/❌

Issues found: [X] — [details and fixes]

PHASE 5: INSTALL
Ready to install /[command-name]?
```

## Database Read/Write

**Reads:**
- CLAUDE.md: current Command Index (to check for conflicts and update)
- Existing command files: to check for overlapping purpose
- Notion database schemas: to validate field references in new command

**Writes:**
- commands/[name].md: creates new command file
- CLAUDE.md: updates Section 14 (Command Index) with new entry

## Commandment Alignment

| Commandment | How /skill-builder Serves It |
|-------------|------------------------------|
| #1 Speed is Life | Builds commands in minutes, not hours of manual work |
| #2 Stop Digging | AI scaffolds the command — AE describes intent |
| #7 One Source of Truth | Ensures new commands integrate with Notion |
| #8 Laws of Karma | Built-in audit — every new command meets quality standards |

## Evidence Grading

- Commandment check results → Verified (checked against defined criteria)
- Audit results → Verified (checked against schemas and existing commands)
- Command quality assessment → Estimated (system judgment)

## Graceful Degradation

| Missing Connector | Impact on /skill-builder |
|-------------------|--------------------------|
| No Notion | Can still build commands, but cannot validate Notion field references. Warns: "Cannot verify database fields — make sure referenced fields exist in your Notion setup." |
| Pre-/setup | Can still build commands, but Commandment 9 (Pitch Angles) check may be limited without company-intel context. Proceeds with warning. |
| All other connectors | No impact. /skill-builder operates on the file system. |

---
description: Guided company record creation — conversational intake, Hell Yes/Hell No qualification
argument-hint: "[Company name]"
intent-triggers:
  - intent: add-account
    phrases:
      - "new prospect"
      - "add [company]"
      - "new account"
      - "track this company"
---

# /add-company — Add Company Record

## Purpose

Guided creation of a new company record. Uses a sequential conversational intake to gather information, applies Hell Yes / Hell No qualification scoring, and creates a properly structured account record. This is the only command that creates company records — ensures data consistency and qualification discipline.

## When to Use

- Adding a new prospect to the territory
- After /research surfaced a company worth tracking
- When a lead comes in and needs to be properly recorded
- Before /add-deal — a company must exist before creating a deal

## Inputs

**User provides:** Company name (required). Optionally: additional details upfront ("add Acme Corp, enterprise SaaS company in fintech, 500 employees").

**System reads:**
- Web search: basic company info if not provided by user
- Companies: check if company already exists (prevent duplicates)

## Execution Steps

1. Check if company already exists in records:
   - If found: "I already have [Company] in my records. Want me to show the existing record? Or is this a different [Company]?"
   - If not found: proceed with creation
2. If user provided details upfront, pre-fill what's available
3. If user provided only the company name, gather information conversationally:
   - **Required fields:** Company Name (provided), Industry, Account Size
   - **Recommended fields:** Website, Territory Status, Annual Revenue (if public/known)
   - **Optional fields:** Tech Stack, Current Products, Notes
   - Don't force the user through every field — accept partial information and note what's missing
4. Run a quick web search to supplement user-provided info (if web search available):
   - Verify company website
   - Identify industry if not stated
   - Check approximate employee count for Account Size classification
5. Apply **Hell Yes / Hell No qualification**:
   - Score against {{ICP_INDUSTRY}}, {{ICP_SIZE}}, {{ICP_USE_CASE}}
   - **Hell Yes:** Clear ICP fit — right industry, right size, clear use case
   - **Evaluating:** Partial fit — some signals positive, some unclear
   - **Hell No:** Poor fit — wrong industry, wrong size, or clear misalignment
   - Present the qualification honestly: "Based on [evidence], this looks like a [Hell Yes/Evaluating/Hell No]. Here's why: [reasons]."
6. Create the company record with all gathered fields
7. Confirm creation and suggest next steps

## Output Format

```
🏢 NEW COMPANY — [Company Name]

RECORD CREATED:
| Field | Value | Source |
|-------|-------|--------|
| Company Name | [name] | User provided |
| Industry | [industry] | [User/Web search] |
| Account Size | [Enterprise/Mid-Market/SMB] | [User/Estimated] |
| Territory Status | [Active/Prospect] | Default: Prospect |
| Website | [url] | [User/Web search] |
| Annual Revenue | [if known] | [Source] |
| Tech Stack | [if known] | [Source] |
| Hell Yes/Hell No | [qualification] | System assessment |

QUALIFICATION ASSESSMENT:
[Hell Yes / Evaluating / Hell No]
- ICP Industry fit: [✅/⚠️/❌] — [reason]
- ICP Size fit: [✅/⚠️/❌] — [reason]
- ICP Use Case fit: [✅/⚠️/❌] — [reason]

FIELDS NOT YET POPULATED:
- [Field]: [how to gather — e.g., "Run /research to identify tech stack"]

NEXT STEPS:
- Run `/research [Company]` to build a full intel brief
- Run `/add-deal [Company]` when there's an opportunity
- Run `/outreach [Company]` to begin prospecting

💡 Want me to run /research next to fill in the gaps?
```

## Database Read/Write

**Reads:**
- Companies: duplicate check (search by Company Name)
- Web search: company verification and basic info

**Writes:**
- Companies: creates new record with all 12 fields populated (some may be empty if info not available):
  - Company Name (Title)
  - Territory Status (default: Prospect)
  - Industry
  - Account Size
  - Hell Yes/Hell No (qualification result)
  - Website
  - Annual Revenue (if known)
  - Tech Stack (if known)
  - Current Products (empty — populated when deal exists)
  - Last Activity (set to creation date)
  - Notes (qualification summary)
  - Primary Contact (empty — populated via /add-deal or manual)

## Commandment Alignment

| Commandment | How /add-company Serves It |
|-------------|---------------------------|
| #1 Speed is Life | Quick conversational intake — not a 20-field form |
| #7 One Source of Truth | Creates in workspace, the canonical source |
| #8 Laws of Karma | Honest qualification — Hell No is a valid answer |

## Evidence Grading

- User-provided company details → Verified
- Web search results → Verified (from public sources)
- Account Size classification (if inferred from employee count) → Estimated
- Hell Yes/Hell No qualification → Estimated (system assessment against ICP criteria)

## Graceful Degradation

| Missing Connector | Impact on /add-company |
|-------------------|------------------------|
| No workspace | Not in SalesSidekick project. Collects all information conversationally and presents it formatted. Capability still works from conversation context but nothing saves between sessions. Open your SalesSidekick workspace to save automatically. |
| No web search | Cannot supplement user info. Relies entirely on user-provided details. Notes which fields could benefit from /research. |
| All other connectors | No impact. |

---
description: Presentation generation — 5 templates, auto-selects by stage, .pptx or Gamma
argument-hint: "[Company name]"
intent-triggers:
  - intent: create-deck
    phrases:
      - "build a deck"
      - "I need slides"
      - "presentation for"
      - "make a deck"
---

# /deck — Presentation Generation

## Purpose

Generates a structured sales presentation from deal and account data. Offers 5 templates (auto-selected by deal stage or user choice), outputs 8 slides each, and produces either a native .pptx file (via PptxGenJS) or a Gamma presentation. Applies brand tokens set during /setup.

## When to Use

- Preparing for a customer-facing meeting that needs slides
- Building a POV deck for an executive sponsor
- Creating a QBR presentation for an existing customer
- Competitive displacement presentation
- Discovery meeting structure

## Inputs

**User provides:** Company name (required). Optionally: template preference, specific slides to include/exclude, or audience context.

Examples:
- `/deck Acme Corp` (auto-selects template based on deal stage)
- `/deck Acme Corp executive summary for their CFO`
- `/deck Acme Corp competitive displacement vs [Competitor]`

**System reads:**
- Companies: company profile, industry, tech stack, account size
- Deals: stage, value, MEDDPICC scores, competitor, next step
- Contacts: stakeholder map for audience awareness
- Call Notes: recent insights for relevance

**System reads from skills:**
- skills/pptx/SKILL.md: PptxGenJS pipeline, brand tokens, 5 templates × 8 slides, visual QA
- skills/gamma/SKILL.md: alternative presentation path, prompt templates, limitations
- skills/company-intel/SKILL.md: product positioning, case studies, proof points

## Execution Steps

### First-Use Calibration (runs once, first time /deck is used)
0. If this is the first time the user runs /deck, ask: "How do you prefer presentations? Data-heavy, narrative, or visual?" Use the response to set deck template preference in the profile skill. This runs once and is remembered for all future /deck calls.

### Standard Execution
1. Load company and deal context from workspace data
2. Determine the template — auto-select based on deal stage or use user preference:

| Template | Auto-Selects When | Best For |
|----------|-------------------|----------|
| **Discovery** | Stage = Prospecting or Discovery | First meetings, understanding needs |
| **POV** | Stage = Qualification | Presenting your point of view, building urgency |
| **Executive Summary** | Stage = Proposal | Decision-maker presentations, business case |
| **QBR** | Deal = Closed Won (existing customer) | Quarterly business reviews, expansion |
| **Competitive Displacement** | Primary Competitor is set | Head-to-head comparisons, switchover case |

3. Build the 8-slide structure per template (see skill for detailed slide breakdowns)
4. Populate slides with account-specific content:
   - Company name, industry, and context throughout
   - Relevant proof points and case studies from company-intel skill
   - MEDDPICC-informed content (address known gaps and strengths)
   - Competitor-specific content if applicable
5. Apply brand tokens (logo, colors, fonts from /setup)
6. Determine output format:
   - If user requested Gamma or Gamma MCP is available → offer Gamma option
   - Default → generate .pptx via PptxGenJS pipeline
7. For .pptx: run visual QA pipeline (LibreOffice → PDF → JPEG) if available
8. Evidence-grade all claims in the deck
9. Present the deck with a slide-by-slide summary

## Output Format

```
📊 DECK — [Company Name] | Template: [Template Name]
Deal: [Deal Name] | Stage: [Stage] | Audience: [if specified]

SLIDE SUMMARY:
| # | Slide Title | Key Content | Evidence Grade |
|---|-------------|-------------|----------------|
| 1 | [Title] | [1-line summary] | [Verified/Estimated] |
| 2 | [Title] | [1-line summary] | [Verified/Estimated] |
| 3 | [Title] | [1-line summary] | [Verified/Estimated] |
| 4 | [Title] | [1-line summary] | [Verified/Estimated] |
| 5 | [Title] | [1-line summary] | [Verified/Estimated] |
| 6 | [Title] | [1-line summary] | [Verified/Estimated] |
| 7 | [Title] | [1-line summary] | [Verified/Estimated] |
| 8 | [Title] | [1-line summary] | [Verified/Estimated] |

OUTPUT: [.pptx file attached / Gamma link]

BRAND TOKENS APPLIED:
- Logo: [✅ applied / ⚠️ not set — run /setup]
- Colors: [✅ applied / ⚠️ using defaults]
- Fonts: [✅ applied / ⚠️ using defaults]

EVIDENCE SUMMARY:
- [X] Verified claims (from workspace data, company-intel)
- [X] Estimated claims (inferred from context)
- [X] Hypothesis claims (projections, assumptions)

⚠️ [50% Rule warning if applicable]

💡 Want me to adjust any slides, try a different template, or switch to Gamma?
```

## Database Read/Write

**Reads:**
- Companies: Company Name, Industry, Tech Stack, Account Size, Website
- Deals: Deal Name, Stage, Deal Value, Primary Competitor, MEDDPICC scores, Next Step
- Contacts: key stakeholders for audience awareness
- Call Notes: recent 3-5 calls for relevant insights

**Writes:** None. /deck generates presentation files. Does not modify workspace data.

## Commandment Alignment

| Commandment | How /deck Serves It |
|-------------|---------------------|
| #1 Speed is Life | Full deck in minutes, not hours of PowerPoint work |
| #4 Context is King | Every slide informed by account-specific intelligence |
| #9 Pitch Angles | Templates built around angles (Financial/Technical/Strategic), not feature lists |

## Evidence Grading

**Mandatory on this command.** All deck claims must be graded:
- Company data from workspace data → Verified
- Case studies and proof points from company-intel skill → Verified (if from real sources)
- ROI projections or estimated impact → Estimated
- Market positioning claims or competitive comparisons → Estimated or Hypothesis
- Growth assumptions or future-state projections → Hypothesis

If >50% of deck content is hypothesis-grade, warn: "This deck is heavy on assumptions. Consider running `/research [Company]` to upgrade claims before presenting."

## Graceful Degradation

| Missing Connector | Impact on /deck |
|-------------------|-----------------|
| No workspace | Not in SalesSidekick project. Asks: "Tell me about the company, your deal stage, and what this presentation needs to accomplish." Produces a generic-structure deck from user input. Capability still works from conversation context but nothing saves between sessions. |
| No PptxGenJS / no pptx skill | Cannot generate .pptx file. Produces slide content as formatted text. If Gamma is available, offers that path instead. |
| No Gamma | Cannot use Gamma path. Default .pptx pipeline or formatted text output. |
| No brand tokens (pre-/setup) | Uses default styling. Notes: "Run /setup to apply your brand colors, logo, and fonts." |
| No company-intel skill | Uses generic proof points. Notes: "Run /setup to generate company-specific case studies and positioning." |
| All other connectors | No impact. |

## Proactive Data Capture

After execution, offer to persist (batched, one confirmation):

| Data | Database | Condition |
|------|----------|-----------|
| Presentation file | Google Drive (working folder) | If Drive connected — save .pptx or Gamma link |
| Deck summary | Companies (Notes field) | Always — append deck type and date to company notes |

If database doesn't exist yet, offer to create it first (see CLAUDE.md Section 14.4).

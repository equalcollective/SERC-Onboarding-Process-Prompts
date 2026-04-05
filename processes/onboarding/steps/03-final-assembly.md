# Step 03 — Final Output Assembly
## When to use: After all 3 steps (00, 01, 02) are complete. No new data — assembly only.

## Meta

| Field | Value |
|---|---|
| **Goal** | Compile all analysis into the final deliverable: 1-screen summary table + detailed analysis + context reference (separate page) |
| **Time** | No new data pulls. Uses outputs from Steps 00/01/02. |
| **Sources** | Notion only — reads previous step outputs, writes final pages |
| **Input** | Step 00 context + Step 01 hero ASIN + Step 02 hero keywords + ICAP |
| **Output** | Notion: brand onboarding page — summary table at top, detail below, context linked separately |

## Constraints
- NO new data pulls — assembly only
- Summary table must fit on 1 screen — self-contained, someone reads ONLY this and understands the brand
- Client goal must be verbatim — never paraphrase
- Every field must be populated — if data unavailable, write "DATA GAP: [reason]"
- Constraints field must include ALL declining metrics, not just the top one
- Hero root groups must show 3-4 groups
- Branded KW health must include purchase share % and status label
- Follow output-schemas/summary-table.md exactly
- Update the brand's Status in Onboarding Outputs database to "Complete"

---

## The Prompt

```
Assemble the final onboarding output for [BRAND_NAME].

Read all previous outputs from Notion:
1. Step 00 context (brand, product, market baseline)
2. Step 01 hero ASIN analysis (hero ASIN, constraints, potential)
3. Step 02 hero keywords + ICAP (root groups, hero groups, branded, ICAP diagnosis)

You are assembling — not analyzing. All decisions are already made.
Your job is to compile them into the final deliverable format.

===== COMPONENT 1: SUMMARY TABLE (1 SCREEN) =====

Follow output-schemas/summary-table.md exactly.

| Field | Value |
|-------|-------|
| Brand Name | [from Step 00] |
| Client Goal | [VERBATIM from Step 00 — do not paraphrase] |
| Hero ASIN | [ASIN + product name from Step 01] |
| Why Hero | [1 sentence: revenue % + potential from Step 01] |
| Constraints (Ranked) | [ALL declining metrics from Step 01, ranked by severity. Format: CRITICAL/HIGH/MEDIUM/LOW: metric + direction + horizon] |
| ICAP Diagnosis | [from Step 02: funnel stage + keyword group + brand vs market gap] |
| Hero Root Groups | [3-4 groups from Step 02 with volume + impression share each] |
| Branded KW Health | [from Step 02: top branded keywords + purchase share + status] |
| Seasonal? | [from Step 01: yes/no + YoY evidence] |
| Market Position | [from Step 02: total search volume + brand purchases + brand share on hero keywords] |

Rules:
- Every field populated. If data unavailable: "DATA GAP: [reason]"
- Constraints = ALL declining metrics, not just the worst one
- Client Goal = verbatim, never interpreted
- Hero Root Groups = 3-4, never fewer
- This table must be self-contained — readable without any other page

===== COMPONENT 2: DETAILED ANALYSIS =====

Below the summary table on the same page, include:

## Hero ASIN Detail
- Full hero ASIN summary table from Step 01
- Child ASIN breakdown
- All constraints with trend data across time horizons
- Potential scoring for all candidates (why hero was chosen, why others weren't)

## Hero Keywords Detail
- All root keyword groups (tiered) from Step 02
- Hero root groups (3-4) with full metrics
- ICAP diagnosis table connecting constraints to keyword-level causes
- Keyword trends (MoM, YoY, share direction, seasonality)

## Branded Keywords
- Branded keyword table from Step 02 with purchase share and status

Add brief inline commentary explaining:
- Why this ASIN was selected as hero (cite the numbers)
- What the primary funnel blocker means practically
- What the hero root groups tell us about where to focus

===== COMPONENT 3: CONTEXT REFERENCE (SEPARATE PAGE) =====

The Step 00 context output should already be saved as a separate
Notion page. Verify it exists. If not, create it from Step 00 data.

This page is internal reference only — not part of the main
deliverable. Link to it from the detailed analysis page but
don't embed the content.

Contents:
- Brand context table (10+ fields from Step 00)
- Product list with revenue
- Top 100 keywords
- Google research notes
- Sales doc extracts
- Data gaps flagged

===== NOTION STRUCTURE =====

Brand's onboarding page should now have:

1. SUMMARY TABLE (top of page — first thing visible)
2. DETAILED ANALYSIS (below summary — scroll to see)
   - Hero ASIN Detail
   - Hero Keywords Detail
   - Branded Keywords
3. LINK to Context Reference (separate page)

===== FINALIZE =====

1. Update the brand's row in "Onboarding Outputs" database:
   - Status → "Complete"
   - Hero ASIN → [ASIN + product name]
   - Hero Root Groups → [3-4 group names]
   - Primary Constraint → [top severity constraint]
   - Client Goal → [verbatim]

2. Confirm in chat:
   - "Onboarding complete for [BRAND_NAME]"
   - Link to the summary page
   - Link to the context reference page

3. Chat summary (5 lines):
   - Hero ASIN and why (1 sentence)
   - Top 2 constraints by severity
   - 3-4 hero root groups
   - Branded keyword health
   - Seasonal or not
```

## Data Sources Used
- **Notion MCP only**: read Steps 00/01/02 outputs, write final pages, update database row
- No Metrics Engine, SQP, or Metabase calls — assembly only

## What This Step Produces
- Summary table (1 screen) — the primary deliverable
- Detailed analysis page — for those who want to dig in
- Context reference link — internal only
- Updated Onboarding Outputs database row — status tracking

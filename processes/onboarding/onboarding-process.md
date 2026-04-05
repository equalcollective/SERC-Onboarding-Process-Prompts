# SERC Onboarding Process - Master Process Document v4

## Goal

Produce the following deliverables for every new brand onboarding:

1. **Hero ASIN** - the single ASIN (parent or child) that represents the highest-leverage optimization target
2. **Hero Keywords** - 3-4 root keyword groups that drive the most volume and opportunity for the hero ASIN
3. **Constraints** - every declining metric ranked by severity, connected to keyword-level causes via ICAP diagnosis
4. **Brand Context Summary** - a one-screen reference combining revenue picture, market position, seasonality, and strategic direction

---

## Global Constraints

These rules apply to every step. Violations invalidate the analysis.

### Time Horizons

All metrics must be pulled across ALL of the following horizons:

| Horizon | Use |
|---------|-----|
| 3 months | Recent momentum, tactical window |
| 6 months | Medium-term trend confirmation |
| 9 months | Extended trend, pre-seasonal context |
| 12 months | Full annual picture |
| YoY (Year-over-Year) | Seasonality-adjusted comparison |
| MoM (Month-over-Month) | Short-term directional change |
| QoQ (Quarter-over-Quarter) | Quarterly momentum |
| WoW (Week-over-Week) | Immediate trend detection |

### Granularity

- **Weekly** granularity for all SQP and trend data
- **Monthly** granularity for all Metrics Engine revenue and sales data
- Both granularities must be present; never use only one

### Incomplete Data Flagging

- If any time horizon returns no data or partial data, flag it explicitly in the output
- Never silently omit a horizon - mark it as "DATA GAP: [reason]"
- If the most recent week/month is incomplete, note it but still include the partial data

### Seasonality Rules

- Always check YoY comparisons before declaring a trend "declining" or "growing"
- A metric declining MoM but flat or growing YoY may be seasonal, not a real decline
- Flag all seasonal patterns with evidence (YoY comparison showing same-period dip)
- If Q4/holiday data is in the window, call it out explicitly

### Branded Keyword Rules

- Branded keywords (containing the brand name or obvious brand misspellings) must be separated in all analysis
- Never mix branded and non-branded keyword metrics
- Branded keywords get their own section with purchase share as the primary health metric
- A branded purchase share below 70% is a warning; below 50% is critical

---

## Tech Stack

| Tool | Role | Key Operations |
|------|------|----------------|
| **Metrics Engine MCP** | Revenue, units, ads, profitability data | `list_sellers`, `list_metrics`, `query_metrics`, `compare_periods` |
| **SQP Analyzer MCP** | Search query performance, keyword share-of-voice | `list_brands`, `search_queries`, `get_trends_*`, `get_volume_alerts` |
| **Notion MCP** | Output delivery, structured pages | `create-pages`, `create-database`, `fetch`, `search` |
| **Metabase** | Product data, listing quality, variation families | SQL queries via `asin-product-data` skill |

---

## Step 00: Context Gathering

### Purpose
Build a 360-degree understanding of the brand before touching any analysis. This step produces the **Brand Context Summary** that feeds every subsequent step.

### Data Sources (in order)

1. **Google Search** - search "[Brand Name] Amazon" and "[Brand Name] [category]" to understand public positioning, price point, product range, competitor landscape
2. **Sales/Strategy Document** - if the client provided a brief, goals doc, or strategy deck, read it and extract: stated goals (verbatim), priority ASINs, known issues, seasonal calendar
3. **Metrics Engine 8-Week Snapshot** - pull the last 8 weeks of revenue, units, and ad spend at the seller level using `query_metrics` with weekly granularity to get a quick health read
4. **SQP Top 100 Keywords** - pull the top 100 keywords by search volume for the brand using `search_queries` with `limit: 100` to understand where the brand shows up in search
5. **Listing Quality Brief** - use the `asin-product-data` skill on the top 3-5 ASINs by revenue to check title quality, image count, A+ content presence, rating, review count

### Output
- `context-output.md` schema (see output-schemas/context-output.md)
- Written to Notion as the first page of the onboarding

### What This Step Feeds Into
- Step 01 uses the revenue overview and product list to scope hero ASIN candidates
- Step 02 uses the top keywords and category context to scope keyword analysis
- All steps reference the client goal to keep recommendations aligned

---

## Step 01: Hero ASIN Selection

### Purpose
Select the single highest-leverage ASIN by evaluating three dimensions. The hero ASIN is not just the biggest revenue driver - it must also have optimization potential and identifiable constraints.

### Dimension 1: Revenue Contribution

**Pull data across ALL time horizons (3/6/9/12mo + YoY + MoM + QoQ + WoW).**

1. Start at the **parent ASIN level** - query revenue, units, and ad spend for all parent ASINs
2. Rank by revenue contribution (% of total brand revenue) at each horizon
3. For the top 3-5 parents, drill into **child ASIN level** - break down revenue by child (size, color, variant)
4. Calculate:
   - Revenue at 3mo, 6mo, 12mo
   - % of total brand revenue at each horizon
   - Average monthly revenue (3mo window)
   - YoY change (same period last year vs this year)
   - MoM trend (is it accelerating or decelerating?)

### Dimension 2: Potential (from SQP)

For each candidate ASIN's primary keyword set:

1. **CVR vs Market** - is the brand's conversion rate above or below the category average? Gap = opportunity
2. **Impression Share** - what % of total impressions does the brand capture? Low share + high volume = room to grow
3. **Market Size Relative** - how large is the total search volume for this ASIN's keywords relative to the brand's other ASINs? Bigger market = bigger upside

Score each candidate: High potential = below-market CVR + low impression share + large market

### Dimension 3: Constraint Identification

**This is the most critical dimension. Identify ALL declining metrics, not just the obvious ones.**

For each candidate ASIN:

1. Pull every available metric across all time horizons
2. Flag every metric that is declining at ANY horizon (MoM, QoQ, YoY, 3mo trend, 6mo trend)
3. Rank declining metrics by severity:
   - **Critical**: Revenue declining YoY AND MoM (confirmed sustained decline)
   - **High**: CVR declining YoY OR impression share declining MoM (competitive pressure)
   - **Medium**: ATC rate declining, click share declining (funnel leaks)
   - **Low**: Single-horizon dips that may be seasonal (flag but don't alarm)
4. Cross-reference with seasonality - a Q1 dip after Q4 peak is expected, not a constraint

### Selection Logic

The hero ASIN is the one that scores highest on the combination of:
- Meaningful revenue contribution (not a tiny ASIN)
- Clear optimization potential (gaps vs market)
- Identifiable, actionable constraints (things we can actually fix)

If the top revenue ASIN has no potential or constraints, it is not the hero. The hero is the ASIN where effort will have the most impact.

### Output
- `hero-asin-output.md` schema (see output-schemas/hero-asin-output.md)
- Written to Notion as the second page

### What This Step Feeds Into
- Step 02 uses the hero ASIN's constraint list to drive ICAP diagnosis
- Step 02 uses the hero ASIN's keyword universe as the starting point for keyword analysis

---

## Step 02: Hero Keywords + ICAP Diagnosis

### Purpose
Identify the 3-4 root keyword groups that matter most for the hero ASIN, tier all keywords, separate branded from non-branded, and connect Step 01's constraints to specific keyword-level funnel blockages.

### Two-Layer Keyword Structure

**Layer 1: All Keywords Tiered at Root Group Level**

1. Pull all keywords for the hero ASIN from SQP
2. Group keywords by root term (e.g., "yoga mat", "exercise mat", "workout mat" are all under root "yoga mat" if that has the highest volume)
3. Tier every root group:
   - **Tier 1**: High volume + brand is present (impression share > 0) + strategic fit with client goal
   - **Tier 2**: Medium volume OR brand has low presence but keyword is relevant
   - **Tier 3**: Low volume or brand has negligible presence
4. For every root group, pull: volume, impression share, click share, purchase share, CTR vs market, ATC vs market, CVR vs market, MoM change, YoY change

**Layer 2: 3-4 Hero Root Groups**

From the tiered list, select 3-4 hero root groups based on:
- Highest volume among Tier 1
- Largest gap between current share and achievable share
- Direct connection to hero ASIN constraints from Step 1
- Client goal alignment

For each hero root group, provide: all constituent keywords, aggregate volume, impression share, purchase share, ICAP blocker, and a brief "Why Hero" explanation.

### Branded Keyword Separation

- Identify all keywords containing the brand name or common misspellings
- Pull purchase share for each branded keyword
- Health check:
  - Purchase share >= 70%: Healthy
  - Purchase share 50-69%: Warning - competitors capturing branded searches
  - Purchase share < 50%: Critical - brand defense needed

### ICAP Funnel Verification

ICAP = Impressions, Clicks, Add-to-Cart, Purchase. This connects Step 01 constraints to keyword causes.

For each hero root group:
1. **Impressions**: Is impression share declining? If yes, visibility is the blocker.
2. **Clicks**: Is CTR below market? If yes, listing presentation (title, main image, price, rating) is the blocker.
3. **Add-to-Cart**: Is ATC rate below market? If yes, detail page content (A+, images, bullets, reviews) is the blocker.
4. **Purchase**: Is CVR below market? If yes, checkout friction (price, shipping, trust) is the blocker.

Map each constraint from Step 01 to its keyword-level cause:
- "Revenue declining" -> which keywords lost purchase share? -> what funnel stage is leaking?
- "CVR below market" -> which root groups show the biggest CVR gap? -> is it an ATC problem or a final purchase problem?

### Output
- `hero-keywords-output.md` schema (see output-schemas/hero-keywords-output.md)
- Written to Notion as the third page

### What This Step Feeds Into
- Step 03 uses all outputs to compile the final deliverable

---

## Step 03: Final Output Assembly

### Purpose
Compile all analysis into a structured, actionable deliverable. Three components:

### Component 1: Summary Table (1 Screen)

A single table that fits on one screen and answers: "What do we know, what is the problem, and where do we focus?"

See `output-schemas/summary-table.md` for the exact schema.

This is the first thing the account manager sees. It must be self-contained - someone should be able to read only this table and understand the brand's situation and priority.

### Component 2: Detailed Analysis

The full Step 01 and Step 02 outputs, written as Notion pages with:
- All tables from hero-asin-output and hero-keywords-output schemas
- Inline commentary explaining why each decision was made
- Data source citations (which MCP tool, which date range, which parameters)

### Component 3: Context Reference (Separate Page)

The Step 00 context output, written as a separate Notion page that can be referenced but does not clutter the main analysis. This is the "background reading" page.

See `output-schemas/context-output.md` for the exact schema.

---

## Data Flow Diagram

```
STEP 00: CONTEXT GATHERING
  Reads:  Google, sales doc, client brief
  Pulls:  Metrics Engine (8-week seller snapshot)
          SQP (top 100 keywords)
          Metabase (listing quality for top ASINs)
  Produces: Brand Context Summary
  Feeds:  Step 01 (ASIN candidates, revenue baseline)
          Step 02 (keyword universe, category context)
          Step 03 (client goal, seasonal notes)

           |
           v

STEP 01: HERO ASIN SELECTION
  Reads:  Brand Context Summary (from Step 00)
  Pulls:  Metrics Engine (revenue at ALL horizons, parent + child)
          SQP (CVR vs market, impression share, market size)
          Metrics Engine (all metrics for constraint scan)
  Produces: Hero ASIN + Constraint List + Potential Score
  Feeds:  Step 02 (hero ASIN identity, constraint list, keyword scope)

           |
           v

STEP 02: HERO KEYWORDS + ICAP
  Reads:  Hero ASIN + Constraints (from Step 01)
          Brand Context Summary (from Step 00)
  Pulls:  SQP (all keywords for hero ASIN, full funnel metrics)
          SQP (branded keyword purchase share)
          SQP (trends: MoM, YoY for all root groups)
  Produces: Tiered Keyword List + Hero Root Groups + ICAP Diagnosis
  Feeds:  Step 03 (all outputs)

           |
           v

STEP 03: FINAL OUTPUT ASSEMBLY
  Reads:  All outputs from Steps 0, 1, 2
  Pulls:  Nothing new - assembly only
  Produces: Summary Table (1 screen)
            Detailed Analysis (Notion pages)
            Context Reference (separate Notion page)
  Feeds:  Account Manager / Client
```

---

## Revision History

| Version | Date | Change |
|---------|------|--------|
| v4 | 2026-04-05 | Current version - added WoW horizon, ICAP verification, branded keyword rules, data flow diagram |

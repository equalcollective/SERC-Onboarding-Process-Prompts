# Prompt 01 — Hero ASIN Selection
## When to use: After context gathering (Prompt 00) is complete. This is the first decision prompt — identifies which ASIN(s) to prioritize.

## Meta

| Field | Value |
|---|---|
| **Goal** | Identify which ASIN(s) the team prioritizes. All energy — ad spend, listing, keywords — goes here. |
| **Time** | ALL horizons: 3/6/9/12mo + YoY monthly + YoY quarterly + QoQ + MoM + WoW (8 weeks). Both weekly AND monthly. |
| **Granularity** | Parent ASIN first, child ASIN drill-down for hero candidates. |
| **Dimension** | Narrow to individual ASINs. Deep on each. |
| **Sources** | Metrics Engine MCP (primary — revenue, sessions, CVR, ads), SQP MCP + Metrics Engine SQP ASIN-level (potential check) |
| **Input** | Completed context from Prompt 00 |
| **Output** | Notion: brand onboarding page — hero ASIN section |

## Constraints
- Goes DEEPER than Prompt 00 — new data at new time horizons, child-level drill-down
- ALL time horizons: last 3mo, 6mo, 9mo, 12mo, avg monthly revenue (3mo), same month last year, same quarter last year, QoQ, MoM, WoW
- Both weekly AND monthly granularity
- Incomplete data flagged in bold: "Data for [period] is INCOMPLETE ([X] of [Y] days)"
- Seasonality: always compare same period last year before concluding decline/growth
- Hero count is flexible — revenue + potential together, not rigid % cutoff
- Constraints = ALL declining metrics ranked by severity, not one "primary"
- NO keyword work in this step (that's Prompt 02)

---

## The Prompt

```
Select the Hero ASIN(s) for [BRAND_NAME].

Read the context from Prompt 00 first (brand context, product list,
client goals, keyword landscape). You need the product understanding
to assess which ASINs matter.

Seller ID: [SELLER_ID]
Marketplace: [MARKETPLACE]
SQP Brand: [SQP_BRAND_NAME]

===== DIMENSION 1: REVENUE CONTRIBUTION =====

Pull revenue data for ALL active parent ASINs across every time horizon.

Call: query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["br_total_sales", "br_units", "br_sessions",
           "br_cvr_pct", "br_featured_offer_pct",
           "organic_sales", "cr_organic_pct",
           "ad_spend", "ad_sales", "ad_roas",
           "cr_tacos_pct", "cr_ad_sales_pct", "br_avg_price"],
  date_range={start: [12_MONTHS_AGO], end: [TODAY]},
  granularity="monthly",
  dimensions=["parent_asin"]
)

Also pull weekly for recent momentum:
Call: query_metrics(
  same metrics,
  date_range={start: [8_WEEKS_AGO], end: [TODAY]},
  granularity="weekly",
  dimensions=["parent_asin"]
)

Also pull YoY comparison:
Call: compare_periods(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["br_total_sales", "br_units", "br_sessions", "br_cvr_pct"],
  period_a={start: [THIS_QUARTER_START], end: [THIS_QUARTER_END]},
  period_b={start: [SAME_QUARTER_LAST_YEAR_START], end: [SAME_QUARTER_LAST_YEAR_END]},
  dimensions=["parent_asin"]
)

From this data, for each active parent ASIN calculate:
- Revenue last 3mo, 6mo, 9mo, 12mo
- Avg monthly revenue (last 3 months)
- % of total brand revenue
- YoY revenue change (same quarter)
- MoM change (last month vs previous month)
- Direction: growing / declining / flat

Present as:
| Parent ASIN | Product | Rev 3mo | Rev 6mo | Rev 12mo | Avg Mo (3mo) | % Total | YoY Change | MoM | Direction |

HERO COUNT DECISION:
- Look at revenue concentration
- Flexible — not rigid % cutoff
- A 60% ASIN can be hero if it also has potential
- Revenue + potential together determine heroes
- Could be 1, 2, or 3-4 depending on brand
- WHEN REVENUE IS EVENLY SPLIT (e.g., top ASINs each at 10-20%
  with no clear dominant ASIN), you MUST select multiple heroes.
  A single hero at 17% while other ASINs sit at 15% and 14% is
  not a valid single-hero call — that's a multi-hero brand.
  In these cases, select all ASINs that meet BOTH:
    (a) meaningful revenue share (top cluster, not long tail)
    (b) growth trajectory or optimization potential
  Constraints like low Buy Box are problems to fix, NOT reasons
  to exclude an ASIN from hero status. Flag them as constraints.

For top revenue parent ASINs, drill to child level:
Call: query_metrics(
  same metrics,
  date_range={start: [3_MONTHS_AGO], end: [TODAY]},
  granularity="monthly",
  dimensions=["parent_asin", "child_asin"],
  filters={parent_asin: [HERO_CANDIDATE_ASINS]}
)

Which child variant carries the parent? Is one color/size doing all work?
Show: most recent complete month + 3mo avg + 6mo avg per child.
Not just one month — need trend visibility at child level.

HERO LEVEL DECISION:
- Default: hero ASIN = PARENT level. Child breakdown is context.
- Exception: if ONE child has 60%+ of parent revenue AND significantly
  better CVR/metrics than other children, call out that specific child
  as the "hero variant" alongside the parent.
- In output: always state the parent as hero ASIN. If a dominant child
  exists, add a note: "Hero variant: [child ASIN] — [variant name]
  carries [X]% of parent revenue with [Y] CVR vs [Z] for other variants."
- Why this matters: PPC targets child ASINs. If one variant dominates,
  PPC budget should focus there, not spread across all variants.

===== DIMENSION 2: POTENTIAL (from SQP) =====

Check potential for ALL hero ASIN CANDIDATES from Dimension 1.
This means:
- If one ASIN dominates revenue → check potential for that one
- If revenue is evenly split (multi-hero case) → check potential
  for EVERY ASIN in the top cluster. Potential determines which
  of the evenly-split ASINs are worth investing in.
- An ASIN with lower revenue but HIGH POTENTIAL can be hero
  alongside or even over a higher-revenue ASIN with no potential.
- Potential is the TIEBREAKER when revenue doesn't decide.

Three checks for potential:

CHECK 1 — CVR vs Industry:
Is brand CVR better than market average on this ASIN's relevant keywords?
Check across ALL relevant keywords (Tier 1 + Tier 2), not just Tier 1.
Tier 1 shows core intent. Tier 2 shows broader market position. Both needed.
IMPORTANT: Only check CVR on keywords where brand has meaningful
PURCHASES (5+ purchases minimum for any confidence, 20+ for high
confidence). Clicks show sample size but purchases show what matters.
Keywords with 0 purchases = no CVR data to trust.
For small brands, meaningful purchase data may exist on only
10-25 keywords — that's fine. Analyze those.
Confidence: High (20+ purchases) / Medium (5-20) / Low (<5)

CHECK 2 — Impression Share:
How visible is the brand? Low impression share = room to grow.

CHECK 3 — Market Size (relative):
Is the market meaningful RELATIVE to the brand's current revenue?
Look at: total search volume + total purchases on relevant keywords.
No fixed threshold — judge relatively.

Potential logic:
- CVR good + imp share low + market meaningful = HIGH POTENTIAL
  (product converts, market exists, just needs visibility)
- CVR good + imp share low + market tiny = low opportunity
- CVR good + imp share high = dominant, defend
- CVR bad + imp share low = needs fixing first
- CVR bad + imp share high = listing/product problem

To check, use Metrics Engine SQP ASIN-level:
Call: query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["sqp_asin_impression_share", "sqp_asin_cvr",
           "sqp_asin_ctr", "sqp_asin_purchase_share",
           "sqp_market_cvr", "sqp_market_ctr",
           "sqp_search_volume", "sqp_market_purchases"],
  date_range={start: [3_MONTHS_AGO], end: [TODAY]},
  dimensions=["asin", "search_query"],
  filters={asin: [HERO_CANDIDATE_CHILD_ASINS]}
)

Also check SQP brand-level share trend:
Call SQP MCP: get_trends_share_metrics(
  brandName=[SQP_BRAND_NAME],
  granularity="monthly"
)

Is share growing, stable, or declining?

Present potential per candidate:
| ASIN | CVR vs Market | Imp Share | Market Volume | Market Purchases | Potential |

===== DIMENSION 3: CONSTRAINT IDENTIFICATION =====

IMPORTANT: Revenue decline is a SYMPTOM, not a cause. If revenue is
down, state it as the problem statement at the top, then focus on
CAUSAL metrics below (sessions, CVR, buybox, ROAS, etc.).

For each hero candidate, check ALL causal metrics across time horizons.
Look at DIRECTION: is it going up, down, or flat?
Also briefly note healthy metrics (status: healthy) for contrast.

| Metric | Last 3mo | Last 6mo | Last 12mo | YoY same qtr | Direction | Severity | If Down |
|--------|----------|----------|-----------|--------------|-----------|----------|---------|
| Sessions | | | | | | | Traffic/visibility problem |
| CVR | | | | | | | Listing/price/competition |
| Ad sales % | | | | | | | Ad dependency changing |
| ROAS | | | | | | | Ad efficiency (CPC? targeting?) |
| TACoS | | | | | | | Becoming more ad-dependent |
| Buy box % | | | | | | | Competition/reseller |
| Organic % | | | | | | | Not building organic |

HIGHLIGHT ALL DECLINING METRICS. Order by severity.
Severity = biggest % drop x business impact.
Sessions down 50% is more severe than organic % down 5%.

Bad ROAS is a valid constraint worth solving.

SEASONALITY CHECK:
- Compare this quarter's metrics to same quarter last year
- Same dip both years = seasonal, not a problem
- This year worse = real decline
- This year better = real growth

CAMPAIGN STRUCTURE — PULL LIVE DATA (do NOT reference audit):
- Pull live campaign data from Metrics Engine FIRST:
  query_metrics(metrics=["ad_spend","ad_sales","ad_roas"],
    dimensions=["campaign","targeting"],
    date_range={start: [3_MONTHS_AGO], end: [TODAY]})
- From this data, independently determine:
  How many campaigns exist? Manual vs auto? Branded vs non-branded?
  What keywords are being targeted? What's the spend distribution?
- Do NOT start from what the audit says and "confirm" it.
  Start from live data and state what you find.
- The audit is irrelevant here — live data is the truth.

Flag incomplete data in bold.

===== HERO ASIN DECLARATION =====

Based on Dimensions 1-3 TOGETHER, declare the hero ASIN(s).

DECISION LOGIC:
- ONE dominant ASIN (clear revenue leader + has potential) → 1 hero
- Revenue evenly split → check potential for ALL top cluster ASINs:
  - ASINs with revenue share + potential → hero
  - ASINs with revenue share but NO potential → not hero (just big, not growable)
  - ASINs with lower revenue but HIGH potential → can still be hero
- Potential is the tiebreaker when revenue doesn't decide
- Constraints like low buy box are problems to fix, NOT reasons to
  exclude from hero status. Flag them as constraints.

For each hero, write a "Why Hero" line (1-2 sentences citing numbers).
Do NOT write a separate prose verdict paragraph.

If 1 hero: key fields go to database properties directly.
If 2+ heroes: use one table with columns per hero.

===== OUTPUT FORMAT =====
STRICT: Follow output-schemas/hero-asin-output.md exactly.
Every table and every field must be populated. If data unavailable, write "DATA GAP: [reason]".
Do not skip tables, skip fields, add extra fields, or change the structure.
Verify your output matches the schema table-by-table, field-by-field before writing to Notion.

## Hero ASIN Summary
| Field | Hero 1 | Hero 2 (if any) |
|-------|--------|-----------------|
| ASIN (parent) | | |
| Product Name | | |
| Revenue 3mo / 6mo / 12mo | | |
| % of total revenue | | |
| Avg monthly revenue (3mo) | | |
| YoY revenue change | | |
| Potential (CVR vs market + imp share + market relevance) | | |
| Constraints (ALL declining, ranked severity) | | |
| Seasonal? (yes/no + evidence) | | |

## Child ASIN Breakdown (for hero parent)
| Child ASIN | Variant | Revenue (recent month) | Revenue (3mo avg) | Revenue (6mo avg) | % of Parent | CVR | Sessions |

## Problem Statement
Revenue decline (if any) stated here as the symptom. E.g., "Revenue -49% YoY (Q1 2025 vs Q1 2026)"

## Constraint Detail (causal metrics only — revenue is not a constraint, it's the symptom)
| Metric | 3mo | 6mo | 12mo | YoY | Direction | Severity | Confidence | Status |
Status = Declining / Healthy. Detail only the declining ones. Note healthy ones briefly.

## Potential Detail
| ASIN | Keyword | CVR vs Market | Imp Share | Search Volume | Purchases (market) |

===== SAVE =====
Save to the brand's page in the Onboarding Outputs database:
https://www.notion.so/5cb5e662750a4ad8af170d1e67592319

Write output under the "Step 01: Hero ASIN" section of the brand's page.
Update database row: Status → "Hero ASIN Done"
Any extra observations outside the schema → write to Notes property on the row.

STOP HERE. Do NOT proceed to Step 02 automatically.
Give the chat verdict below, then WAIT for human review and approval.

Chat verdict (3 sentences):
- Which ASIN is hero and why (revenue + potential)
- Top 2-3 constraints ranked by severity
- Seasonal or not
```

## Data Sources Used
- **Metrics Engine MCP**: `query_metrics` (monthly 12mo + weekly 8wk + child drill-down), `compare_periods` (YoY), SQP ASIN-level metrics
- **SQP MCP**: `get_trends_share_metrics` (share trajectory over time)

## What This Prompt Does NOT Do
- Does not identify root keywords (Step 02)
- Does not tier keywords (Step 02)
- Does not do ICAP funnel at keyword level (Step 02)
- Does not recommend campaign actions

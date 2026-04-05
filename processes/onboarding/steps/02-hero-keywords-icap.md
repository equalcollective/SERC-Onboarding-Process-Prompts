# Prompt 02 — Hero Keywords, Root Groups & ICAP Verification
## When to use: After Hero ASIN Selection (Prompt 01). Three jobs: root keyword groups, hero keywords, and ICAP problem verification.

## Meta

| Field | Value |
|---|---|
| **Goal** | Find root keyword groups + 3-4 hero root groups + tier all keywords + verify Step 2 constraints at keyword level via ICAP |
| **Time** | ALL horizons: 3/6/9/12mo + MoM + YoY + WoW. Weekly + monthly. |
| **Granularity** | Root keyword group level (primary). Individual keyword (drill-down). |
| **Dimension** | Per-keyword for hero ASIN. Deep per keyword group. |
| **Sources** | SQP MCP (brand-level trends, tagging, volume alerts) + Metrics Engine MCP (ASIN-level SQP, ROAS) |
| **Input** | Context from Prompt 00 + Hero ASIN from Prompt 01 |
| **Output** | Notion: brand onboarding page — hero keywords section. SQP: tags applied. |

## Constraints
- Goes DEEPER than Prompt 00's SQP pull — per-keyword trends, MoM, YoY, volume alerts, per-keyword ICAP
- ALL time horizons for keyword analysis
- Both weekly AND monthly granularity
- Search volume is a key factor — relevant keyword with tiny volume isn't hero material
- Branded keywords (brand name in query): ALWAYS separate. Check purchase share (should be 80-90%). NEVER hero keywords. Flag if defense needed.
- If brand has very different products across hero ASINs, filter SQP per ASIN. If similar products, brand-level is fine.
- Output at root keyword GROUP level, not individual keywords
- 3-4 hero root groups (fewer = better, more focused)
- Incomplete data flagged in bold
- No campaign action recommendations — just identification. Campaign analysis uses this as pre-fed input later.

---

## The Prompt

```
Select Hero Keywords for [BRAND_NAME].

Read context from Prompt 00 (product understanding, keyword landscape)
and hero ASIN output from Prompt 01 (which ASIN, constraints, potential).

Seller ID: [SELLER_ID]
Marketplace: [MARKETPLACE]
SQP Brand: [SQP_BRAND_NAME]
Hero ASIN(s): [HERO_ASIN_LIST from Prompt 01]

===== JOB 1: ROOT KEYWORDS + HERO KEYWORDS + TIERING =====

STEP 1 — Pull all keywords for hero ASIN:

From SQP MCP (brand-level, broad pull):
Call: search_queries(
  brandName=[SQP_BRAND_NAME],
  limit=100
)

From Metrics Engine (ASIN-level, deep pull):
Call: query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["sqp_asin_impression_share", "sqp_asin_clicks",
           "sqp_asin_impressions", "sqp_asin_purchases",
           "sqp_asin_cart_adds", "sqp_asin_cvr",
           "sqp_asin_ctr", "sqp_asin_atc_rate",
           "sqp_asin_purchase_share", "sqp_asin_cart_share",
           "sqp_market_cvr", "sqp_market_ctr",
           "sqp_market_atc_rate", "sqp_market_purchases",
           "sqp_market_impressions", "sqp_search_volume"],
  date_range={start: [3_MONTHS_AGO], end: [TODAY]},
  dimensions=["asin", "search_query"],
  filters={asin: [HERO_CHILD_ASINS]}
)

STEP 2 — Separate branded keywords:

Identify all keywords containing the brand name.
For branded keywords:
- Check brand purchase share (should be 80-90%)
- If purchase share < 80%: FLAG "Branded purchase share is [X]%
  — defense campaign may be needed"
- Branded keywords are NEVER hero keywords
- Set them aside for the branded output table

STEP 3 — Filter non-branded for relevancy:

For each non-branded keyword:
- KEEP if: the hero ASIN's product is a genuine, direct answer
  to this search query
- REMOVE if: different product type, attribute mismatch,
  wrong category, or irrelevant despite sharing a word
- Use product context from Prompt 00 to judge
- Search volume matters: relevant but tiny volume = note it
  but don't prioritize

STEP 4 — Group by root keyword:

Root keyword = the 1-3 word base term defining search intent.
Group all variations under their root.

Example:
  Root: "goal planner"
  Group: "goal planner", "goal planner 2026", "goal planner
  for women", "best goal planner", "90 day goal planner"

Per root group, calculate AGGREGATE metrics:
- Total search volume (sum of all keywords in group)
- Total brand impressions, clicks, cart adds, purchases
- Total market impressions, clicks, cart adds, purchases
- Brand impression share (brand imp / market imp)
- Brand click share (brand clicks / market clicks)
- Brand purchase share (brand purchases / market purchases)
- Brand CTR vs Market CTR
- Brand ATC rate vs Market ATC rate
- Brand CVR vs Market CVR

STEP 5 — Tier all keyword groups:

Tier 1: Root group directly describes hero product +
  conversion evidence (brand has ATCs or purchases in SQP) +
  meaningful search volume. High relevance, high confidence.

Tier 2: Broader category + SQP data inconclusive or low +
  still relevant to the product. Moderate confidence.

Tier 3: Generic, very high volume, low intent match.
  Brand would be needle in haystack.

STEP 6 — Select 3-4 hero root groups:

From Tier 1 root groups, select the top 3-4 MUST-WIN groups.
- Highest relevance + highest volume + strongest conversion
- These are root GROUPS, not individual keywords — captures
  all interrelated keywords under one root
- Fewer is better = focused execution

Selection criteria:
- Is this root group directly describing our hero product?
- Does SQP show we convert (or have potential to convert)?
- Is search volume meaningful relative to our revenue?
- 3-4 groups max. These are where ALL keyword energy goes first.

STEP 7 — Tag in SQP:

Call SQP MCP: manage_query_tags for each keyword:
- Tier 1 keywords → tag "Tier 1"
- Tier 2 keywords → tag "Tier 2"
- Tier 3 keywords → tag "Tier 3"
- Branded keywords → tag "Branded"

If multiple hero ASINs with VERY DIFFERENT products:
- Tag per ASIN: e.g., "Planner-Tier1", "HabitCards-Tier1"
- Only if products are genuinely different
- If similar products, no need for ASIN-specific tags

Confirm tags applied.

===== JOB 2: ICAP PROBLEM VERIFICATION =====

This connects Prompt 01's metric-level constraints to
keyword-level causes. Step 2 said "CVR going down."
Step 3 says "On 'goal planner' group, ATC rate is 4% vs
market 8% — listing content problem on this keyword."

For each HERO ROOT GROUP, analyze the ICAP funnel:

| Stage | Brand Metric | Market Metric | If brand << market |
|-------|-------------|--------------|-------------------|
| Impressions | Impression share | N/A | Presence problem → PPC |
| Clicks | Brand CTR | Market CTR | Creative problem → image/title/price |
| Add to Cart | Brand ATC rate | Market ATC rate | Listing problem → bullets/A+/reviews |
| Purchases | Brand CVR | Market CVR | Conversion problem → buybox/shipping/trust |

For each hero group, identify:
- WHERE in the funnel the brand breaks
- HOW BIG the gap is (brand rate vs market rate)
- WHAT it means practically (what to fix)

Focus logic:
- CVR good + impression share low → increase impressions (PPC)
- CVR not good → listing improvement first
- The answer is: which ASIN, which keyword group, which funnel stage

===== JOB 3: KEYWORD TREND ANALYSIS =====

For hero root groups, check trends across all time horizons.

From SQP MCP:
Call: get_trends_share_metrics(
  brandName=[SQP_BRAND_NAME],
  granularity="weekly"
)
Call: get_trends_share_metrics(
  brandName=[SQP_BRAND_NAME],
  granularity="monthly"
)
Call: get_trends_percentage_metrics(
  brandName=[SQP_BRAND_NAME]
)
Call: get_volume_alerts(brandName=[SQP_BRAND_NAME])

For each hero root group assess:
| Check | Period | What It Tells |
|-------|--------|--------------|
| Search volume trend | 3/6/9/12mo + MoM + YoY | Demand growing or dying? |
| Impression share trend | 3/6/9/12mo + MoM + YoY | Gaining or losing visibility? |
| Click share trend | 3/6/9/12mo + MoM + YoY | Winning more clicks? |
| Keyword seasonality | Same month/quarter last year | Seasonal keyword? |
| Volume momentum | WoW last 4-8 weeks | What's happening now? |

Flag any significant volume spikes, drops, or new keywords
from volume alerts.

Also pull ROAS for the hero ASIN from Metrics Engine:
Call: query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["ad_roas", "ad_spend", "ad_sales", "ad_acos_pct"],
  date_range={start: [3_MONTHS_AGO], end: [TODAY]},
  granularity="monthly",
  dimensions=["parent_asin"],
  filters={parent_asin: [HERO_PARENT_ASINS]}
)

===== OUTPUT FORMAT =====
Follow the exact schema defined in output-schemas/hero-keywords-output.md.

## Root Keyword Groups (all tiered)
| Root Group | Tier | Keywords in Group | Total Search Volume | Brand Imp Share | Brand Click Share | Brand Purchase Share | Brand CTR vs Market | Brand ATC vs Market | Brand CVR vs Market | ICAP Blocker | MoM Trend | YoY Trend |

## Hero Root Groups (top 3-4 must-win)
| Hero Root Group | Keywords | Search Volume | Brand Imp Share | Brand Purchase Share | ICAP Blocker | Why Hero |

## Branded Keywords
| Branded Keyword | Search Volume | Brand Purchase Share | Status (healthy >80% / needs defense <80%) |

## ICAP Problem Diagnosis
| Hero ASIN | Root Group | Funnel Blocker | Brand Rate | Market Rate | Gap | What to Fix |

## Keyword Trends
| Root Group | Volume Trend (MoM) | Volume Trend (YoY) | Share Trend | Seasonal? |

## Volume Alerts (if any)
| Keyword | Change | Direction | Significance |

===== SAVE =====
Save to Notion under brand's onboarding page — hero keywords section.
Tags applied in SQP.

Chat summary:
- 3-4 hero root groups and why
- Top ICAP blockers per group
- Branded keyword health
- Any significant volume alerts
- Where brand outperforms vs underperforms market
```

## Data Sources Used
- **SQP MCP**: `search_queries` (brand-level keywords), `get_trends_share_metrics` (weekly + monthly), `get_trends_percentage_metrics` (brand vs industry over time), `get_volume_alerts` (spikes/drops), `manage_query_tags` (apply tier + branded tags)
- **Metrics Engine MCP**: `query_metrics` with `asin` + `search_query` dimensions (ASIN-level SQP data), `query_metrics` with `parent_asin` (ROAS)

## What This Prompt Does NOT Do
- Does not recommend campaign structure or bid actions (that's campaign analysis later)
- Does not change any live campaigns or bids
- Tier-to-action mapping (Tier 1 = bid first, etc.) is context for later campaign analyzer, not an onboarding action

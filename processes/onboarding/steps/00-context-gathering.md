# Prompt 00 — Context Gathering (Brand + Product + Market)
## When to use: First step for every new brand onboarding. Gathers and structures all context. No decisions made here.

## Meta

| Field | Value |
|---|---|
| **Goal** | Build baseline understanding of brand, products, client goals, market. INPUT for decision steps — not a decision. |
| **Time** | Metrics Engine snapshot = last 8 weeks (weekly). SQP = top 100 keywords (latest available). |
| **Granularity** | Parent ASIN level for Metrics Engine. Brand level for SQP. |
| **Dimension** | Wide — all ASINs, all keywords. No drill-down. |
| **Sources** | Google (primary for brand/market) > Notion sales doc (client goals = truth, rest = context) > Metrics Engine MCP > SQP MCP |
| **Output** | Notion: brand onboarding page — context section |

## Constraints
- Sales doc is additional context, NOT source of truth (except client goals/notes)
- Google is PRIMARY for brand/market understanding — research there FIRST
- If Google has limited info, flag: "Not enough public data for this brand — relying on sales doc and tool data"
- Both weekly and monthly granularity
- If current period data is incomplete, flag in bold: "Data for [period] is INCOMPLETE ([X] of [Y] days)"
- Note any obvious seasonal patterns for later steps
- Branded keywords: identify them but don't analyze deeply here
- This step does NOT: select hero ASIN, tier keywords, do ICAP funnel, do market sizing, do deep multi-horizon analysis

---

## The Prompt

```
Onboard a new brand: [BRAND_NAME]

You are gathering context — not making decisions. Build the most
complete baseline understanding so Steps 2 and 3 can make precise
decisions. Flag what you don't know.

===== STEP 1a — RESEARCH BRAND ON GOOGLE (DO THIS FIRST) =====

Before touching any tool or sales doc, research the brand online:
- Search for: [BRAND_NAME] website, Amazon store, social media, reviews
- What category does this brand play in?
- Who are the top 3-5 competitors in this category?
- What price range is typical: commodity / mid-range / premium?
  Where does this brand sit?
- Is this category discovery-driven (browsing, visual, impulse)
  or intent-driven (specific search, specs, considered purchase)?
- Any seasonal patterns for this category?
- What is the brand's multi-channel presence?
  (website, Instagram, TikTok, podcast, retail, etc.)

If Google has limited info on this brand:
FLAG: "Not enough public data available for [BRAND_NAME] —
relying on sales doc and tool data for brand context."

===== STEP 1b — READ SALES DOC FROM NOTION =====

- Search Notion for [BRAND_NAME] in the Brands database
- Fetch the brand page
- Extract and record:
  - Client goals (verbatim) — THIS IS SOURCE OF TRUTH
  - Budget expectations, risk tolerance — SOURCE OF TRUTH
  - Business overview — take as CONTEXT ONLY
  - Financials (COGS if available) — CONTEXT ONLY
  - Contact details
  - Any linked Seller Central Audit — fetch for additional context
- Strategy suggestions from sales team — CONTEXT ONLY, not truth
- If anything in sales doc conflicts with Google research, flag
  the discrepancy: "Sales doc says X, but Google research shows Y"

===== STEP 1c — PULL PRODUCT DATA FROM METRICS ENGINE =====

Seller ID for [BRAND_NAME]: [find via metrics_list_sellers]
Marketplace: [US/UK as applicable]

Call: query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["br_total_sales", "br_units", "br_sessions",
           "br_cvr_pct", "br_featured_offer_pct",
           "organic_sales", "cr_organic_pct",
           "ad_spend", "ad_sales", "ad_roas",
           "cr_tacos_pct", "br_avg_price"],
  date_range={start: [8_WEEKS_AGO], end: [TODAY]},
  granularity="weekly",
  dimensions=["parent_asin"]
)

From this data, document:
- Which ASINs exist and what products they are
- Which are active (have revenue) vs dead ($0 revenue)
- What are the major revenue-contributing ASINs
- Product names, number of variants, price points
- Any critical issues visible (low buybox, declining sessions)

===== STEP 1d — PULL KEYWORD LANDSCAPE FROM SQP =====

- Call list_brands() — verify SQP brand name
  (may differ from Metrics Engine name)
- Call search_queries(brandName=[SQP_BRAND], limit=100)
  — top 100 keywords by search volume
- From this:
  - What search terms is this brand operating in?
  - What's the keyword universe?
  - Note any branded keywords (brand name in query)
  - Initial sense of market size (total search volumes)

===== STEP 1e — LISTING QUALITY (BRIEF TOUCH) =====

Use the asin-product-data skill on the top 3-5 ASINs by revenue:
/asin-product-data [TOP_ASIN_1] [TOP_ASIN_2] [TOP_ASIN_3]

From this, briefly note:
- Title quality (keywords present? readable?)
- Image count and quality
- A+ content present?
- Star rating and review count
- Any obvious red flags

NOT a full assessment — just flag what's visible.

If the skill returns no data or errors:
FLAG: "Listing quality not assessed — no data source available"

===== OUTPUT FORMAT =====
Follow the exact schema defined in output-schemas/context-output.md.

| Field | Value |
|-------|-------|
| Brand Name | |
| Category / Market | |
| Products (active ASINs with names) | |
| Client Goal (verbatim from sales doc) | |
| Key search terms (top 10-15 from SQP) | |
| Major revenue-contributing ASIN(s) | |
| Revenue overview (last 8 weeks) | |
| Price positioning (commodity/mid/premium) | |
| Discovery vs Intent | |
| Seasonality notes | |
| Listing quality notes (if available) | |
| Data gaps (what info was NOT available) | |

Save to Notion under the brand's onboarding page — context section.

Chat summary (5 points):
- What this brand is (one sentence)
- Client's goal (one sentence)
- Major revenue ASIN(s) and approximate revenue
- Key search terms the brand operates in
- Data gaps or flags raised
```

## Data Sources Used
- **Google**: brand website, social, reviews, category research (PRIMARY)
- **Notion MCP**: sales doc (brand page in Brands database), linked audit
- **Metrics Engine MCP**: `query_metrics` with parent_asin dimension — ASIN hierarchy, revenue, sessions, CVR, buybox, ads overview
- **SQP MCP**: `list_brands`, `search_queries` — top 100 keywords, search landscape

## What This Prompt Does NOT Do
- Does not select hero ASIN (Step 01)
- Does not identify root keywords or tier keywords (Step 02)
- Does not do ICAP funnel analysis (Step 02)
- Does not do market sizing (Step 01)
- Does not do deep multi-horizon time analysis (Step 01 + 02)

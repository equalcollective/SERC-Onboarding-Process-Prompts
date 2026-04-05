# Available Tools

Documentation of all MCP tools and what they can do.

---

## Metrics Engine MCP

**Endpoint:** `https://api.merchantbots.com/mcp/metrics-engine/mcp`
**Replaces:** Old MB Onboarding MCP

### What It Gives
- Business Report data: revenue, units, sessions, CVR, buybox, page views
- Advertising data: spend, sales, ROAS, ACoS, clicks, impressions, CPC, orders
- Campaign-level data: by campaign, ad group, targeting, match type
- Placement data: Top of Search, Rest of Search, Product Pages
- Search term data: actual customer search terms from ads
- SQP ASIN-level data: per-ASIN impression share, CTR, CVR, ATC rate, purchase share vs market
- Cross-report metrics: TACoS, organic %, ad sales %
- Period comparison: YoY, QoQ, MoM via compare_periods

### What It Does NOT Give
- Brand-level SQP trends (use SQP MCP for that)
- Keyword tagging (use SQP MCP)
- Volume alerts (use SQP MCP)

### Key Tools

**`metrics_list_sellers`** — List all sellers. Returns seller_id, name, marketplace.

**`metrics_list_metrics`** — List all available metrics and dimensions.

**`metrics_query_metrics`** — Main query tool.
- Parameters: metrics[], seller_id, marketplace, date_range, granularity, dimensions[], filters
- Granularity: daily, weekly, monthly, quarterly
- Dimensions: parent_asin, child_asin, campaign, ad_group, targeting, match_type, placement, search_term, search_query, asin
- Example: `query_metrics(seller_id="X", marketplace="US", metrics=["br_total_sales","ad_roas"], date_range={start:"2025-01-01",end:"2026-04-01"}, granularity="monthly", dimensions=["parent_asin"])`

**`metrics_compare_periods`** — Compare two time periods.
- Parameters: metrics[], seller_id, marketplace, period_a, period_b, granularity, dimensions, filters
- Returns: period_a results, period_b results, absolute delta, % change
- Use for: YoY comparison, QoQ comparison

**`metrics_get_metric_detail`** — Get formula and sources for a specific metric.

---

## SQP MCP

**Endpoint:** `https://sqp-merchantbots.replit.app/mcp`

### What It Gives
- Brand-level keyword data: search volume, brand vs industry funnel metrics
- Share metrics: impression share, click share, cart add share, purchase share
- Conversion rates: brand CTR, ATC rate, CVR vs industry
- Trends: weekly and monthly share/percentage/absolute trends over time
- Volume alerts: significant search volume changes, spikes, new keywords
- Keyword tagging: persistent tags (Tier 1/2/3, Branded, etc.)
- Tag-based filtering: pull data for specific tag groups

### What It Does NOT Give
- ASIN-level SQP data (use Metrics Engine for that)
- Revenue, sessions, ads data (use Metrics Engine)
- Campaign data (use Metrics Engine)

### Key Tools

**`list_brands`** — List all brands. Verify brand name before querying.

**`search_queries`** — Fetch keyword performance data.
- Parameters: brandName, limit, keyword, startDate, endDate, periodGranularity, includeTags, excludeTags, minSearchVolume
- Returns: per-keyword search volume, brand + industry funnel metrics, tags

**`get_trends_share_metrics`** — Share of voice trends over time.
- Parameters: brandName, granularity (weekly/monthly), startDate, endDate, includeTags
- Returns: impression_share_pct, click_share_pct, cart_add_share_pct, purchase_share_pct per period

**`get_trends_percentage_metrics`** — Brand vs industry conversion rates over time.
- Parameters: same as above
- Returns: brand_ctr vs industry_ctr, brand_atc_rate vs industry, brand_cvr vs industry per period

**`get_volume_alerts`** — Detect significant search volume changes.
- Parameters: brandName, granularity, minVolume, minPercentChange, changeDirection
- Returns: keywords with significant volume changes, direction, % change

**`manage_query_tags`** — Add, remove, or replace tags on keywords.
- Actions: add, remove, replace
- Parameters: action, brandName, searchQueries[], tagName

**`list_tags`** — List all available tags with metadata.

**`preview_data`** — Check data size before fetching.

---

## Notion MCP

**Endpoint:** `https://mcp.notion.com/mcp`

### What It Gives
- Read brand pages from Brands database (sales doc, client goals, audit)
- Search for pages and databases
- Create pages for output storage
- Update existing pages
- Read previous prompt outputs for chaining

### What It Does NOT Give
- Any analytics data — Notion is for documents and storage only

### Key Tools

**`notion-fetch`** — Read a page by URL or ID.

**`notion-search`** — Search workspace for pages/databases.

**`notion-create-pages`** — Create new pages with content.

**`notion-update-page`** — Update existing page content or properties.

---

## Metabase

**URL:** `https://metabase.equalcollective.com`
**Access:** API key via headers

### What It Gives
- All raw reports — 386+ available
- Listing/product data (if available)
- Additional campaign reports beyond what Metrics Engine provides

### Rules
- READ ONLY — never create, edit, or delete
- Search cards via `GET /api/card`
- Run cards via `POST /api/card/{id}/query`
- Currently not deeply integrated — used for listing quality data if available

---

## Tool Selection Guide

| I need... | Use |
|---|---|
| Revenue, sessions, CVR, buybox | Metrics Engine: `query_metrics` |
| Ad spend, ROAS, ACoS, TACoS | Metrics Engine: `query_metrics` |
| Campaign/ad group/targeting detail | Metrics Engine: `query_metrics` with campaign dimensions |
| Search term report (from ads) | Metrics Engine: `query_metrics` with search_term dimension |
| Placement performance | Metrics Engine: `query_metrics` with placement dimension |
| SQP at ASIN level | Metrics Engine: `query_metrics` with asin + search_query dimensions |
| YoY / QoQ comparison | Metrics Engine: `compare_periods` |
| Brand-level keyword landscape | SQP MCP: `search_queries` |
| Keyword trends over time | SQP MCP: `get_trends_share_metrics`, `get_trends_percentage_metrics` |
| Volume spikes/drops | SQP MCP: `get_volume_alerts` |
| Tag keywords | SQP MCP: `manage_query_tags` |
| Sales doc / client goals | Notion MCP: `notion-fetch` |
| Store outputs | Notion MCP: `notion-create-pages` |
| Listing quality data | Metabase (if available) |

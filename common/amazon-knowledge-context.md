# Amazon Knowledge Context

Shared context for all processes. Uses the same language as Metrics Engine MCP for metrics and dimensions.

---

## Metrics Reference

### Sales & Revenue (from Business Reports)
| Metric Key | Name | What It Is |
|---|---|---|
| `br_total_sales` | Total Sales | Total ordered product sales |
| `br_units` | Units Ordered | Total units ordered |
| `br_avg_price` | Avg Selling Price | Revenue / units |
| `organic_sales` | Organic Sales | Sales not attributed to ads |
| `cr_organic_pct` | Organic Sales % | Organic / total sales. Healthy: 60-80%. Below 40% = ad-dependent. |
| `cr_ad_sales_pct` | Ad Sales % | Ad-attributed / total sales |

### Traffic & Conversion (from Business Reports)
| Metric Key | Name | What It Is |
|---|---|---|
| `br_sessions` | Sessions | Unique visits in 24hr. Top of funnel. |
| `br_page_views` | Page Views | Total views (includes repeat in same session) |
| `br_cvr_pct` | Conversion Rate | Units / sessions. Amazon avg 10-15%. Premium 5-8%. |
| `br_featured_offer_pct` | Buy Box % | Share of time you own "Add to Cart." Below 95% = losing sales. Critical below 50%. |

### Advertising
| Metric Key | Name | What It Is |
|---|---|---|
| `ad_spend` | Ad Spend | Total advertising spend |
| `ad_sales` | Ad Sales | Revenue attributed to ad clicks |
| `ad_roas` | ROAS | Ad sales / ad spend. Good >3x. Excellent >5x. Critical <1x. |
| `ad_acos_pct` | ACoS | Ad spend / ad sales. Good <30%. Excellent <15%. Critical >50%. |
| `cr_tacos_pct` | TACoS | Ad spend / TOTAL sales. Most important efficiency metric. Good <15%. Excellent <8%. Critical >30%. |
| `ad_clicks` | Ad Clicks | Total ad clicks |
| `ad_impressions` | Ad Impressions | Total ad impressions |
| `ad_cpc` | CPC | Cost per click |
| `ad_cvr_pct` | Ad CVR | Ad orders / ad clicks |
| `ad_ctr_pct` | Ad CTR | Ad clicks / ad impressions |
| `ad_orders` | Ad Orders | Orders from ads |

### Campaign & Targeting (from Metrics Engine)
| Metric Key | Name | What It Is |
|---|---|---|
| `tgt_clicks` | Targeting Clicks | Clicks by targeting expression |
| `tgt_impressions` | Targeting Impressions | Impressions by targeting expression |
| `tgt_sales` | Targeting Sales | Sales by targeting expression |
| `tgt_spend` | Targeting Spend | Spend by targeting expression |
| `tgt_roas` | Targeting ROAS | ROAS by targeting expression |
| `tgt_acos_pct` | Targeting ACoS | ACoS by targeting expression |
| `tgt_cpc` | Targeting CPC | CPC by targeting expression |

### Search Term (from Metrics Engine)
| Metric Key | Name | What It Is |
|---|---|---|
| `st_clicks` | ST Clicks | Clicks from customer search terms |
| `st_impressions` | ST Impressions | Impressions from search terms |
| `st_sales` | ST Sales | Sales attributed to search terms |
| `st_spend` | ST Spend | Spend from search terms |
| `st_orders` | ST Orders | Orders from search terms |
| `st_units` | ST Units | Units from search terms |

### Placement (from Metrics Engine)
| Metric Key | Name | What It Is |
|---|---|---|
| `pl_clicks` | Placement Clicks | Clicks by placement |
| `pl_impressions` | Placement Impressions | Impressions by placement |
| `pl_sales` | Placement Sales | Sales by placement |
| `pl_spend` | Placement Spend | Spend by placement |

### SQP — Brand Level (from SQP MCP or Metrics Engine)
| Metric Key | Name | What It Is |
|---|---|---|
| `sqp_search_volume` | Search Volume | Total searches for this query |
| `sqp_brand_impression_share` | Brand Impression Share | Your brand's % of all impressions |
| `sqp_brand_ctr` | Brand CTR | Your brand clicks / your impressions |
| `sqp_brand_cvr` | Brand CVR | Your brand purchases / your clicks |
| `sqp_brand_atc_rate` | Brand ATC Rate | Your brand cart adds / your clicks |
| `sqp_brand_purchase_share` | Brand Purchase Share | Your brand's % of all purchases |
| `sqp_brand_cart_share` | Brand Cart Share | Your brand's % of all cart adds |
| `sqp_market_cvr` | Market CVR | Market-wide click-to-purchase rate |
| `sqp_market_ctr` | Market CTR | Market-wide click-through rate |
| `sqp_market_atc_rate` | Market ATC Rate | Market-wide click-to-cart rate |
| `sqp_market_purchases` | Market Purchases | Total purchases across all brands |

### SQP — ASIN Level (from Metrics Engine)
| Metric Key | Name | What It Is |
|---|---|---|
| `sqp_asin_impression_share` | ASIN Impression Share | This ASIN's % of all impressions |
| `sqp_asin_ctr` | ASIN CTR | This ASIN clicks / impressions |
| `sqp_asin_cvr` | ASIN CVR | This ASIN purchases / clicks |
| `sqp_asin_atc_rate` | ASIN ATC Rate | This ASIN cart adds / clicks |
| `sqp_asin_purchase_share` | ASIN Purchase Share | This ASIN's % of all purchases |
| `sqp_asin_cart_share` | ASIN Cart Share | This ASIN's % of all cart adds |

---

## Dimensions Reference

| Dimension Key | Name | Category | Use For |
|---|---|---|---|
| `parent_asin` | Parent ASIN | Product | Product family level analysis |
| `child_asin` | Child ASIN | Product | Individual variant analysis |
| `campaign` | Campaign | Advertising | Campaign-level performance |
| `ad_group` | Ad Group | Advertising | Ad group level |
| `targeting` | Targeting | Advertising | Targeting expression (keyword/ASIN) |
| `match_type` | Match Type | Advertising | Exact/phrase/broad |
| `placement` | Placement | Advertising | Top of Search / Rest / Product Pages |
| `search_term` | Search Term | Advertising | Customer search terms from ads |
| `search_query` | Search Query | SQP | SQP search queries |
| `asin` | ASIN (SQP) | SQP | ASIN-level SQP data |

---

## Benchmarks

| Metric | Good | Excellent | Critical |
|---|---|---|---|
| ROAS | >3x | >5x | <1x |
| TACoS | <15% | <8% | >30% |
| ACoS | <30% | <15% | >50% |
| Buy Box % | >90% | >95% | <50% |
| CVR (average) | 10-15% | >18% | <5% |
| CVR (premium) | 5-8% | >10% | <3% |
| CTR on search | >0.5% | >1% | <0.2% |
| Organic Sales % | >40% | >60% | <20% |
| Branded Purchase Share | >80% | >90% | <70% |

---

## Causality Map (Diagnostic Logic)

When a metric is down, here's what to investigate:

### Sessions Down
- Check: Buy Box % (lost buybox = lost sessions)
- Check: Ad impressions (ad visibility dropping?)
- Check: Organic rank (lost keyword rankings?)
- Check: Seasonality (same period last year?)
- Check: Competition (new entrants?)

### CVR Down
- Check: Price changes (yours or competitors)
- Check: Review rating decline
- Check: Buy Box % instability
- Check: Main image or listing changes
- Check: Traffic source quality (irrelevant ad traffic diluting CVR?)
- Check: Seasonality (browse vs buy season?)

### ROAS Down
- Check: CPC trend (competition pushing bids up?)
- Check: Ad CVR (same traffic, lower conversion?)
- Check: Targeting (wasted spend on irrelevant keywords?)
- Check: Match type distribution (too much broad?)
- Check: Search term report for irrelevant terms

### TACoS Up (getting worse)
- Check: Organic sales declining (same ad spend, less organic = worse TACoS)
- Check: Ad spend increased without proportional total sales increase
- Check: Is brand becoming more ad-dependent?

### Buy Box % Down
- Check: New 3P sellers or resellers on your listing
- Check: Price competitiveness
- Check: Fulfillment method (FBA vs FBM)
- Check: Account health issues

### Organic % Down
- Check: Ad spend increased (naturally shifts ratio)
- Check: Organic sessions/sales actually declining (real problem)
- Check: Keyword rankings lost

---

## ICAP Funnel Logic

**ICAP = Impressions → Clicks → Add to Cart → Purchases**

At each stage, compare brand rate vs market rate:

| Stage | Brand Metric | Market Metric | If brand << market | Root Cause | Fix |
|---|---|---|---|---|---|
| Impressions | Impression share | N/A | Presence problem | Not showing up in search | PPC, organic rank, keyword targeting |
| Clicks | Brand CTR | Market CTR | Creative problem | Showing but not clicked | Main image, title, price, rating, badges |
| Add to Cart | Brand ATC rate | Market ATC rate | Listing problem | Clicked but not carted | Bullets, A+ content, reviews, price perception |
| Purchases | Brand CVR | Market CVR | Conversion problem | Carted but not bought | Buy box, shipping speed, price vs competitors, trust |

**Decision logic:**
- CVR good + impression share low → increase impressions (PPC is the lever)
- CVR bad → fix listing/product before spending more on visibility
- CTR bad → fix main image and title before scaling impressions
- ATC bad → fix listing content before scaling clicks

---

## Amazon Terminology

| Term | Definition |
|---|---|
| ASIN | Amazon Standard Identification Number. Unique product ID. |
| Parent ASIN | Product family grouping (not purchasable). |
| Child ASIN | Individual variant (color, size) under a parent. Purchasable. |
| Hero ASIN | The ASIN the team focuses all energy on. |
| Hero Keywords | Top 3-4 root keyword groups — the must-win keywords. |
| Root Keyword | Base 1-3 word term from which long-tail variations derive. |
| SQP | Search Query Performance — Amazon Brand Analytics data. |
| ICAP | Impressions → Clicks → Add to Cart → Purchases funnel. |
| TACoS | Total Ad Cost of Sales — ad spend / total sales. Most important efficiency metric. |
| Buy Box | The "Add to Cart" button. If you don't own it, you lose the sale. |
| FBA | Fulfillment by Amazon. Higher fees, better Buy Box chance. |
| FBM | Fulfillment by Merchant. Lower fees, harder Buy Box. |
| Branded Keywords | Search queries containing the brand name. |
| Defense Campaign | Bidding on your own brand name to prevent competitors from stealing traffic. |

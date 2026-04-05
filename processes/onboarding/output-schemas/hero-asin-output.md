# Hero ASIN Output Schema (Step 01)

This is the structured output produced by Step 1: Hero ASIN Selection.

## Hero ASIN Summary Table

| Column | Description |
|--------|-------------|
| **ASIN** | Parent or child ASIN ID |
| **Product** | Product name / title |
| **Revenue 3mo** | Total revenue over the last 3 months |
| **Revenue 6mo** | Total revenue over the last 6 months |
| **Revenue 12mo** | Total revenue over the last 12 months |
| **% Total (3mo)** | Revenue as % of total brand revenue at 3-month horizon |
| **% Total (6mo)** | Revenue as % of total brand revenue at 6-month horizon |
| **% Total (12mo)** | Revenue as % of total brand revenue at 12-month horizon |
| **Avg Mo (3mo)** | Average monthly revenue over the last 3 months |
| **YoY Change** | Revenue change vs same period last year (%) |
| **Potential: CVR vs Market** | Brand CVR vs category CVR (e.g., "8.2% vs 10.5% = -2.3pt gap") |
| **Potential: Imp Share** | Brand impression share on primary keywords (e.g., "12% of 50K volume") |
| **Potential: Market Relevance** | Total addressable search volume for this ASIN's keywords relative to other ASINs |
| **Constraints (Ranked)** | ALL declining metrics ranked by severity: Critical / High / Medium / Low |
| **Seasonal?** | Yes/No with YoY evidence |

## Child ASIN Breakdown

For the selected hero ASIN (if it is a parent), break down by child:

| Column | Description |
|--------|-------------|
| **Child ASIN** | Child ASIN ID |
| **Variant** | Size, color, or other variant descriptor |
| **Revenue 3mo** | Total revenue over the last 3 months |
| **% of Parent** | Revenue as % of parent ASIN total |
| **Avg Mo (3mo)** | Average monthly revenue over the last 3 months |
| **YoY Change** | Revenue change vs same period last year (%) |
| **MoM Trend** | Direction of month-over-month change (up/down/flat + %) |

## Constraint Detail Table

Every declining metric for the hero ASIN, fully expanded:

| Column | Description |
|--------|-------------|
| **Metric** | Name of the declining metric |
| **Current Value** | Most recent value |
| **3mo Trend** | Direction and magnitude over 3 months |
| **6mo Trend** | Direction and magnitude over 6 months |
| **YoY Change** | Change vs same period last year |
| **MoM Change** | Most recent month-over-month change |
| **Severity** | Critical / High / Medium / Low |
| **Seasonal?** | Is this decline explained by seasonality? (Yes/No + evidence) |
| **Notes** | Any additional context |

## Potential Detail Table

Full scoring for each candidate ASIN considered:

| Column | Description |
|--------|-------------|
| **ASIN** | ASIN ID |
| **Product** | Product name |
| **CVR Gap** | Brand CVR minus market CVR (negative = opportunity) |
| **Imp Share** | Current impression share on primary keywords |
| **Market Volume** | Total monthly search volume for this ASIN's keyword set |
| **Potential Score** | Composite: larger negative CVR gap + lower imp share + higher market volume = higher potential |
| **Selected?** | Yes/No - was this the chosen hero ASIN? |
| **Reason** | If not selected, why not (e.g., "High revenue but no CVR gap - already optimized") |

## Rules

- The summary table must show the top 3-5 candidates, with the hero ASIN clearly marked
- Constraints must list ALL declining metrics, not just the most severe
- Every severity rating must be justified by the data (which horizons show decline)
- Child breakdown is required if the hero is a parent ASIN
- Potential detail table must show why non-selected ASINs were passed over

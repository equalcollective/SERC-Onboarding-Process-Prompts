# Hero ASIN Output Schema (Step 01)

This is the structured output produced by Step 1: Hero ASIN Selection.

## Single vs Multiple Hero Layout

- **If 1 hero ASIN:** Data lives in database properties + "Why Hero" section in page body. No separate summary table needed.
- **If 2+ hero ASINs:** Use one table with a column per hero. Do not create separate tables per hero.

## Problem Statement (Revenue Symptom)

Before constraint details, state the revenue problem as the headline. Revenue decline is the symptom, not a cause — it goes here, separate from the causal constraint analysis below.

| Column | Description |
|--------|-------------|
| **ASIN** | Parent or child ASIN ID |
| **Product** | Product name / title |
| **Revenue 3mo** | Total revenue over the last 3 months |
| **Avg Mo (3mo)** | Average monthly revenue over the last 3 months |
| **YoY Change** | Revenue change vs same period last year (%) |
| **MoM Trend** | Direction of recent month-over-month changes |

This is "what's happening" — the sections below explain "why."

## Monthly Revenue Detail

Last 3 months only. The aggregated 3/6/9/12mo numbers and YoY comparison cover the longer view — do not dump 12 rows.

| Column | Description |
|--------|-------------|
| **Month** | Month name + year |
| **Revenue** | Total revenue for that month |
| **MoM Change** | Month-over-month % change |

## Hero ASIN Summary Table

Top 3-5 candidates with the hero clearly marked:

| Column | Description |
|--------|-------------|
| **ASIN** | Parent or child ASIN ID |
| **Product** | Product name / title |
| **Revenue 3mo** | Total revenue over the last 3 months |
| **Revenue 6mo** | Total revenue over the last 6 months |
| **% Total (3mo)** | Revenue as % of total brand revenue at 3-month horizon |
| **% Total (6mo)** | Revenue as % of total brand revenue at 6-month horizon |
| **Avg Mo (3mo)** | Average monthly revenue over the last 3 months |
| **YoY Change** | Revenue change vs same period last year (%) |
| **Potential: CVR vs Market** | Brand SQP CVR vs industry SQP CVR (e.g., "8.2% vs 10.5% = -2.3pt gap") |
| **Potential: Imp Share** | Brand impression share on primary keywords (e.g., "12% of 50K volume") |
| **Potential: Market Relevance** | Total addressable search volume for this ASIN's keywords relative to other ASINs |
| **Constraints (Ranked)** | Causal metrics only (sessions, CVR, buybox, ROAS, imp share, etc.) — NOT revenue. Ranked by severity: Critical / High / Medium / Low |
| **Seasonal?** | Yes/No with YoY evidence |
| **Confidence** | Judgment call: relevancy first, then search volume, then meaningful clicks (20-30+ generally, depends on brand scale, never 2 clicks), then purchases/CVR. Not rigid thresholds — depends on brand size. |

### Hero Level Rule
- Default: hero = parent ASIN. Child breakdown is supporting context.
- If one child has 60%+ of parent revenue AND significantly better CVR/metrics than siblings: call it out as "Hero Variant" in the summary.
- Always state parent as hero ASIN. Dominant child is noted alongside, not instead.

## Child ASIN Breakdown

For the selected hero ASIN (if it is a parent), break down by child:

| Column | Description |
|--------|-------------|
| **Child ASIN** | Child ASIN ID |
| **Variant** | Size, color, or other variant descriptor |
| **Revenue (Recent Month)** | Revenue for the most recent complete month |
| **Avg Mo (3mo)** | Average monthly revenue over the last 3 months |
| **Avg Mo (6mo)** | Average monthly revenue over the last 6 months |
| **% of Parent** | Revenue as % of parent ASIN total (based on 3mo) |
| **YoY Change** | Revenue change vs same period last year (%) |
| **MoM Trend** | Direction of month-over-month change (up/down/flat + %) |

## Constraint Detail Table (Causal Metrics Only)

Every declining causal metric for the hero ASIN. Revenue decline is NOT listed here — it is the problem statement above. This table answers "why is revenue declining?"

| Column | Description |
|--------|-------------|
| **Metric** | Name of the declining causal metric (sessions, BR CVR, SQP CVR, buybox %, ROAS, imp share, etc.) |
| **Status** | Healthy / Declining / Critical |
| **Current Value** | Most recent value |
| **3mo Trend** | Direction and magnitude over 3 months |
| **6mo Trend** | Direction and magnitude over 6 months |
| **YoY Change** | Change vs same period last year |
| **MoM Change** | Most recent month-over-month change |
| **Severity** | Critical / High / Medium / Low |
| **Seasonal?** | Is this decline explained by seasonality? (Yes/No + evidence) |
| **Confidence** | Judgment call based on relevancy + volume + clicks + purchases. Depends on brand scale. |
| **Notes** | Any additional context |

Include healthy metrics briefly (Status: Healthy) but only detail declining ones.

## Potential Detail Table

Full scoring for each candidate ASIN considered:

| Column | Description |
|--------|-------------|
| **ASIN** | ASIN ID |
| **Product** | Product name |
| **CVR Gap** | Brand SQP CVR minus market SQP CVR (negative = opportunity) |
| **Imp Share** | Current impression share on primary keywords |
| **Market Volume** | Total monthly search volume for this ASIN's keyword set |
| **Potential Score** | Composite: larger negative CVR gap + lower imp share + higher market volume = higher potential |
| **Confidence** | High / Medium / Low — based on sample size for rate metrics |
| **Selected?** | Yes/No - was this the chosen hero ASIN? |
| **Reason** | If not selected, why not (e.g., "High revenue but no CVR gap - already optimized") |

## Rules

- The summary table must show the top 3-5 candidates, with the hero ASIN clearly marked
- If 1 hero: data goes in database properties + "Why Hero" page section. If 2+: one table with columns per hero.
- Revenue decline is the problem statement at the top — constraints focus on causal metrics only
- Constraints must list ALL declining causal metrics, not just the most severe
- Every severity rating must be justified by the data (which horizons show decline)
- Child breakdown is required if the hero is a parent ASIN — show recent month + 3mo avg + 6mo avg
- Monthly revenue detail: last 3 months only, not 12
- Potential detail table must show why non-selected ASINs were passed over
- Do not include a "Hero ASIN Verdict" prose paragraph — the table and Why Hero section speak for themselves
- **Every number must include its date range** — e.g., "$5,769 (Q1 2026: Jan-Mar)" not just "$5,769"
- **Every metric must name its source** — e.g., "BR CVR (Metrics Engine)" or "SQP CVR (SQP MCP)"
- **Rate metrics must include sample size** — if CVR/CTR/ATC is based on <50 events, flag "low confidence"
- **Add Confidence column** to any table with rate metrics: based on purchases (High 20+ / Medium 5-20 / Low <5). Show click count for context.
- **Campaign/structural claims must be verified live** — don't state "zero campaigns exist" based solely on a sales audit. Query Metrics Engine campaign/targeting data to confirm. If unverified, state: "Per Seller Central Audit (date) — not independently verified"

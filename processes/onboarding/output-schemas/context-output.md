# Context Output Schema (Step 00)

This is the Brand Context Summary produced by Step 0: Context Gathering. It is written to Notion as a separate reference page.

Context = brief brand story + client goal + product catalogue + basic revenue sense. Deep product analysis, keyword rates, listing quality, and pricing belong in later steps.

## Schema

| Field | Value |
|-------|-------|
| **Brand Name** | Full brand name as it appears on Amazon |
| **Category / Market** | Primary Amazon category and subcategory (e.g., "Sports & Outdoors > Exercise & Fitness > Yoga > Yoga Mats") |
| **Products (Active ASINs)** | Count of active ASINs + brief product range description (e.g., "23 active ASINs across yoga mats, blocks, straps, and bags") |
| **Client Goal** | Verbatim from sales doc or client brief. If no doc provided, write "No client goal document provided" |
| **Key Search Terms** | Top 10-15 search terms by volume from SQP. For each: keyword name + monthly volume + period only. No rate metrics here — detailed SQP with CTR/ATC/CVR rates belongs in Step 02. |
| **Major Revenue ASINs** | Top 5 ASINs by revenue with ASIN ID, product name, and approximate revenue share (e.g., "B0XX - Ultra Mat 6mm (38%), B0YY - Travel Mat (18%)...") |
| **Revenue Overview** | Last 3 months MoM (monthly revenue + MoM % change) OR last 4-5 weeks WoW (weekly revenue + WoW % change). Pick whichever gives the clearest recent picture. Include period total and average. Do NOT include 12-month or 8-week full tables — aggregated YoY comparisons in Step 01 cover the longer view. |
| **Seasonality Notes** | Any seasonal patterns visible in the revenue data or known from category (e.g., "Fitness category peaks Jan-Mar, visible uptick in recent weeks") |
| **Data Gaps** | List any data that could not be retrieved, any horizons with missing data, any tools that returned errors |

## Removed from Context (belongs in later steps)

These fields were previously in the context schema but belong in Step 01 (Hero ASIN) or Step 02 (Hero Keywords):
- **Detailed keyword rates** (CTR, ATC, CVR vs market) — belongs in Step 02 ICAP analysis
- **Listing quality analysis** (title length, images, A+ content, ratings) — belongs in Step 01 hero ASIN analysis
- **Price positioning** — belongs in Step 01 hero ASIN analysis
- **Discovery vs Intent keyword ratio** — belongs in Step 02 keyword tiering
- **Flags for Next Steps** — context gathers data, it does not make decisions or identify blockers. Flags/conclusions belong in Step 01/02.

## Rules

- This document is a reference, not an analysis. State facts, do not interpret.
- Context gathers — it never concludes. No "primary blocker" statements, no "biggest opportunity" claims.
- Client Goal must be verbatim or explicitly marked as unavailable.
- All numbers should include the date range they cover.
- Key Search Terms: keyword names + volume only. No rate metrics in this step.
- Revenue: 3 months MoM or 4-5 weeks WoW is sufficient. Not 8 weeks + 12 months.
- Data Gaps must be populated even if empty ("No data gaps identified").

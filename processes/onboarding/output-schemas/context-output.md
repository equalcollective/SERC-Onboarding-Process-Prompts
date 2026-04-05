# Context Output Schema (Step 00)

This is the Brand Context Summary produced by Step 0: Context Gathering. It is written to Notion as a separate reference page.

## Schema

| Field | Value |
|-------|-------|
| **Brand Name** | Full brand name as it appears on Amazon |
| **Category / Market** | Primary Amazon category and subcategory (e.g., "Sports & Outdoors > Exercise & Fitness > Yoga > Yoga Mats") |
| **Products (Active ASINs)** | Count of active ASINs + brief product range description (e.g., "23 active ASINs across yoga mats, blocks, straps, and bags") |
| **Client Goal** | Verbatim from sales doc or client brief. If no doc provided, write "No client goal document provided" |
| **Key Search Terms** | Top 10-15 search terms by volume from SQP, with approximate volume (e.g., "yoga mat (45K), exercise mat (28K), thick yoga mat (15K)...") |
| **Major Revenue ASINs** | Top 5 ASINs by revenue with ASIN ID, product name, and approximate revenue share (e.g., "B0XX - Ultra Mat 6mm (38%), B0YY - Travel Mat (18%)...") |
| **Revenue Overview (8 Weeks)** | Total brand revenue for the last 8 weeks with weekly trend direction. Note if trending up, down, or flat. Include total and weekly average. |
| **Price Positioning** | Price range across the catalog + where the hero candidates sit relative to competitors (e.g., "$19.99-$49.99 range, main products at $29.99 vs category median $24.99") |
| **Discovery vs Intent** | Ratio of broad/discovery keywords to specific/intent keywords in the top 100 (e.g., "60% discovery terms like 'yoga mat', 40% intent terms like 'thick yoga mat for bad knees'") |
| **Seasonality Notes** | Any seasonal patterns visible in the 8-week snapshot or known from category (e.g., "Fitness category peaks Jan-Mar, visible uptick in weeks 1-4") |
| **Listing Quality** | For top 3-5 ASINs: title length, image count, A+ content (yes/no), rating, review count. Flag any obvious gaps. If Metabase data unavailable, write "Listing data not pulled" |
| **Data Gaps** | List any data that could not be retrieved, any horizons with missing data, any tools that returned errors |

## Rules

- This document is a reference, not an analysis. State facts, do not interpret.
- Client Goal must be verbatim or explicitly marked as unavailable.
- All numbers should include the date range they cover.
- Data Gaps must be populated even if empty ("No data gaps identified").

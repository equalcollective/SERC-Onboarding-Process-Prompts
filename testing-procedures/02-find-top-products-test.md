# Test Procedure 2: Find Top Products

## The Prompt

```
Run Find Top Products for [BRAND_NAME].

===== HOW TO RUN =====

1. Read the output of Procedure 1 (Context Gathering) from the
   task page "Onboarding [BRAND_NAME]" in the Tasks database:
   https://www.notion.so/fdada86ca84a4d04881afefd828eb17c
2. Run all steps below
3. Write output inside the SAME task page — below Procedure 1 output

===== STEP 1: PULL REVENUE DATA =====

Find seller in Metrics Engine: metrics_list_sellers
Seller ID: [find from list]
Marketplace: [US/UK]

Pull last 90 days revenue per parent ASIN:
query_metrics(
  seller_id=[SELLER_ID],
  marketplace=[MARKETPLACE],
  metrics=["br_total_sales"],
  date_range={start: [90_DAYS_AGO], end: [TODAY]},
  granularity="monthly",
  dimensions=["parent_asin"]
)

===== STEP 2: RESOLVE PRODUCT NAMES =====

CRITICAL: Metrics Engine returns ASIN IDs without product names.
You MUST look up every ASIN to get the actual product name.
NEVER guess product names from memory or web search.

Use the asin-product-data skill or query Metabase directly:
SELECT asin, title FROM orange_schema.new_asins
WHERE asin IN ([ALL_PARENT_ASINS]) AND marketplace = 'US'

If an ASIN is not found in Metabase, search it on Amazon.
NEVER write "Unknown product" in the output.
NEVER guess or assume which product an ASIN is.
Every ASIN must have a verified product name.

===== STEP 3: RANK AND SELECT =====

1. Sort all parent ASINs by revenue descending
2. Calculate revenue % for each (ASIN revenue / total brand revenue)
3. Calculate cumulative revenue % (running total)
4. Select all ASINs needed to cross 70% cumulative revenue — these are "Hero" ASINs
   - If 1 ASIN crosses 70% alone → 1 hero
   - If 2 ASINs needed to cross 70% → 2 heroes
   - And so on
5. All remaining ASINs = "Parked" with reasoning

===== STEP 4: CROSS-CHECK WITH CLIENT CONTEXT =====

Read the Procedure 1 output (Client Goals + Important Client Notes).
Check:
- Did client mention specific products to focus on or avoid?
- Does the data match client preference?
- If mismatch: flag it clearly
  Example: "Client wants to focus on Product X but data shows
  Product Y has 60% of revenue — needs discussion"
- If no mismatch: note "No mismatch — data aligns with client context"

===== STEP 5: UPDATE PRODUCTS IN NOTION =====

For each Hero ASIN only (ignore Parked):

1. Search the Products database for the ASIN
   (collection://a28ee0e2-e0c2-444a-b3a0-5b0946d71fe8)
2. If found → use existing product page
3. If NOT found → create new product in Products database:
   - Product Name: "[Brand Name] - [Short Product Name]"
   - ASIN: [ASIN]
   - Parent ASIN: [parent ASIN]
   - Brand: link to brand page
   - Focus Priority: "Active"
4. Update product page with:
   - Product Understanding: brief description of what the
     product is, who buys it, key features
   - COGS ($): if COGS was mentioned in Procedure 1 sales notes
     for this product, fill it in. If not available, leave empty.
   - Revenue Details (in page content):

     | Metric | Value |
     |---|---|
     | Revenue (90d) | $X |
     | Revenue % of brand | X% |
     | Date range | [start] to [end] |
     | Source | Metrics Engine |
     | Status | Hero ASIN (Xth of Y) |

5. Link all hero products to the task's Product(s) property

===== OUTPUT =====

Write inside the SAME task page "Onboarding [BRAND_NAME]"
below Procedure 1 output.

Page heading:
# Procedure 2: Find Top Products

Then the table:

| Parent ASIN | Product Name | Revenue (90d) | Revenue % | Cumulative % | Status | Reasoning |
|-------------|-------------|---------------|-----------|--------------|--------|-----------|
| B0XXXXX | [VERIFIED product name] | $X | 45% | 45% | Hero | Highest revenue, covers 45% of sales |
| B0YYYYY | [VERIFIED product name] | $X | 28% | 73% | Hero | Needed to cross 70% threshold |
| B0ZZZZZ | [VERIFIED product name] | $X | 15% | 88% | Parked | Not needed for 70% threshold |

Then below the table:
**Client Context Cross-Check:** [any mismatches or "No mismatch"]

STOP. Present output. Wait for review.
```

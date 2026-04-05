# Hero Keywords Output Schema (Step 02)

This is the structured output produced by Step 2: Hero Keywords + ICAP Diagnosis.

## Root Keyword Groups Table (All Tiered)

Every root keyword group, tiered and scored:

| Column | Description |
|--------|-------------|
| **Root Group** | Root keyword term (e.g., "yoga mat") |
| **Tier** | 1 / 2 / 3 |
| **Keywords** | Count of individual keywords in this group |
| **Volume** | Total monthly search volume for the group |
| **Imp Share** | Brand impression share (%) |
| **Click Share** | Brand click share (%) |
| **Purchase Share** | Brand purchase share (%) |
| **CTR vs Market** | Brand CTR minus market CTR (e.g., "+0.5pt" or "-1.2pt") |
| **ATC vs Market** | Brand ATC rate minus market ATC rate |
| **CVR vs Market** | Brand CVR minus market CVR |
| **ICAP Blocker** | Primary funnel stage blocker (Impressions / Clicks / Cart / Purchase / None) |
| **MoM** | Month-over-month volume change (%) |
| **YoY** | Year-over-year volume change (%) |

## Hero Root Groups (3-4 Selected)

The priority keyword groups for the hero ASIN:

| Column | Description |
|--------|-------------|
| **Group** | Root keyword term |
| **Keywords** | List of constituent keywords in this group |
| **Volume** | Total monthly search volume |
| **Imp Share** | Brand impression share (%) |
| **Purchase Share** | Brand purchase share (%) |
| **ICAP Blocker** | Primary funnel stage blocker |
| **Why Hero** | Brief explanation of why this group was selected (e.g., "Highest volume Tier 1 group with 3.1pt CVR gap and only 8% imp share - largest growth opportunity") |

## Branded Keywords Table

| Column | Description |
|--------|-------------|
| **Keyword** | The branded search term |
| **Volume** | Monthly search volume |
| **Purchase Share** | Brand purchase share (%) |
| **Status** | Healthy (>=70%) / Warning (50-69%) / Critical (<50%) |

## ICAP Diagnosis Table

Connects hero ASIN constraints to keyword-level funnel causes:

| Column | Description |
|--------|-------------|
| **ASIN** | Hero ASIN ID |
| **Group** | Root keyword group where the blocker manifests |
| **Blocker** | Funnel stage (Impressions / Clicks / Cart / Purchase) |
| **Brand Rate** | Brand's rate at the blocked funnel stage (%) |
| **Market Rate** | Market average rate at the blocked funnel stage (%) |
| **Gap** | Difference (brand minus market, in percentage points) |
| **Fix** | Recommended action category (e.g., "Listing optimization - main image and title", "PPC bid increase for visibility", "A+ content and review strategy", "Price/shipping competitiveness") |

## Trends Table

Directional trends for each hero root group:

| Column | Description |
|--------|-------------|
| **Group** | Root keyword group |
| **Vol MoM** | Volume month-over-month change (%) |
| **Vol YoY** | Volume year-over-year change (%) |
| **Share Trend** | Brand impression/purchase share direction over 3 months (Growing / Declining / Flat) |
| **Seasonal?** | Yes/No - is the volume change explained by seasonality? Include evidence. |

## Rules

- All root groups must be tiered - no untiered keywords
- Hero root groups must number 3-4, never fewer or more
- Branded keywords must be completely separate from the tiered analysis
- ICAP diagnosis must trace back to at least one constraint from Step 1
- Every "Fix" recommendation must be a category, not a specific tactical instruction
- Trends table must cover all hero root groups, not just the top one
- If a group has no ICAP blocker (brand outperforms market at all stages), mark as "None" and note in Why Hero

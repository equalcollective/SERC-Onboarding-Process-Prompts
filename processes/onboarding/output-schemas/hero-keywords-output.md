# Hero Keywords Output Schema (Step 02)

This is the structured output produced by Step 2: Hero Keywords + ICAP Diagnosis.

## Hero Root Groups Table (Primary Output)

The 3-4 priority keyword groups for the hero ASIN. This is the primary keyword table.

| Column | Description |
|--------|-------------|
| **Group** | Root keyword term |
| **Keywords** | List of constituent keywords in this group |
| **Volume** | Total monthly search volume |
| **Imp Share** | Brand impression share (%) |
| **Purchase Share** | Brand purchase share (%) |
| **ICAP Blocker** | Primary funnel stage blocker (Impressions / Clicks / Cart / Purchase / None) |
| **Confidence** | High (500+ events) / Medium (50-500) / Low (<50 events) — based on sample size for rate metrics |
| **Why Hero** | Brief explanation of why this group was selected (e.g., "Highest volume Tier 1 group with 3.1pt CVR gap and only 8% imp share") |

## Full Tiered Root Groups (Reference / Collapsed)

Every root keyword group, tiered and scored. This is reference material — present as a collapsed or secondary section, not the primary table.

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
| **Confidence** | High (500+ events) / Medium (50-500) / Low (<50 events) |
| **MoM** | Month-over-month volume change (%) |
| **YoY** | Year-over-year volume change (%) |

## Branded Keywords Table

| Column | Description |
|--------|-------------|
| **Keyword** | The branded search term |
| **Volume** | Monthly search volume |
| **Purchase Share** | Brand purchase share (%) |
| **Status** | Healthy (>=70%) / Warning (50-69%) / Critical (<50%) |

## ICAP Diagnosis Table

Connects hero ASIN constraints to keyword-level funnel causes. No "Fix" column — onboarding identifies WHAT is broken, not HOW to fix it. Fix recommendations belong in strategy/campaign phase.

| Column | Description |
|--------|-------------|
| **ASIN** | Hero ASIN ID |
| **Group** | Root keyword group where the blocker manifests |
| **Blocker** | Funnel stage (Impressions / Clicks / Cart / Purchase) |
| **Brand Rate** | Brand's rate at the blocked funnel stage (%) |
| **Market Rate** | Market average rate at the blocked funnel stage (%) |
| **Gap** | Difference (brand minus market, in percentage points) |
| **Confidence** | High (500+ events) / Medium (50-500) / Low (<50 events) |

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

- All root groups must be tiered — no untiered keywords
- Hero root groups must number 3-4, never fewer or more
- **Hero Root Groups table is the primary output.** Full tiered list is reference/collapsed — not the headline.
- Branded keywords must be completely separate from the tiered analysis
- ICAP diagnosis must trace back to at least one constraint from Step 1
- ICAP table: no "Fix" column. Keep ASIN, group, blocker, brand rate, market rate, gap, confidence only.
- **One value per cell** — no multi-line values in table cells. Keep cell content short. If explanation needed, use a separate "Notes" column.
- Do not include a summary paragraph at the end of Step 02 output. The ICAP table + hero root groups table are self-explanatory. The chat summary (5 lines) serves as the verbal recap.
- Trends table must cover all hero root groups, not just the top one
- If a group has no ICAP blocker (brand outperforms market at all stages), mark as "None" and note in Why Hero
- **Every number must include its date range** — e.g., "12,669/mo (Jan 2026)" not just "12,669/mo"
- **Every metric must name its source** — e.g., "SQP impression share (SQP MCP)" or "ad spend (Metrics Engine)"
- **Rate metrics must include sample size** — e.g., "ATC 10.1% (713 clicks)". If <50 events, flag "low confidence"
- **Add Confidence column** to all tables with rate metrics: High (500+ events) / Medium (50-500) / Low (<50 events)
- **If a rate is sporadic** (0% most periods, spike in one), present it as such — never cherry-pick a single good period as the representative value

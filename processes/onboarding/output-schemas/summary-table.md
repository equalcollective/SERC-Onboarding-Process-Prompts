# Summary Table Schema

Summary fields are stored as database properties. The page body contains the "Why Hero" explanation only.

## Database Properties (where key fields live)

| Property | Description |
|----------|-------------|
| **Brand Name** | Full brand name |
| **Client Goal** | Verbatim from sales doc in bullet points. Each goal/expectation = one bullet. Never a paragraph. |
| **Hero ASIN** | ASIN ID + product name (e.g., B0XXXXXXXX - Ultra Yoga Mat 6mm) |
| **Constraints (Ranked)** | Bullet list, one constraint per bullet, ordered by severity. Format per bullet: • [CRITICAL/HIGH/MEDIUM/LOW]: metric + direction + horizon. Never a paragraph. |
| **Hero Root Groups** | Bullet list, one group per bullet: • group name (volume, imp share). Never a paragraph. |
| **Notes** | Only extra observations that don't fit in any other property or the page body. Keep minimal. If nothing extra, leave empty. |

## Page Body: Brand Context + Why Hero

The only prose section on the page. Two parts:

**Part 1: Brand & Product Context (2-3 lines)**
- What does this brand do? What category/market?
- What is the hero product and what's its USP?
- How's the market — competitive, niche, growing, saturated?

**Part 2: Why Hero (2-3 lines)**
- Revenue share + potential summary + primary constraint

**Part 3: Key Context (bullet points)**
- • Seasonal: yes/no + one-line evidence (e.g., "Dec peak, Q1 dip matches YoY")
- • Market position: brand share on hero keywords (e.g., "0.3% imp share on $33K/mo Tier 1 market")
- • Branded KW health: purchase share status (e.g., "86% — healthy, no defense needed")

**Format:** Parts 1-2 = narrative (4-6 sentences). Part 3 = bullet points. This replaces what was in Notes — keeping Notes minimal for true extras only.

**Example:**
"Goal Crazy is a founder-led brand selling a 90-day undated guided planner ($34.95) in a mid-range goal planner market dominated by Clever Fox and BestSelf. USP is the integrated Goal Discovery Course — it's a coaching system, not just stationery. The Tier 1 market ('goal planner', 'goal journal') does ~$33K/mo in purchases.

The Undated Planner (B085TN9RTW) is the hero — 100% of brand revenue. HIGH potential: product converts above market when found (SQP ATC 10.1% on 713 clicks) but impression share is 0.2-0.5% on Tier 1 keywords. Massive visibility gap with zero non-branded PPC campaigns."

## Rules

- Every database property must be populated. If data is unavailable, write "DATA GAP: [reason]"
- Constraints property must include ALL declining metrics, not just the top one
- Client Goal must be verbatim — do not interpret or rephrase
- Hero Root Groups must show 3-4 groups, never fewer
- Seasonal, Market Position, and Branded KW Health go in the Notes property — not the page body
- **Every number must include its date range** — e.g., "0.23% imp share (Jan 2026)" not just "0.23% imp share"
- **Every metric must name its source** — specify SQP CVR vs BR CVR vs Ad CVR; specify Metrics Engine vs SQP MCP vs Seller Central Audit
- **Rate metrics (CVR, CTR, ATC) must include sample size** — e.g., "10.1% ATC (713 clicks)". If sample <50, flag as "low confidence"
- **If a rate metric is sporadic** (e.g., 0% most months with occasional spikes), say so — don't present a single good month as representative

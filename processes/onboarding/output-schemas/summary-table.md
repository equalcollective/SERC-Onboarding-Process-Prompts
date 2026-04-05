# Summary Table Schema

Summary fields are stored as database properties. The page body contains the "Why Hero" explanation only.

## Database Properties (where key fields live)

| Property | Description |
|----------|-------------|
| **Brand Name** | Full brand name |
| **Client Goal** | Verbatim goal from client brief or sales doc. Do not paraphrase. |
| **Hero ASIN** | ASIN ID + product name (e.g., B0XXXXXXXX - Ultra Yoga Mat 6mm) |
| **Constraints (Ranked)** | All declining metrics ranked by severity. Format: [Critical/High/Medium/Low]: metric + direction + horizon |
| **Hero Root Groups** | 3-4 root groups with volume + impression share each |
| **Notes** | Seasonal evidence, market position, branded KW health, and any other context that doesn't fit above |

## Page Body: "Why Hero" Section

The only section written inside the page body. Explains why this ASIN was chosen as the hero.

**Must include:**
- Revenue share of total brand (e.g., "42% of brand revenue at 3mo")
- Potential summary (e.g., "SQP CVR 15% below market on Tier 1+2 keywords, impression share 8% on 50K monthly volume keywords")
- Brief rationale tying revenue weight + upside potential together

**Format:** 2-4 sentences. Not a table — this needs narrative explanation.

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

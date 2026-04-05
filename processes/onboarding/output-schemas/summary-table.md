# Summary Table Schema (1 Screen)

This table is the primary deliverable. It must fit on a single screen and be self-contained.

## Schema

| Field | Value |
|-------|-------|
| **Brand Name** | Full brand name |
| **Client Goal** | Verbatim goal from client brief or sales doc. Do not paraphrase. |
| **Hero ASIN** | ASIN ID + product name (e.g., B0XXXXXXXX - Ultra Yoga Mat 6mm) |
| **Why Hero** | Revenue % of total (e.g., "42% of brand revenue at 3mo") + potential summary (e.g., "CVR 15% below market, impression share 8% on 50K monthly volume keywords") |
| **Constraints (Ranked)** | All declining metrics ranked by severity. Format: [Critical/High/Medium/Low]: metric + direction + horizon (e.g., "CRITICAL: Revenue -18% YoY and -5% MoM; HIGH: CVR -12% vs market YoY; MEDIUM: ATC rate -8% MoM") |
| **ICAP Diagnosis** | Funnel stage blocker + keyword group + gap size (e.g., "Click-to-Cart blocker on 'yoga mat' group: ATC rate 4.2% vs market 6.8% = 2.6pt gap") |
| **Hero Root Groups** | 3-4 root groups with volume + impression share each (e.g., "1. yoga mat (45K vol, 12% imp share) 2. exercise mat (28K vol, 6% imp share) 3. workout mat (18K vol, 9% imp share)") |
| **Branded KW Health** | Purchase share for top branded keywords (e.g., "BrandName yoga mat: 78% purchase share - Healthy") |
| **Seasonal?** | Yes/No + evidence (e.g., "Yes - Q1 dip matches YoY pattern, volume recovers by March historically") |
| **Market Position** | Total search volume for hero keywords + brand purchases + brand share (e.g., "150K total monthly searches, 2.1K brand purchases, 3.2% purchase share") |

## Rules

- Every field must be populated. If data is unavailable, write "DATA GAP: [reason]"
- Constraints field must include ALL declining metrics, not just the top one
- Client Goal must be verbatim - do not interpret or rephrase
- Hero Root Groups must show 3-4 groups, never fewer
- Branded KW Health must include the purchase share percentage and status label

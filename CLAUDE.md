# SERC Onboarding — Claude Code Instructions

## What This Repo Is

This is the Amazon brand onboarding system. It contains the process definition, executable prompts, shared context, and output schemas for onboarding new brands.

## How to Use

### Before Running Any Prompt
1. Read `common/amazon-knowledge-context.md` — understand all metrics, dimensions, benchmarks, causality map, ICAP logic
2. Read `common/available-tools.md` — know what each MCP tool does, its key calls, and when to use which tool
3. Read `processes/onboarding/onboarding-process.md` — understand the full process flow, constraints, and data flow

### Running Prompts
- Prompts are in `processes/onboarding/steps/`
- Run in order: 00 → 01 → 02 → 03
- **STOP after each step. Do NOT proceed to the next step automatically.**
- After completing a step: write output to Notion, give chat summary, then WAIT.
- The human will review the output, give feedback, and explicitly ask you to run the next step.
- Never combine steps. Never run Step 01 immediately after Step 00. Always wait for approval.
- Each prompt reads the previous step's output from Notion
- Output must follow the exact schemas in `processes/onboarding/output-schemas/`

### Output Rules
- **Output schemas are STRICT. Do not deviate, skip fields, or change table structure.** Every field in the schema must be populated. If data is unavailable, write "DATA GAP: [reason]" — never leave blank or omit.
- **Extra observations** that don't fit the schema go to the **Notes** property on the brand's database row. Never lose insights — if you find something important outside the schema, write it to Notes.
- **Summary table** (from output-schemas/summary-table.md) is the primary deliverable — fits on 1 screen
- **Detailed analysis** follows the hero-asin-output.md and hero-keywords-output.md schemas exactly
- **Context reference** follows context-output.md exactly — stored as a separate Notion page (internal only)
- **ALL output goes to the Onboarding Outputs database:** https://www.notion.so/5cb5e662750a4ad8af170d1e67592319
  - **Always create a NEW row** for every onboarding run — even if the brand already exists in the database. If "Goal Crazy" was onboarded before and you're asked to onboard it again, create a fresh row. Old rows stay as history.
  - When a new brand is requested, create a new row with: Brand Name, Status = "Not Started". Store Seller ID, SQP Brand Name, and Marketplace in the page body (operational data, not properties).
  - Each brand's page (inside the row) contains the step outputs as sections
  - After each step, update the Status property on the brand's row
  - After each step, update the Notes property with any extra observations
- **Before writing to Notion, verify your output matches the schema table-by-table, field-by-field.** If a schema has 10 fields, your output must have exactly 10 fields.

## Global Rules (Apply Always)

### Time Horizons
Every analysis uses ALL of these: 3mo, 6mo, 9mo, 12mo, YoY (same month + same quarter), MoM, QoQ, WoW (last 4-8 weeks). Never use a single window.

### Granularity
Both weekly AND monthly. Always.

### Incomplete Data
Flag in bold: "Data for [period] is INCOMPLETE ([X] of [Y] days)." Never silently omit.

### Seasonality
Always compare to same period last year before concluding decline or growth.

### Branded Keywords
Separate from all analysis. Check purchase share (should be 80-90%). Never hero keywords. Flag if defense needed.

### Data Claims
Every recommendation cites a specific data point. No opinions without numbers.

### Independent Research
- Do NOT rely on pre-defined data, existing SQP tags, or sales doc claims as starting points
- Build all analysis from scratch using live tool queries
- If existing tags exist in SQP from previous runs, verify independently. Override if your analysis disagrees.
- NEVER start from a sales doc/audit claim and try to "confirm" it. Always pull live data FIRST and state what YOU find. The audit is irrelevant — live data is the truth.

### Cross-Step Referencing
- When a later step needs to reference a finding from an earlier step, write "per Step 01 constraint #X" instead of re-stating the full fact
- Do NOT repeat the same data point across multiple steps — reference it
- Each step should contain only NEW analysis, not rehashed content from previous steps

### SQP Tagging — Relevancy First
- ONLY tag keywords that are genuinely relevant to the hero ASIN's product. Not top 100. Not top by volume. Only relevant.
- Every keyword must pass: "would a customer searching this actually want THIS product?"
- If a keyword is about a different product type, category, or use case — do NOT tag it even if the brand has impressions on it.
- Tags are persistent in SQP and used across sessions. Wrong tags pollute future analysis for every team member.
- After tagging, confirm how many keywords per tier and that all are product-relevant.

### SQP Rate Metrics — Meaningful Sample Only
- NEVER calculate CVR/CTR/ATC on keywords where brand has <5 clicks. The number is meaningless.
- For CVR/CTR/ATC analysis, use keywords sorted by brand clicks or purchases — not by search volume.
- Top 100 by volume will show 90 keywords with zero brand data for small brands. Useless for rate analysis.
- Analyze the 20-50 keywords where the brand actually has clicks. That's where conversion data is real.
- For volume/impression analysis (market sizing, impression share), top by volume is fine.
- For conversion analysis (CVR, CTR, ATC vs market), top by brand clicks is the right lens.

### Bullet Points, Not Paragraphs
- Database properties that contain multiple items (Client Goal, Constraints, Hero Root Groups, Notes) must use bullet points — one item per bullet.
- Never write these as paragraphs. Paragraphs hide information. Bullets make every point scannable.
- Format: • point 1 • point 2 • point 3

### No Summary Paragraphs
- Do NOT write prose summary paragraphs at the end of any step output
- Tables are self-explanatory. The chat summary (given verbally after each step) serves as the recap.
- The only prose section allowed is "Why Hero" (2-4 sentences) in the final assembly

### Data Attribution (Apply to ALL outputs)
Every data point must include:
1. **Date range** — when the data is from (e.g., "Jan 2026", "Q1 2025 vs Q1 2026", "Oct 2024–Feb 2026 avg")
2. **Metric source** — which tool/report produced it (e.g., "SQP CVR", "BR CVR", "Ad CVR", "Metrics Engine", "Seller Central Audit")
3. **Sample qualifier** (for rate metrics like CVR, CTR, ATC) — Confidence is a judgment call based on (in order): relevancy to product → search volume → meaningful clicks (20-30+ generally, depends on brand scale, never 2 clicks) → purchases/CVR. Not rigid thresholds — scale of brand matters. A small brand with 15 clicks on a keyword may be meaningful. A large brand with 15 clicks is noise.

Format: `metric value (source, date range, sample if applicable)`
Example: "SQP ATC rate 10.1% (SQP MCP, all-time through Q4 2024, 713 brand clicks)"
Example: "Sessions -50% YoY (Metrics Engine BR, Q1 2025: 4,105 vs Q1 2026: 2,035)"

### CVR Types — Never Say Just "CVR"
Three different CVRs exist. Never use bare "CVR" — always prefix:
- **BR CVR** (Business Report, session-to-order) — product's own conversion over time. NOT comparable to market.
- **SQP CVR** (Search Query Performance, click-to-purchase) — brand vs industry, directly comparable. Use THIS for market comparison.
- **Ad CVR** (ad click-to-order) — ad traffic only.
When showing own performance → BR CVR. When comparing to market → SQP Brand CVR vs SQP Industry CVR. Always separate rows in output tables.

### Verify Everything from Live Data
The Sales Doc and Seller Central Audit are **starting points, not conclusions**. Every factual claim about the account must be independently verified by querying live data (Metrics Engine, SQP MCP, Metabase). This includes:
- Campaign structure ("how many campaigns exist", "no manual campaigns")
- Performance metrics ("ROAS is X", "buy box is Y%")
- Product status ("ASIN is dead", "no sales")
- Market position ("impression share is X%")

**Three outcomes for any claim:**
1. **Verified** — you queried live data and confirmed it. State: "Verified via [tool] ([date range])"
2. **Unable to verify** — you tried to query but the tool returned no data or errored. State: "Unable to verify — [tool] returned [issue]. Per Sales Doc/Audit ([date])."
3. **Not included** — if you can't verify and the claim is central to a recommendation, do not include it. Don't build analysis on unverified assumptions.

**The ONLY things trusted from the Sales Doc without verification:**
- Client goals / stated objectives (verbatim)
- Client notes / preferences / budget tolerance
- Contact details
- Anything the CLIENT said (not the sales team's interpretation)

Everything else in the Sales Doc — business overview, financials, strategy suggestions, competitive claims — is **context to inform your queries**, not facts to repeat.

### Google Research
Always do Google research FIRST before reading sales doc for brand/market context.

## MCP Tools

| Tool | Use For |
|---|---|
| Metrics Engine MCP | Revenue, sessions, CVR, buybox, ads, campaigns, targeting, placements, search terms, SQP ASIN-level |
| SQP MCP | Brand-level keywords, share trends, volume alerts, keyword tagging |
| Notion MCP | Read sales docs, store outputs |
| Metabase | Listing quality data via asin-product-data skill (read only) |

See `common/available-tools.md` for full documentation.

## Step Boundaries

| Step | Does | Does NOT Do |
|---|---|---|
| Step 00 (Context) | Baseline understanding, product list, keyword landscape | Hero selection, keyword tiering, ICAP, market sizing, deep time analysis |
| Step 01 (Hero ASIN) | Revenue ranking, potential scoring, constraint identification | Root keywords, keyword tiering, ICAP at keyword level |
| Step 02 (Hero Keywords) | Root groups, hero keywords, tiering, ICAP verification, branded check | Campaign recommendations, strategy, bid actions |

## Directory Structure

```
common/                          ← Read first. Shared context.
  amazon-knowledge-context.md    ← Metrics, dimensions, benchmarks, causality map
  available-tools.md             ← MCP tools documentation

processes/onboarding/
  onboarding-process.md          ← Full process doc
  steps/
    00-context-gathering.md      ← Executable prompt
    01-hero-asin-selection.md    ← Executable prompt
    02-hero-keywords-icap.md     ← Executable prompt
  output-schemas/
    summary-table.md             ← 1-screen summary format
    context-output.md            ← Step 00 output format
    hero-asin-output.md          ← Step 01 output format
    hero-keywords-output.md      ← Step 02 output format

mcp/
  mcp-config.json                ← MCP connection endpoints
```

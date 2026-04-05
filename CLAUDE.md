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
- Run in order: 00 → 01 → 02
- Each prompt reads the previous step's output from Notion
- Each prompt writes output to Notion
- Output must follow the exact schemas in `processes/onboarding/output-schemas/`

### Output Rules
- **Summary table** (from output-schemas/summary-table.md) is the primary deliverable — fits on 1 screen
- **Detailed analysis** follows the hero-asin-output.md and hero-keywords-output.md schemas
- **Context reference** follows context-output.md — stored as a separate Notion page (internal only)
- Output goes to the brand's row in the "Onboarding Outputs" Notion database under "Onboarding Prompts!"
- After each step, update the Status property on the brand's row

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

### Sales Doc
Additional context only — NOT source of truth (except client goals/notes which ARE truth).

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

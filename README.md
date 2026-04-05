# SERC Onboarding Process & Prompts

Amazon brand onboarding system — process definition, executable prompts, and shared context.

## What This Is

A structured process for onboarding new Amazon brands. Given a new brand, this system produces:
1. Brand & market context
2. Hero ASIN(s) — which product to focus on and why
3. Hero keywords — which keyword groups to target
4. Constraints — what's broken and where in the funnel
5. A 1-screen summary table with all decisions

## Architecture

```
common/                  ← Shared across all processes
  amazon-knowledge-context.md   ← Metrics, dimensions, Amazon terminology, causality map
  available-tools.md            ← MCP tools documentation

processes/
  onboarding/            ← Brand onboarding process
    onboarding-process.md       ← Full process doc (constraints, flow, data diagram)
    steps/                      ← Executable prompts (run in Claude Code)
      00-context-gathering.md
      01-hero-asin-selection.md
      02-hero-keywords-icap.md
    output-schemas/             ← Exact output formats
      context-output.md
      hero-asin-output.md
      hero-keywords-output.md
      summary-table.md

  account-check/         ← Future: alerts, ongoing monitoring

mcp/
  mcp-config.json        ← MCP connection config
```

## How to Use

1. Read `common/` files for context on metrics and tools
2. Read `processes/onboarding/onboarding-process.md` for the full flow
3. Run prompts in order: 00 → 01 → 02
4. Each prompt reads previous output from Notion
5. Each prompt writes output to Notion
6. AM can intervene between any step

## Tech Stack

- **Metrics Engine MCP** — revenue, sessions, CVR, ads, campaign data, SQP ASIN-level
- **SQP MCP** — brand-level keywords, trends, volume alerts, tagging
- **Notion MCP** — sales doc input, output storage
- **Metabase** — listing data, additional reports (read only)

## Output Storage

Outputs go to Notion (not this repo). This repo contains process definitions and prompts only.

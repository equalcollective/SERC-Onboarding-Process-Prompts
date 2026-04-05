# Onboarding Prompt Suggestions

Collected feedback from onboarding runs. Each suggestion is tracked here and applied to the relevant prompt/schema files. Once applied, marked with ✅.

**Status key:** ✅ = applied to prompt/schema files | ⏳ = logged, not yet applied

---

## ✅ Suggestion 1: Specify CVR source, time range, and sample size in summary output
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** `output-schemas/summary-table.md` > "Why Hero" field, and any constraint/potential field referencing CVR
**Problem:** The summary said "CVR outperforms market by +2.3pt" without specifying:
- Which CVR? (Business Report CVR, Ad CVR, or SQP CVR — these are very different metrics)
- What time range? (single month vs aggregated)
- What sample size? (a CVR from 5 clicks is meaningless vs 500 clicks)
**Suggestion:** Require all CVR claims in summary output to include:
1. **Metric source** — "SQP CVR (click-to-purchase on Tier 1 keywords)" or "BR CVR (sessions-to-orders)" or "Ad CVR (ad clicks-to-orders)"
2. **Time range** — "Mar 2025" or "Q1 2025 aggregate"
3. **Sample qualifier** — "from X brand clicks" or "based on X sessions" — if sample is <50 clicks, flag as "low confidence"
4. **Consistency** — if the metric is sporadic (e.g., 0% most months, spike in one month), say so rather than presenting the spike as representative

**Example fix:**
- Before: "CVR +2.3pt above market"
- After: "SQP CVR +2.3pt above market on Tier 1 keywords (5.4% vs 3.1%, Mar 2025, 37 brand clicks — low confidence, sporadic signal)"

---

## ✅ Suggestion 2: Every data claim must include a date range
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** ALL output schemas — `summary-table.md`, `hero-asin-output.md`, `hero-keywords-output.md`, `context-output.md`
**Problem:** Data points like "impression share is 0.2-0.5%" or "sessions -50% YoY" or "$33K/mo addressable market" don't tell the reader WHEN. Data without a date range is ambiguous — the reader doesn't know if it's last week, last quarter, or a 12-month average. This matters because metrics change over time and seasonality is a factor.
**Suggestion:** Enforce a global rule across all output schemas: every number must include its date range in parentheses immediately after.
- Applies to: revenue figures, percentages, share metrics, volume numbers, YoY/MoM comparisons, market sizing
- Format: `metric (date range)` — e.g., "0.23% imp share (Jan 2026)" or "$5,769 (Q1 2026: Jan-Mar)"
- For comparisons: include both periods — e.g., "Sessions -50% YoY (Q1 2025: 4,105 vs Q1 2026: 2,035)"
- For ranges spanning multiple data points: state the window — e.g., "0.2-0.5% imp share (Jan-Mar 2026)"

**Example fixes:**
- Before: "Tier 1 market: ~$33K/mo addressable"
- After: "Tier 1 market: ~$33K/mo addressable (based on Jan 2026 SQP search volume × market purchase rate)"
- Before: "impression share is 0.2-0.5% on core keywords"
- After: "impression share is 0.2-0.5% on Tier 1 keywords (Jan-Mar 2026)"

---

## ✅ Suggestion 3: Verify campaign structure claims from live data, not just the sales audit
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** `steps/01-hero-asin-selection.md` (constraint identification), `steps/02-hero-keywords-icap.md` (ICAP diagnosis)
**Problem:** The onboarding output stated "zero non-branded manual PPC campaigns exist" and "$8.65 total spend on Tier 1+2 keywords" as facts. These came from the **Seller Central Audit** Notion page (written pre-onboarding by the sales team), not from a live query of campaign/targeting data. The audit could be stale — campaigns may have been created between the audit date and the onboarding run.
**Suggestion:** Add a required verification step in Step 01 or Step 02:
1. **Pull live campaign data** from Metrics Engine — query `ad_spend`, `ad_sales` with `campaign` and `targeting` dimensions for the hero ASIN over the last 30-90 days
2. **Cross-check against audit claims** — confirm whether campaigns cited in the audit still reflect reality
3. **Source-tag the claim** — if stating "zero manual campaigns," specify: "Verified via Metrics Engine targeting query (date range)" or "Per Seller Central Audit (audit date) — not independently verified"
4. **Flag if audit is >2 weeks old** — campaign structures change fast; stale audit data should be treated as context, not truth

**Why it matters:** Claiming "zero campaigns exist" when they actually do exist (just launched recently) would make the entire ICAP diagnosis wrong — the blocker might not be impressions if campaigns are already running.

---

## ✅ Suggestion 4: Sales Doc / Audit = starting point, not conclusion — verify everything or don't include
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** `CLAUDE.md` global rules, all step prompts
**Problem:** The onboarding run treated Seller Central Audit claims as facts (e.g., "zero non-branded campaigns", campaign ROAS numbers, buy box history) without independently verifying via live data queries. The audit is written by the sales team pre-onboarding and could be stale or wrong. Building recommendations on unverified claims is risky.
**Suggestion:** Strengthen the verification rule to three clear outcomes:
1. **Verified** — queried live data, confirmed. Tag it: "Verified via [tool] ([date range])"
2. **Unable to verify** — tried but tool returned no data. Tag it: "Unable to verify — per Sales Doc/Audit ([date])"
3. **Not included** — if it can't be verified and it's central to a recommendation, don't include it at all

Only things trusted from Sales Doc without verification: client goals (verbatim), client preferences, contact details — anything the CLIENT said. Everything else (business overview, financials, strategy, competitive claims) is context to inform queries, not facts to repeat.
**Applied to:** `CLAUDE.md` > "Verify Everything from Live Data" section (rewritten), Sales Doc rule tightened

---

## ✅ Suggestion 7: Distinguish BR CVR vs SQP CVR — always use SQP CVR for market comparison
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** ALL output schemas, `CLAUDE.md` global rules, `steps/01-hero-asin-selection.md`, `steps/02-hero-keywords-icap.md`
**Problem:** The output said "CVR outperforms market" without specifying which CVR. There are three different CVRs:
- **BR CVR** (Business Report) = sessions-to-orders on the listing page. Measures product's own conversion. CANNOT be compared to market/industry.
- **SQP CVR** (Search Query Performance) = clicks-to-purchases on search results. Brand vs Industry directly comparable.
- **Ad CVR** = ad clicks-to-orders. Only measures ad traffic.
Mixing them up or being ambiguous leads to wrong conclusions.
**Suggestion:** Enforce strict rules:
1. When showing the product's own conversion performance over time → use **BR CVR**, label as "BR CVR (session-to-order, Metrics Engine)"
2. When comparing to market/industry → MUST use **SQP CVR**, label as "SQP Brand CVR vs SQP Industry CVR (click-to-purchase)"
3. Never say just "CVR" — always prefix with BR/SQP/Ad
4. In Hero ASIN Summary table: include BOTH as separate rows — BR CVR for own performance, SQP CVR vs Industry for market comparison
**Applied to:** `CLAUDE.md` (Data Attribution rule updated), all output schemas (rules added), `hero-asin-output.md` (CVR fields split into BR and SQP), Notion output (Hero ASIN Summary table rebuilt with both CVR types)

---

## ✅ Suggestion 5: Include monthly revenue view in Step 00 context (not just weekly)
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** `steps/00-context-gathering.md`, `output-schemas/context-output.md`
**Problem:** Step 00 only presented an 8-week weekly snapshot. Monthly view makes MoM trends immediately visible (e.g., -42.7% Jan→Feb, +67.8% Feb→Mar) without the reader having to mentally aggregate weeks. Weekly is good for recent momentum; monthly is better for MoM/seasonal patterns.
**Suggestion:** Step 00 should include BOTH:
1. **Monthly view** (12 months if available, minimum 6) — with a MoM % change column for instant trend reading
2. **Weekly view** (last 8 weeks) — for recent momentum and WoW patterns
Both views should be in the output. Monthly first (big picture), weekly second (recent detail).
**Applied to:** `steps/00-context-gathering.md` (added monthly pull), `output-schemas/context-output.md` (updated Revenue Overview field), Notion output (monthly table added)

---

## ✅ Suggestion 6: SQP keyword landscape table must include brand AND industry CVR + ATC rate
**Date:** 2026-04-05
**Source:** Goal Crazy onboarding run
**Applies to:** `steps/00-context-gathering.md` (SQP pull section), `output-schemas/context-output.md` (Key Search Terms field)
**Problem:** The Step 00 SQP keyword table only showed impression share + CTR. Missing brand vs industry CVR and ATC rate. Without these, the reader can't tell if the product actually converts on these keywords — which is the most important context for Step 01/02 decisions.
**Suggestion:** The SQP keyword landscape table in Step 00 must include ALL funnel metrics:
- Volume, Period, Imp Share (existing)
- Brand CTR vs Industry CTR (existing)
- **Brand ATC rate vs Industry ATC rate** (NEW)
- **Brand CVR vs Industry CVR** (NEW)
- Where brand CVR or ATC is 0%, note why (e.g., "0 purchases — insufficient clicks from low imp share")
- Where sample is <50 clicks, flag "low confidence" on rate metrics
**Applied to:** `steps/00-context-gathering.md` (SQP output table updated), `output-schemas/context-output.md` (Key Search Terms description expanded), Notion output (table rebuilt with all funnel metrics)

---

## ⏳ Suggestion 8: Clean up Notion database properties
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** Notion Onboarding Outputs database, `CLAUDE.md`, `steps/03-final-assembly.md`
**Changes:**
- Remove from visible properties: Onboarded Date, SQP Brand Name, Seller ID, Marketplace (operational data — store in page body or notes, not properties)
- Move Primary Constraint above Notes in property order
- Notes should be last column
- Seller ID / SQP Brand Name needed by prompts → store in page body at top, not as database properties
**Status:** ✅

---

## ⏳ Suggestion 9: Verify all assumptions from sales doc independently
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** All prompts, `CLAUDE.md`
**Problem:** Claims like "zero non-branded PPC" were taken from sales audit without live verification in some places.
**Change:** Every structural/factual claim must be verified via live data query. If can't verify, tag "Unable to verify — per Sales Doc ([date])". Never state as fact without verification.
**Status:** ✅

---

## ✅ Suggestion 10: Move summary table fields to database properties — remove separate summary section
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/summary-table.md`, `steps/03-final-assembly.md`, Notion database
**Problem:** Summary table inside the page repeats what's already in database properties (Hero ASIN, Constraints, Hero Root Groups, Client Goal). Redundant.
**Change:** Key fields live in database properties directly. Inside the page, only keep "Why Hero" as a short section (needs more than one line). Seasonal, market position etc can go in Notes property. Remove the separate summary table section from the page body.
**Status:** ✅

---

## ✅ Suggestion 11: Brand context should be brief — deep product/keyword analysis belongs in later steps
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `steps/00-context-gathering.md`, `output-schemas/context-output.md`
**Changes:**
- Context = brief brand story + client goal + product catalogue (what exists) + basic revenue sense
- Remove detailed keyword table from context (just list top 10-15 keywords with volume, no rates)
- Remove detailed listing quality analysis from context — belongs in hero ASIN section
- Remove pricing analysis from context — belongs in hero ASIN section
- Revenue in context: last 3 months MoM or 4-5 weeks WoW is enough. Not 8 weeks + 12 months.
- Full SQP landscape table removed — just keyword names + volume. Detailed SQP with rates belongs in Step 02.
**Status:** ✅

---

## ✅ Suggestion 12: Remove "Flags for Next Steps" — distribute to relevant sections
**Date:** 2026-04-05
**Source:** Output review (both Lakhan + Claude)
**Applies to:** `steps/00-context-gathering.md`, `output-schemas/context-output.md`
**Problem:** Step 00 has "Flags for Next Steps" which makes decisions that belong in Step 01 (e.g., "impression share is the primary blocker"). Context shouldn't make decisions.
**Change:** Remove the standalone "Flags" section. If a flag is important (e.g., buy box instability), it goes in the relevant constraint section in Step 01/02. Context only gathers, never concludes.
**Status:** ✅

---

## ⏳ Suggestion 13: SQP CVR check should cover all relevant keywords, not just Tier 1
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `steps/01-hero-asin-selection.md` (Dimension 2: Potential)
**Problem:** Currently potential check uses "relevant keywords" which defaults to Tier 1. But CVR vs industry should be checked across Tier 1 + Tier 2 minimum.
**Change:** Potential check pulls CVR vs industry on ALL relevant keywords (Tier 1 + Tier 2). Tier 1 tells you about core intent. Tier 2 tells you about broader market position. Both needed for full picture.
**Status:** ✅

---

## ⏳ Suggestion 14: Don't trust pre-defined SQP tags — build tiering from scratch
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `steps/02-hero-keywords-icap.md`
**Problem:** If existing tags exist in SQP from a previous run or sales audit, the LLM might use them as starting point.
**Change:** Add explicit rule: "Do NOT use existing SQP tags as starting point. Build tiering from scratch using product understanding + SQP data. If existing tags exist, verify them independently. Override if your analysis disagrees."
**Status:** ✅

---

## ✅ Suggestion 15: Hero ASIN detail table is redundant if only 1 hero — merge into summary
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-asin-output.md`, `steps/01-hero-asin-selection.md`
**Change:** If 1 hero ASIN, no separate "Hero ASIN Summary" table — data lives in database properties + "Why Hero" section. If 2+ heroes, add columns to one table (not separate tables). Remove the separate "Hero ASIN Verdict" prose paragraph — table speaks for itself.
**Status:** ✅

---

## ✅ Suggestion 16: Monthly revenue table — only last 3 months, not 12
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `steps/01-hero-asin-selection.md`, `output-schemas/hero-asin-output.md`
**Problem:** Full 12-month monthly table is overkill. The 3/6/9/12mo aggregated row already shows the trend.
**Change:** Show last 3 months monthly detail only. The 3/6/9/12mo + YoY aggregated numbers cover the rest. If seasonality needs evidence, reference the aggregated YoY comparison, don't dump 12 rows.
**Status:** ✅

---

## ✅ Suggestion 17: Child ASIN breakdown — show recent month + 3mo/6mo avg, not just one month
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-asin-output.md`, `steps/01-hero-asin-selection.md`
**Problem:** Child breakdown showed only January data — a single snapshot. No trend.
**Change:** Child ASIN table should show: most recent complete month + 3mo avg + 6mo avg. This gives "where is it now" + "where has it been."
**Status:** ✅

---

## ✅ Suggestion 18: Revenue in constraints is a symptom, not a cause — reorder
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-asin-output.md`, `steps/01-hero-asin-selection.md`
**Problem:** Revenue decline is listed alongside sessions/CVR/ROAS as a "constraint." But revenue is the OUTCOME of other metrics declining — it's the symptom, not the cause.
**Change:** Constraints focus on causal metrics (sessions, CVR, buybox, ROAS, impression share, etc.). Revenue decline is noted as the consequence but separated — either at the top as "the problem statement" or at the bottom as "the result." Show healthy metrics briefly too (status: healthy/declining per metric) but only detail declining ones.
**Status:** ✅

---

## ✅ Suggestion 19: One hero keywords table, not two — keep hero (must-win) only
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-keywords-output.md`, `steps/02-hero-keywords-icap.md`
**Problem:** Root keyword groups table (ALL tiered) + Hero root groups table are two separate tables. The full tiered list is reference material.
**Change:** Keep only the Hero Root Groups (must-win) table as the primary output. The full tiered list is captured in SQP tags and can be a collapsed/reference section, not a primary table. Reduces redundancy.
**Status:** ✅

---

## ✅ Suggestion 20: Fix column value inconsistency in keyword/ICAP tables
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-keywords-output.md`
**Problem:** ICAP blocker column values sometimes bleed into adjacent columns in Notion rendering. Values shift across cells.
**Change:** Enforce rule: one value per cell, no multi-line values in table cells. Keep cell content short. If explanation needed, put it in a separate "Notes" column, not crammed into the metric cell.
**Status:** ✅

---

## ✅ Suggestion 21: Remove ICAP diagnosis/fix column — just show constraints
**Date:** 2026-04-05
**Source:** Output review
**Applies to:** `output-schemas/hero-keywords-output.md`, `steps/02-hero-keywords-icap.md`
**Problem:** The ICAP table has a "Fix" column with recommendations. But onboarding identifies WHAT's broken, not HOW to fix it. Fix recommendations belong in strategy/campaign phase.
**Change:** Remove "Fix" column from ICAP table. Keep: ASIN, keyword group, funnel blocker, brand rate, market rate, gap. That's it.
**Status:** ✅

---

## ✅ Suggestion 22: Remove Step 02 summary paragraph — ICAP table speaks for itself
**Date:** 2026-04-05
**Source:** Claude review
**Applies to:** `steps/02-hero-keywords-icap.md`
**Problem:** "Step 02 Summary" paragraph at the end of Step 02 output repeats the ICAP diagnosis table in prose. Redundant.
**Change:** Remove the summary paragraph. The ICAP table + hero root groups table are self-explanatory. Chat summary (5 lines) serves as the verbal recap.
**Status:** ✅

---

## ⏳ Suggestion 23: Cross-step references instead of re-stating facts
**Date:** 2026-04-05
**Source:** Claude review
**Applies to:** `steps/02-hero-keywords-icap.md`, output schemas
**Problem:** Step 02 re-states facts from Step 01 (e.g., "sessions -50% YoY", "zero non-branded campaigns") in full instead of referencing them.
**Change:** When Step 02 needs to reference a Step 01 finding, write "per Step 01 constraint #1" instead of re-stating the full fact. Reader can scroll up or check the constraint table. Reduces repetition across 3-5 instances per output.
**Status:** ✅

---

## ✅ Suggestion 24: Add confidence column to rate metric tables
**Date:** 2026-04-05
**Source:** Claude review
**Applies to:** `output-schemas/hero-keywords-output.md`, `output-schemas/hero-asin-output.md`
**Problem:** Rate metrics (CVR, CTR, ATC) based on small samples are flagged in text but not structurally in the table. Reader has to read fine print.
**Change:** Add a "Confidence" column to tables showing rate metrics. Values: "High (500+ events)" / "Medium (50-500)" / "Low (<50 events)". Makes it immediately visible which numbers to trust.
**Status:** ✅

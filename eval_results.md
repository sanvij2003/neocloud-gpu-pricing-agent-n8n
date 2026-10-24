# Evaluation results: Neocloud GPU Pricing Agent (n8n)

Runs: Oct 23 to 24, 2026. Orchestrator: openai/gpt-oss-120b (Nebius). Sub-agents: Qwen3-30B-A3B-Instruct-2507 (Nebius). Search: Tavily.
Ground truth: provider pricing pages checked by hand on Oct 23, 2026 (coreweave.com/pricing, lambda.ai/pricing).

## 1. Failure tests (6 of 6 passed)

| Test | What we did | Result | Caveat |
|---|---|---|---|
| F1 Invalid input | Sent "QWERTY Cloud" | Pass. Asked to clarify; no tools called (4.6 s, 1,966 tokens) | Rule depends on model knowledge: it also asked to clarify "IREN" (renamed from Iris Energy) |
| F2 Broken news search | tavily_news given a fake API key | Pass. Every news call failed ("Tavily rejected the API key"); run continued; no invented headlines | Brief says "none found", so a failure is reported as an absence |
| F3 Reject competitors | Disapproved gate 1, typed "CoreWeave, IREN, Lambda" | Pass. Re-gated with the typed list | Before the fix, the orchestrator invented IREN's ticker, model and URL; after the fix it writes "user-supplied" |
| F4 Reject brief | Clicked "Request changes" at gate 2 | Pass. No row saved until approval; revision applied 3.5 of 4 requested fixes | Agent describes a human rejection as "I wasn't able to save automatically"; the re-save wrote prices_json = "PLACEHOLDER" |
| F5 No rate card | Target IREN | Pass. All IREN prices "not found"; nothing invented | No implied price derived from IREN's ARR disclosures; in run 5 (different target) IREN was assigned Microsoft's price |
| F6 Memory | Second NBIS run after a saved brief | Pass. "Changes since last brief" read the saved prices | Cannot tell a real price change from a corrected misread (CoreWeave B200 spot $4.36 to $4.26 was a fix, not a move) |

## 2. Full runs

| # | Target / competitors | Time (incl. gate wait) | Tokens | Completed + saved | Main issues |
|---|---|---|---|---|---|
| 1 | NBIS vs CoreWeave, Lambda, "Crusade Energy" | 3:47 | 279,143 | Yes | Invented company name and SEC claim passed gate 1; Lambda prices from a blog; CoreWeave B300 on-demand invented |
| 2 | NBIS vs CoreWeave, IREN, Lambda | n/a | n/a | Yes | CoreWeave H100/H200 missed; Lambda "no public pricing" (false); "cheapest" claim built on missing data |
| 3 | NBIS vs CoreWeave, Lambda, GMI Cloud (F2) | 2:38 | 280,148 | Yes | Lambda H100 "$0.50" misread (fixed later by code range check); two Nebius pages disagree |
| 4 | IREN vs Nebius, CoreWeave, Lambda (F4, F5) | 10:53 | 331,469 | Yes (after revision) | Snapshot contradicted table (fixed via F4); prices_json saved as "PLACEHOLDER" |
| 5 | CRWV vs Nebius, IREN, Lambda | 4:42 | 702,328 | Yes | research_agent hit its 12-iteration cap on IREN; Microsoft Foundry price attributed to IREN |

Time includes human review at both gates, so agent-only time is lower.

## 3. Price accuracy vs ground truth

| Provider | Ground truth (per GPU-hour) | Agent result | Score |
|---|---|---|---|
| CoreWeave | H100 $6.16 / spot $2.46; H200 $6.31 / $2.62; B200 $8.60 / $4.26; B300 contact sales / spot $4.48; GB200 $10.50; GB300 contact sales | Run 1: 7/8 (B300 on-demand invented). Runs 3 to 5: all listed cells correct; B300 on-demand "not found" vs "contact sales" | High once code normalization was added |
| Nebius | docs.nebius.com: H100 $3.85 / spot $2.15; H200 $4.50 / $2.45; B200 $7.15 / $3.95; B300 $7.85 / $4.30. nebius.com/prices: H100 $4.50, H200 $5.40, B200 $8.50, B300 $9.50 | Same values in every run; conflict between the two official pages flagged in runs 3 and 5 | Consistent; which page is current not verified by hand |
| Lambda | lambda.ai/pricing shows B200 1-Click Cluster (reserved, 2 weeks to 1 year): $9.86 / $9.36 / $8.87; on-demand instance prices not verified | Run 1: blog price $2.59; run 2: none; run 3: misread $0.50 to $0.87 (excluded by code); runs 4 and 5: none | 0 of 5 runs correct |
| IREN | No public rate card (contracts) | Runs 2 and 4: "not found" (correct); run 5: Microsoft Foundry price misattributed | Mostly correct; one serious misattribution |

## 4. Discovery agent (gate 1 input)
Errors in 4 of 5 runs: "Crusade Energy" (twice, source was crusoe.ai), CoreWeave labeled "private", GMI Cloud "contract-only" from a 2024 article, a fake company named "Neocloud", Applied Digital (a CoreWeave data-center landlord) labeled "public rate card". Every error was visible at gate 1; one passed because it was approved without careful reading.

**Caveat found in the workflow export:** the stricter discovery prompt (exact names from the source, sourced tickers, "unverified" when unsupported) was written but never applied; every run above used the original discovery prompt. The same is true of the extraction "minimum term = reserved" rule. These two fixes are untested in this evaluation.

## 5. What the evidence shows
1. **Code-enforced rules held; prompt-only rules did not.** Unit conversion, GPU scope, blog-source flags and price range checks (all in code) worked every time after they were added. The research tool budget, the "no blog prices" rule, "only cited sources" and "no memory" (prompt only) were broken repeatedly; only the hard iteration cap stopped the research loop.
2. **The human gates caught every discovery error that a reviewer read.** Their value depends on the reviewer actually reading.
3. **Comparability is harder than extraction.** Providers publish different products (on-demand instances, reserved clusters, dynamic spot, contract-only). Labeling the price type correctly matters as much as the number.
4. **Missing data can turn into false conclusions** ("cheapest" when competitors were not found), so the brief needs explicit gap statements.
5. **Revision loops can quietly damage saved data** (prices_json "PLACEHOLDER"); only checking the table showed it. The save_brief field description still asked for "the normalized array" while the orchestrator prompt asked for prices_compact, a mismatch that may have contributed.
6. **Check the export, not the plan.** The exported workflow showed two prompt fixes that were never applied, the same lesson as Week 2.

## 6. Cost
Nebius spend: fill in from the Nebius usage page (Week 2 total was $0.09; balance before Week 3 was $27.57). Tavily stayed on the free plan. Typical full run: 250k to 330k tokens; worst case 702k when the research agent looped.

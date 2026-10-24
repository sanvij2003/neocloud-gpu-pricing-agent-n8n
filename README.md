# Neocloud GPU Pricing Agent (n8n)

A multi-agent research system that compares GPU-hour pricing across AI neoclouds (Nebius, CoreWeave, IREN, Lambda and others) and writes a sourced competitor brief for investors. Built for The Gen Academy "Mastering Agentic AI" Week 3 project (use case 3A, Market Research Agent: Competitor Analysis), no-code / low-code track.

## What it does
You type a company (for example `Nebius (NBIS)`). The agent:
1. Checks for a previous brief on that company (memory).
2. Finds 3 direct competitors (discovery agent + web search).
3. **Pauses for your approval of the competitor list** (gate 1). You can approve or replace it.
4. Researches each company's official pricing page and recent news (research agent).
5. Turns the notes into structured price rows (extraction agent, JSON schema).
6. **Converts every price to $ per GPU-hour in code**, computes price gaps, flags conflicting sources, blog sources and implausible prices, and lists changes since the last brief (`normalize_prices`, JavaScript).
7. Writes a fixed-format brief.
8. **Pauses for your approval of the brief** (gate 2). You can approve or request changes.
9. Saves the approved brief and prices for the next run's comparison.

## Architecture
```
Chat Trigger
  -> Orchestrator (AI Agent, gpt-oss-120b, Simple Memory, max 25 iterations)
       Tools:
       ├─ get_past_brief        Data table read (briefs)
       ├─ discovery_agent       AI Agent Tool, Qwen3-30B
       │     └─ Tavily search
       ├─ research_agent        AI Agent Tool, Qwen3-30B, max 12 iterations
       │     ├─ tavily_search
       │     ├─ tavily_news
       │     └─ tavily_extract
       ├─ extraction_agent      AI Agent Tool, Qwen3-30B + Structured Output Parser
       ├─ normalize_prices      Code Tool (JavaScript): units, gaps, conflicts, range checks
       ├─ gate_1_competitors    Human review (chat approval)
       │     └─ save_competitor_list   Data table write
       └─ gate_2_brief          Human review (chat approval)
             └─ save_brief             Data table write
  -> Chat (response)
```
Both write actions sit behind a human-review gate: n8n blocks the save tool until the user clicks Approve.

## Stack
| Part | Choice |
|---|---|
| Workflow | n8n Cloud 2.43 |
| Orchestrator model | `openai/gpt-oss-120b` via Nebius Token Factory |
| Sub-agent models | `Qwen/Qwen3-30B-A3B-Instruct-2507` via Nebius Token Factory |
| Web search and page extraction | Tavily (official n8n node) |
| Memory | n8n Simple Memory (session) + Data tables (across sessions) |

## Repository layout
```
README.md                                  this file
neocloud_pricing_agent.json                n8n workflow export (no credentials inside)
eval_results.md                            failure tests, 5 scored runs, findings
run5_research_agent_max_iterations.json    error evidence from run 5
NBIS_run3.md                               sample brief (Nebius)
IREN_run.md                                sample brief (IREN, after requested changes)
```

## How to run it
1. Import `neocloud_pricing_agent.json` into n8n (Workflows > Import from file).
2. Create credentials and select them on the nodes:
   - **Nebius**: OpenAI-type credential, base URL `https://api.tokenfactory.nebius.com/v1`, your Nebius API key. Used by all four model nodes.
   - **Tavily account**: your Tavily API key. Used by the four Tavily nodes.
3. Create two Data tables (all columns string) and select them on the data table tools:
   - `competitor_runs`: target, competitors, run_date
   - `briefs`: target, run_date, brief, prices_json
4. Open the chat and send a company with its ticker, for example `Nebius (NBIS)` or `IREN Limited (IREN)`.
5. Read the competitor list carefully at gate 1, then read the brief at gate 2 before approving.

Keep the chat trigger private. Never put API keys in node fields or prompts.

## Evaluation summary
Full details in ` eval_results.md`.
- **Failure tests: 6 of 6 passed** (invalid input, broken search key, rejected competitor list, rejected brief, company with no rate card, memory comparison), each with a documented caveat.
- **CoreWeave prices** matched the provider page once unit conversion moved into code (H100 $6.16, H200 $6.31, B200 $8.60 per GPU-hour).
- **Lambda prices** were wrong or missing in all 5 runs (blog sources, a misread $0.50, reserved vs on-demand confusion).
- **Discovery** made errors in 4 of 5 runs; the human gate exposed every one.
- **Main lesson:** rules enforced in code held every time; rules that lived only in prompts were broken repeatedly.

## Known limitations
- List prices only. Large customers sign private multi-year contracts that the brief cannot see.
- The orchestrator's step order is enforced by the prompt, so it can slip.
- Research is sequential; a full run takes about 2.5 to 5 minutes and 250k to 700k tokens.
- Pricing pages change, render poorly for extraction, and mix products (on-demand, reserved clusters, dynamic spot).
- The memory comparison cannot tell a real price change from a corrected misread.
- After a revised brief, the saved price string was once a placeholder.

## Next steps
- Save prices from code instead of passing them through the model.
- Enforce per-company tool budgets in code, not prompts.
- Schedule weekly runs to build a spot-price history per GPU model.
- Add Crusoe, Together AI and RunPod with verified pricing pages.

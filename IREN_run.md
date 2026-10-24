# Neocloud GPU pricing brief: IREN Limited vs Nebius, CoreWeave, Lambda (2026-10-24)

> Sample output, run 4 (Oct 24, 2026). Final version after the user clicked "Request changes" at gate 2 and asked for four fixes (failure test F4). Facts in the company snapshots (Microsoft agreement, 150k GPUs, Nostrum acquisition, financing figures) were not verified by hand. Source [22] is a 2024 Nebius blog post mismatched during renumbering. The saved prices_json for this run was "PLACEHOLDER" (see eval/results.md).

## Summary
- Cheapest on‑demand price per GPU model is offered by Nebius (e.g., H100 on‑demand $3.85 [2]), with CoreWeave generally higher and IREN providing no public rates.
- Cheapest spot price per GPU model is also Nebius (e.g., H100 spot $2.15 [2]), again lower than CoreWeave and with IREN lacking any listed spot rates.
- Recent news: IREN expands its AI Cloud platform to Europe through the acquisition of Nostrum Group (2026‑10‑20) [13].

## Price table ($ per GPU‑hour)
GPU | Price type | IREN | Nebius | CoreWeave | Lambda
--- | --- | --- | --- | --- | ---
B300 | on‑demand | not found [1] | $7.85 [2] | not found [3] | not found [4]
B300 | spot | not found [1] | $4.30 [2] | $4.48 [3] | not found [4]
B200 | on‑demand | not found [1] | $7.15 [2] | $8.60 [3] | not found [4]
B200 | spot | not found [1] | $3.95 [2] | $4.26 [3] | not found [4]
H200 | on‑demand | not found [1] | $4.50 [2] | $6.31 [3] | not found [4]
H200 | spot | not found [1] | $2.45 [2] | $2.62 [3] | not found [4]
H100 | on‑demand | not found [1] | $3.85 [2] | $6.16 [3] | not found [4]
H100 | spot | not found [1] | $2.15 [2] | $2.46 [3] | not found [4]
GB200 | on‑demand | not found [1] | not found [2] | $10.50 [3] | not found [4]
GB300 | on‑demand | not found [1] | not found [2] | contact sales [3] | not found [4]
GB300 | spot | not found [1] | not found [2] | not offered [3] | not found [4]
blended | spot | not found [1] | not found [2] | not found [3] | not found [4]

## Price gaps vs IREN Limited
None identified (no public pricing for IREN to compare).

## Changes since last brief
First brief for this company.

## Company snapshots
**IREN Limited**: no public rate‑card; offers H100, H200, B200, B300, GB300 GPUs; announced a $9.7 bn multi‑year Microsoft agreement and expanded capacity to 150 k GPUs [5][6]. Recent news: Europe expansion via Nostrum acquisition [13].

**Nebius**: public rate‑card model; on‑demand prices $3.85‑$7.85 [2]; spot prices $2.15‑$4.30 [2]; financing facility $3.5 bn with Citi [10]; opened compute pricing to European AI builders [22].

**CoreWeave**: public rate‑card model; on‑demand prices $6.16‑$10.50 [3]; spot prices $2.46‑$4.48 [3]; financing via $3.7 bn convertible notes [14]; launched CoreWeave Forge [15].

**Lambda**: public rate‑card model; no prices found in the table; no recent news found.

## Conflicts and gaps
- **IREN**: all price cells are "not found", a gap for every GPU model and price type.
- **Lambda**: all price cells are "not found", a gap for every GPU model and price type.

## Sources
[1] https://iren.com/solutions/gpu-cloud/ai-cloud | provider page | retrieved 2026-10-24
[2] https://docs.nebius.com/compute/resources/pricing | provider page | retrieved 2026-10-24
[3] https://www.coreweave.com/pricing | provider page | retrieved 2026-10-24
[4] https://lambda.ai/pricing | provider page | retrieved 2026-10-24
[5] https://iren.com/resources/blog/iren-signs97-billion-agreement-with-microsoft-to-deploy-ai-cloud-infrastructure | company blog | retrieved 2026-10-24
[6] https://iren.com/resources/news/iren-expands-ai-cloud-capacity-to-150000-gpus | company news | retrieved 2026-10-24
[10] https://nebius.com/newsroom/nebius-secures-3-5-billion-financing-facility-with-citi | filing | retrieved 2026-10-24
[13] https://iren.com/resources/news/nostrum-announcement | news | retrieved 2026-10-24
[14] https://www.reuters.com/legal/transactional/coreweaves-ai-contracts-lenders-financing-its-build-out-2026-10-15 | news | retrieved 2026-10-24
[15] https://www.coreweave.com/blog/coreweave-forge-turn-ai-iteration-into-compounding-improvement | blog | retrieved 2026-10-24
[22] https://nebius.com/blog/posts/digest-august-2024 | blog | retrieved 2026-10-24

## What this brief cannot see
Large customers usually sign private multi‑year contracts. Those prices are not public and can differ from list prices.

# Neocloud GPU pricing brief: Nebius vs CoreWeave, IREN, Lambda (2026-10-24)

> Sample output, run 3 (Oct 23, 2026, evening). Saved as produced by the agent, errors included. See eval/results.md for scoring: CoreWeave H100/H200 were missed in this run (they are on coreweave.com/pricing), Lambda "no public pricing" is false, and the "cheapest" claim rests on missing competitor data. The memory comparison ("Changes since last brief") worked.

## Summary
- Cheapest on‑demand GPU price: Nebius H100 on‑demand at $3.85 per GPU‑hour [1].
- Nebius on‑demand prices are lower than CoreWeave for all listed GPUs; IREN and Lambda have no public pricing.
- Recent news: CoreWeave announced its Fully Connected 2026 conference series and the delivery of NVIDIA Vera Rubin at scale (2026‑10‑02, 2026‑10‑20, 2026‑09‑30) [3][4][5].

## Price table ($ per GPU-hour)
GPU | Price type | Nebius | CoreWeave | IREN | Lambda
--- | --- | --- | --- | --- | ---
B300 | on_demand | $7.85 [1] | not found | not found | not found
B300 | spot | $4.30 [1] | $4.48 [2] | not found | not found
B200 | on_demand | $7.15 [1] | $8.60 [2] | not found | not found
B200 | spot | $3.95 [1] | $4.36 [2] | not found | not found
H200 | on_demand | $4.50 [1] | not found | not found | not found
H200 | spot | $2.45 [1] | not found | not found | not found
H100 | on_demand | $3.85 [1] | not found | not found | not found
H100 | spot | $2.15 [1] | not found | not found | not found
GB200 | on_demand | not found | $10.50 [2] | not found | not found
GB300 | on_demand | not found | contact sales [2] | not found | not found
GB300 | spot | not found | not offered [2] | not found | not found

## Price gaps vs Nebius
- B300 spot: CoreWeave $4.48 vs Nebius $4.30 = +$0.18 per GPU‑hour (+4.19%)
- B200 on_demand: CoreWeave $8.60 vs Nebius $7.15 = +$1.45 per GPU‑hour (+20.28%)
- B200 spot: CoreWeave $4.36 vs Nebius $3.95 = +$0.41 per GPU‑hour (+10.38%)

## Changes since last brief
- Nebius H200 spot price changed from $2.89 to $2.45 (‑$0.44) per GPU‑hour.

## Company snapshots
**Nebius**
- Business model: public rate card
- GPU offering: B300, B200, H200, H100 (per‑GPU pricing)
- Positioning: European AI cloud for developers and enterprises
- Recent news (last 30 days): none found

**CoreWeave**
- Business model: public rate card
- GPU offering: B200, B300, GB200, GB300 (node‑based pricing)
- Positioning: Fast‑growing AI cloud focused on large‑scale training workloads
- Recent news (last 30 days):
  - 2026‑10‑02 | Fully Connected 2026: Superintelligence Happens When Pioneers Connect [3]
  - 2026‑10‑20 | CoreWeave Fully Connected 2026 Keynote: Introducing CoreWeave Forge | Built for Adoption [4]
  - 2026‑09‑30 | CoreWeave Delivers NVIDIA Vera Rubin at Scale [5]

**IREN**
- Business model: user‑supplied
- GPU offering: not disclosed
- Positioning: user‑supplied
- Recent news (last 30 days): none

**Lambda**
- Business model: user‑supplied
- GPU offering: not disclosed
- Positioning: user‑supplied
- Recent news (last 30 days): none

## Conflicts and gaps
- Conflicts: none identified.
- Gaps: No public pricing found for IREN and Lambda; several GPU models not listed for CoreWeave (e.g., H100, H200).

## Sources
[1] https://docs.nebius.com/compute/resources/pricing | provider page | retrieved 2026-10-24
[2] https://www.coreweave.com/pricing | provider page | retrieved 2026-10-24
[3] https://www.coreweave.com/blog/fully-connected-2026-superintelligence-happens-when-pioneers-connect | company news | retrieved 2026-10-24
[4] https://www.coreweave.com/resources/videos/coreweave-fully-connected-2026-keynote-introducing-coreweave-forge-built-for-adoption | company news | retrieved 2026-10-24
[5] https://www.coreweave.com/news/coreweave-delivers-nvidia-vera-rubin-performance-at-scale-starting-with-cognition | company news | retrieved 2026-10-24

## What this brief cannot see
Large customers usually sign private multi‑year contracts. Those prices are not public and can differ from list prices.

# Results

## Functional benchmark (iteration-2)

| Metric | With Skill | Without Skill | Delta |
|--------|------------|---------------|-------|
| Pass rate | **100%** ± 0% | ~59% ± 36% | +0.41 |
| Date (UTC) | 2026-10-27T03:39:27Z | | |

Source: `benchmarks/iteration-2/benchmark.json`.

All three functional cases passed with skill: FRIGG dual abstracts, human-overclaim, missing AI counterpart.

## Trigger hillclimb

| | Train acc | Test acc | Train recall | Notes |
|--|-----------|----------|--------------|-------|
| Baseline (iter0) | 91.67% | 100% | 75% | FN: missing-AI counterpart not covered in description |
| Winner (iter1) | **100%** | **100%** | **100%** | Added 缺对照 path + near-miss exclusions |

Held-out test failures after winner: **none**.

Details under the private workspace `trigger-hillclimb/` (not required to run the skill).

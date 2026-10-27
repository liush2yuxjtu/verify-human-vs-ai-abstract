# Evals for verify-human-vs-ai-abstract

## Functional evals (`functional-evals.json` / `evals.json`)

Three prompts with checkable expectations:

| id | name | What it stresses |
|----|------|------------------|
| 1 | frigg-wm2-abstracts | Real dual abstracts; writing PASS + results BLOCKED/incomparable + `first` honesty risk |
| 2 | ai-clearer-human-overclaim | Human overclaims → must NOT declare human writing better |
| 3 | missing-ai-counterpart | Single abstract → SKIP/BLOCKED; never invent AI baseline |

Run with your skill-creator / executor loop: with_skill vs without_skill, grade against `expectations`, aggregate into `benchmark.json`.

## Trigger evals (`trigger-evals.json`)

20 queries with `should_trigger` labels (CN+EN). Split recorded under `split` (seed 42, ~60% train / 40% test).

Scoring: from **name + description only** (do not load the skill body), judge whether you would open this skill. Report accuracy / precision / recall on train and held-out test.

Near-miss negatives cover code review, wasm/passkey verify skills, style-only rewrite, translation, LaTeX, plagiarism, plotting, pitch copy, and paper radar.

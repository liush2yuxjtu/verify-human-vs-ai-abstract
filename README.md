# verify-human-vs-ai-abstract

验证**人写**学术摘要是否比 **AI 生成**摘要更好 —— 证据优先，分面判定，不凭语感。

Evidence-first skill to verify whether a **human-written** academic abstract is better than an **AI-generated** counterpart. Separate `writing_better` from `results_better`. Statuses: PASS / FAIL / BLOCKED / SKIP.

## When to use / 何时使用

- 两版摘要（人写 + AI）问谁更好 / 「人写更好吗」 / 投稿前比摘要
- 核对 `first` / SOTA / 巨大提升等声明是否诚实
- 缺 AI 对照但仍按人写 vs AI 流程 verify（此时 `writing_better` = SKIP/BLOCKED，禁止宣称人写更好）

**不要用于：** 普通代码 review、wasm/游戏可玩性、passkey 核验、纯文风润色、翻译、LaTeX、查重、只画实验图、海报 pitch、论文雷达。

## How / 怎么用

1. 加载 [`SKILL.md`](SKILL.md)
2. 按面打分：结构（[`references/five-move.md`](references/five-move.md)）、量表（[`references/rubric.md`](references/rubric.md)）、诚实性、数字可核验、可比性
3. 分别给出 `writing_better` 与 `results_better`；缺对照走「缺 AI / 人写对照」强制段

## Evals

- Functional cases: [`evals/functional-evals.json`](evals/functional-evals.json)（亦见 `evals/evals.json`）
- Trigger queries: [`evals/trigger-evals.json`](evals/trigger-evals.json)
- How to interpret: [`evals/README.md`](evals/README.md)
- Benchmark snapshot: [`benchmarks/iteration-2/`](benchmarks/iteration-2/) · summary [`RESULTS.md`](RESULTS.md)

Functional with-skill pass rate on iteration-2: **100%** (without-skill ~59%).

## Layout

```
SKILL.md
references/five-move.md
references/rubric.md
evals/functional-evals.json
evals/trigger-evals.json
evals/README.md
benchmarks/iteration-2/
RESULTS.md
LICENSE
```

## License

MIT — see [`LICENSE`](LICENSE).

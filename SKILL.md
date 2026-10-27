---
name: verify-human-vs-ai-abstract
description: >-
  验证人写学术摘要/论文 abstract 是否比 AI 生成版更好（writing_better / results_better）。在以下情况使用：用户给出人写与 AI
  两版摘要并问谁更好、「人写更好吗」、投稿前比摘要、核对 first/SOTA/巨大提升是否诚实、或争论谁更适合会议 abstract；即使用户暂时只有一篇、缺
  AI/人写对照但仍按人写 vs AI 流程来 verify abstract，也加载本 skill（对 writing_better 给
  SKIP/BLOCKED，禁止宣称人写更好）。不要用于：普通代码 review、wasm/游戏可玩性、passkey/密钥核验、纯文风润色（无 AI
  对照摘要）、翻译摘要、LaTeX 排版、查重/plagiarism、只画实验表图、海报 pitch、论文雷达/日报检索。按 claim→surface→evidence 出
  PASS/FAIL/BLOCKED/SKIP；默认中文报告。
---
# 验证人写 vs AI 学术摘要

写法对齐 [dot-pi](https://github.com/liush2yuxjtu/dot-pi) `agent/skills/verify`：**先有证据再下结论**，每个面一个硬判定，不许部分通过，不许为了绿灯削弱检查。

## 铁律

```
没有对本轮两篇摘要做打分证据，不许说「人写更好」
没有表格 / 论文 / 可复核来源，不许说「实验也更好」
```

本轮若未按下面各面对 H/A 打分，就不能说人写更好。语感、「读起来顺」、作者偏好都不算证据。

**写作质量与实验优势是不同主张。** 人写可以写作 PASS，同时实验数字仍是 BLOCKED 或 FAIL。

默认用**中文**写验证报告；用户用其他语言提问时跟随用户。

## 意图

判断**人写学术摘要**相对**AI 生成摘要**，在会议/期刊投稿写作质量上是否更好；并单独判断任何「结果也更好」的说法是否已被核实。

## 何时启用

出现以下任一情况就加载（即使没说「verify」）：

- 用户贴出人写 + AI 两版摘要，问谁更好 / 人写是否更好
- 投稿前拿 AI 稿对照打磨摘要
- 争论 “first”、巨大提升、或人写稿是否能投

仅有一篇摘要、无 AI 对照时：走「缺 AI / 人写对照」；`writing_better`=`SKIP`/`BLOCKED`，**绝不**说人写比 AI 更好（即使用户问「好不好」）。

## 主张与候选

先写清主张，例如：「会议摘要写作上，人写 H1 优于 AI 稿 A1。」

冻结候选：

- **H** — 人写正文（粘贴或路径 + 哈希/mtime）
- **A** — AI 正文（粘贴或路径 + 哈希/mtime）
- 可选 **论文/表/仓库** — 用于数字核对

若 H 与 A 报告的基线或指标不同，先记下来，再谈「实验结果更好」。

## 面（Surface）对照表

| 主张轴 | 核对面 | 必需证据 |
|---|---|---|
| 写作结构 | 两篇对照五步模板 | H/A 每步有/弱/无（[references/five-move.md](references/five-move.md)） |
| 写作量表 | 两篇 1–5 打分 | 每维分数 + 一句理由（[references/rubric.md](references/rubric.md)） |
| 声明诚实 / 新颖性风险 | H/A 中的绝对声明（“first”“SOTA”、夸张提升） | 本轮 Web/arXiv 相关工作；搜不到则该面 BLOCKED |
| 数字可核验 | H/A 每个定量说法 | 对上论文表 / 仓库日志 / 公开网页；否则 FAIL/BLOCKED |
| 指标可比性 | 每个 delta 的基线+指标+数据集 | 对比设定相同，否则 FAIL(不可比) |
| 可选：实验优势 | 同一协议下的同一批表 | 并排数字；禁止只凭文案推断 |

## 步骤（按序）

1. **重述主张** — 拆成 `writing_better` 与（若宣称）`results_better`。
2. **冻结候选** — 引用或路径固定两篇正文；打分时不偷偷改写。
3. **结构面** — 按五步给 H/A 打分（背景/缺口 → 本文动作 → 方法 → 结果 → 启示）。见 [references/five-move.md](references/five-move.md)。
4. **量表面** — Clarity / Objective / Results punch / Impact / Claim honesty（1–5）。写作胜出要求：H 在 Clarity+Objective+Impact 上领先，且 H 的 Claim honesty 未因误导性绝对声明而 FAIL。
5. **诚实/新颖性面** — 搜独特表述、方法名、“first …”。与 “first” 重叠的相关工作 → 标风险；无范围限定且有明确先验 → 该稿此面 FAIL。
6. **数字面** — 列出每个数字/delta；先论文表后网页。无来源的独特数字 → 该稿实验声明 FAIL(可核验性)，不自动否定写作。
7. **可比性面** — H/A 基线不同则 `results_better` 不能 PASS。
8. **边角探针** — 任选一项：字数、未定义缩写堆砌、或结果埋在末句。
9. **判定** — 每面一个状态，再套总规则。

禁止删面、削弱面或跳面来刷绿。网页搜索不可用时，诚实/联网数字面标 **BLOCKED**，不得标 PASS。

## 状态

每个面恰好一个状态——不许部分通过：

- **PASS** — 证据支持该面上的主张
- **FAIL** — 证据反对，或结构/诚实性被破坏
- **BLOCKED** — 到不了所需证据面（无表、搜不到、缺 AI/人写稿）
- **SKIP** — 本次询问不在范围内（例如只要写作、不要实验）

## 总判定规则

### `writing_better`（人写写作更好）

- **PASS**：结构偏 H，量表 Clarity+Objective+Impact 偏 H，且 H 的 Claim honesty 不是 FAIL。
- **FAIL**：A 赢下这些写作面，或 H 只靠虚假绝对声明显得更清楚。
- **BLOCKED**：缺任一篇正文，或无法打量表。

### `results_better`（人写报告的实验更好）

- **PASS**：所引 delta 数字可核验 PASS，且指标可比 PASS，且核实后的数字支持人写所宣称的设定。
- **FAIL**：与表格冲突，或实际不可比却文案暗示全面更优。
- **BLOCKED**：无表/论文/网页来源（未发表草稿常见）。
- 用户只问摘要「人写更好」时：评 `writing_better`；除非同时宣称实验更优，否则 `results_better` 为 **SKIP**。

两主张永不可合成一个绿灯。


## 缺 AI / 人写对照（强制）

若用户**只给一篇**摘要、没有 H 与 A 两份正文，或明确缺对照稿：

1. **禁止**给出任何形式的「人写优于 AI / 人写更好 / human better than AI」结论（包括委婉的「这篇可以」「质量不错所以更好」）。
2. `writing_better` 必须是 **SKIP**（询问范围是人写 vs AI 对比）或 **BLOCKED**（缺对照导致无法打分）——二者择一写进总判定表，**不得**写 PASS/FAIL。
3. **禁止编造** AI 基线或「典型 AI 摘要」来凑对比。
4. 报告总判定表第一行必须类似：`writing_better | SKIP 或 BLOCKED | 缺对照，不做 H>A 结论`。
5. 可附**单篇**结构/诚实性点评，但必须单独标明「单篇点评 ≠ 人写 vs AI 胜负」；单篇 FAIL（例如无数字）**不得**解读为人写赢了 AI。
6. 结尾只要求用户补 AI 稿或改做「单篇五步点评（非对比）」，不要替用户宣布胜负。

触发词示例：「帮我看看摘要好不好」「verify my abstract」且正文只有一段时，一律走本节，不要默认当成已有 AI 对照。

## 报告

先给总判定（`writing_better`、`results_better`）。再每面一行：状态 + 证据（引文、分数、URL、表引用）。最后给落败或风险稿的最小修改清单（如收窄 “first”、并入 AI 的局限句、补基准名）。

先保留原始打分再解释。报告语言跟随用户（默认中文）。

## 参考

- [references/five-move.md](references/five-move.md) — 五步摘要清单
- [references/rubric.md](references/rubric.md) — 1–5 量表卡
- 规范来源：dot-pi `agent/skills/verify/SKILL.md`

---
name: verify-human-vs-ai-abstract
description: >-
  给学术摘要/会议 abstract 打分或对已有摘要做 hillclimb（可人写 vs
  AI、也可甲/乙/多稿、也可单篇迭代）。默认主轴是读者兴趣、贡献论证与 story 是否讲清（hook / problem–gap–hook / why
  it matters）；真实性与实验细节由正文表保证，无表时 results 标 BLOCKED
  但不自动否定写作胜出。也用于：投稿前比摘要、盲评谁更抓人、改稿前后对照打分、核对 first/SOTA 风险（诚实面单独记，不盖过
  story）。不要用于：普通代码 review、wasm/游戏可玩性、passkey、纯文风润色（无摘要任务）、翻译、LaTeX、查重、只画实验表、海报
  pitch、论文雷达。按 claim→surface→evidence 出 PASS/FAIL/BLOCKED/SKIP；默认中文报告。
---
# 学术摘要评分与 hillclimb

写法对齐 [dot-pi](https://github.com/liush2yuxjtu/dot-pi) `agent/skills/verify`：**先有证据再下结论**，每个面一个硬判定，不许部分通过，不许为了绿灯削弱检查。

公开仓模板：[liush2yuxjtu/verify-human-vs-ai-abstract](https://github.com/liush2yuxjtu/verify-human-vs-ai-abstract)。

## 评分哲学（主轴）

**摘要的主任务是引起阅读兴趣、讲清 story、论证贡献；实验真实性与完整局限由正文（表/附录）保证。**

- **主轴（写作胜出）**：兴趣 / hook、problem–gap–hook、贡献是否可带走、story 是否连贯。细节堆砌、过度降调备忘录式写法，即使「更诚实」也不自动赢。
- **辅轴（风险便签，不单独定写作胜负）**：无限定 “first/SOTA”、空喊大数字却不点数据集/基线名 → 记风险与改稿建议；**不得**仅因 Claim honesty 更高就判另一篇写作失败。
- **实验轴**：有正文表才谈 `results_better`；无表 → `results_*` = **BLOCKED**，并写明「真实性交正文，本轮只评摘要叙事」。

依据与延伸阅读（评分时按需引用，不必全文复述）：

- [Nature NSMB: Scientific writing 101](https://www.nature.com/articles/nsmb0210-139) — abstract 抓 big picture、别 bogged down in details、entice 往下读
- [How to … write an abstract (Clinical Teacher)](https://ora.ox.ac.uk/objects/uuid:be59ebfd-cc0c-49af-9955-cc800da4c756/files/rzk51vh96f) — story not study；problem–gap–hook；摘要是促销文档
- [Story, Not Study（AACE / Lingard 系材料）](https://www.aaceonline.com/common/Uploaded%20files/AACE_Files/ICEC2025/StoryNotStudyArticle.pdf) — 摘要既 summarize 又 persuade；突出 novelty
- [Let Me Speak! Meeting abstract（PMC）](https://pmc.ncbi.nlm.nih.gov/articles/PMC6294637/) — 叙事引导审稿人；要有料但不能淹没创新点
- [How to write a good abstract…（PMC）](https://pmc.ncbi.nlm.nih.gov/articles/PMC3136027/) — Results 要有具体发现；结论勿 claim 超过数据
- [How research abstracts succeed and fail（OUP Blog）](https://blog.oup.com/2021/11/how-research-abstracts-succeed-and-fail/) — 兴趣与关键假设/范围的平衡；勿 misleading
- [How to write an honest but effective abstract](https://www.sciencedirect.com/science/article/pii/S2468227619307318) — 减 spin，但仍要有效传达贡献
- [Writing an abstract（ANU）](https://www.anu.edu.au/students/academic-skills/research-writing/journal-article-writing/writing-an-abstract) — 独立摘要，帮读者决定是否深读
- [How to write a successful conference abstract（Scenario）](https://doi.org/10.4081/scenario.2025.628) — 避免过空或过载次要细节

默认用**中文**写报告；用户用其他语言提问时跟随用户。

## 铁律

```
没有对本轮候选摘要做打分证据，不许说「谁更好」
没有表格 / 论文 / 可复核来源，不许说「实验也更好」
无表时不得用「更诚实」单独否掉 story 更强的稿
```

语感、作者身份（谁人手写 / 谁 AI）都不算证据。用户要求盲评时，一律称 **甲/乙/丙**（或 Draft-1/Draft-2），禁止在报告里写人写/AI 标签。

**`story_better`（或 `writing_better`）与 `results_better` 永远分开。**

## 意图与模式

本 skill 支持三种用法（自动识别）：

| 模式 | 何时 | 产出 |
|---|---|---|
| **对比打分** | ≥2 篇摘要（人写 vs AI，或任意甲/乙） | 总判定谁 story 更强；可选 results |
| **单篇评分** | 只有一篇 | 五步+量表+风险便签+改稿清单；**禁止**编造对照稿宣布「优于 AI」 |
| **Hillclimb** | 用户要改摘要 / 迭代 / 「下一版更好吗」 | 冻结基线稿 → 打分 → 最小修改 → 对新稿重打 → 对照表（必须分数/面状态相对基线变化） |

## 何时启用

- 比两版摘要谁更好 / 谁更抓人 / 谁 story 更清楚
- 投稿前打磨、改稿前后对照、hillclimb 摘要
- 单篇「给这篇 abstract 打分」
- 盲评（隐去人写/AI 来源）
- 核对 first/SOTA 风险（记辅轴，不单独定写作胜负）

不要默认启用：纯代码 review、游戏可玩性、passkey、无摘要任务的润色、翻译、排版、查重、雷达检索。

## 主张与候选

先写清主张，例如：

- 「会议摘要 **story/贡献论证** 上，乙优于甲。」
- 「Draft-2 相对 Draft-1 的 hillclimb：story 面是否提升。」

冻结候选（路径或全文 + 哈希/mtime；打分中不偷偷改写）：

- **候选集** — 甲/乙/… 或 H/A 或 Draft-n
- 可选 **论文/表/仓库** — 仅用于 `results_*`

基线或指标不同时先记下，再谈实验谁更好。

## 面（Surface）对照表

| 主张轴 | 核对面 | 必需证据 |
|---|---|---|
| Story / 兴趣（**主轴**） | Hook、缺口具体度、贡献可带走、读完是否想点开正文 | 逐句证据 + [references/five-move.md](references/five-move.md) 第1/2/5步偏强 |
| 写作结构 | 五步模板 | 每步有/弱/无 |
| 写作量表 | 1–5 | Clarity / Objective / **Story pull** / Results punch / Impact / Claim honesty（见 rubric） |
| 声明风险（**辅轴**） | first/SOTA/空喊提升 | Web/arXiv；记风险，默认不否决 story 胜出 |
| 数字锚点（辅） | 是否点名数据集/基线/指标 | 有锚点加分；无锚点大数字 → 风险便签 |
| 数字可核验（实验轴） | 每个定量说法 | 对上正文表；无表 → BLOCKED |
| 指标可比 | delta 的基线+指标+数据 | 设定相同否则 FAIL(不可比) |

## 步骤（按序）

1. **识别模式** — 对比 / 单篇 / hillclimb。
2. **重述主张** — `story_better`（可与用户说的 `writing_better` 互换）与可选 `results_better`。
3. **冻结候选** — 盲评时打乱标签为甲/乙，且不在报告中还原身份（除非用户事后要求对照表）。
4. **Story/结构面** — 五步；额外标 hook 强度与贡献句是否可引用带走。
5. **量表面** — 见 [references/rubric.md](references/rubric.md)。**主胜出维：Story pull + Objective + Impact**（并列时看 Clarity，再看 Results punch 是否服务叙事而非堆账本）。
6. **风险辅轴** — first/SOTA/无锚点大数字 → 风险等级 Low/Med/High + 改法；**High 也不自动否决 story PASS**，除非用户显式要求「诚实优先」模式。
7. **数字/可比（仅 results）** — 无表一律 BLOCKED。
8. **Hillclimb 专用** — 列出相对基线：哪些面 ↑/↓/＝；禁止无对照分就说「改好了」。
9. **判定** — 每面一状态，再套总规则。

禁止删面刷绿。用户声明「真实性由正文保证 / 只评兴趣与 story」时：诚实面仍记录，但总判定以 story 主轴为准（诚实优先模式需用户明说才切换）。

## 状态

- **PASS** / **FAIL** / **BLOCKED** / **SKIP** — 每面恰好一个；不许部分通过。

## 总判定规则

### `story_better` / `writing_better`（默认 = 兴趣与贡献叙事）

- **PASS（候选 X）**：Story pull + Objective + Impact 偏 X；结构上贡献/缺口更清楚；读者更可能想读正文。
- **FAIL（相对主张）**：对照稿在这些主维上明显更强。
- **BLOCKED**：缺候选正文或无法打量表。
- Claim honesty **单独成行**；默认不因辅轴 High 而翻转主轴，除非用户要「诚实优先」。

### `results_better`

- **PASS**：表可核验 + 可比 + 数字支持宣称。
- **FAIL**：与表冲突，或不可比却暗示全面更优。
- **BLOCKED**：无表/论文（摘要场景默认常见）。
- 用户只问谁更抓人 / story：`results_better` = **SKIP** 或注明「交正文」。

### Hillclimb

- 新稿相对基线：主轴至少一维明确 ↑ 且无主维崩盘 → 可报「迭代成功」。
- 仅减风险、story 不变 → 报「风险改善、吸引力持平」，勿夸成大胜。

两主张永不可合成一个绿灯。

## 单篇 / 缺对照

1. 禁止编造 AI/人写基线宣布胜负。
2. 可出完整单篇评分 + 改稿清单（供 hillclimb）。
3. 若用户问的是「人写是否优于 AI」但只有一篇：`story_better` 对 H>A 主张 = **SKIP/BLOCKED**，并说明缺对照。

## 报告

1. 模式 + 主张 + 冻结候选（盲评用甲/乙）。
2. 总判定表：`story_better` / `results_better`（及 hillclimb 相对基线）。
3. 各面状态 + 证据句/分数。
4. 风险便签（first、无锚点数字等）。
5. **最小修改清单**（服务下一轮 hillclimb：先补 hook/贡献句，再补数据集·基线名，最后才收窄 first）。
6. 需要时附「乙的叙事骨架 + 甲的锚点」类合成建议。

先保留原始打分再解释。

## 参考

- [references/five-move.md](references/five-move.md) — 五步摘要清单
- [references/rubric.md](references/rubric.md) — 1–5 量表（含 Story pull）
- 上文「评分哲学」中的公开链接
- 规范来源：dot-pi `agent/skills/verify/SKILL.md`

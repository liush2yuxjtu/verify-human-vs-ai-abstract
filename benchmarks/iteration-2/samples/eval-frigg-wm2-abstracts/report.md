# 验证报告：人写摘要 vs AI 摘要（FRIGG / WM2）

## 总判定

| 主张 | 状态 | 说明 |
|---|---|---|
| `writing_better`（人写写作更好） | **PASS** | 结构、Clarity、Objective、Impact 均偏向人写；Claim honesty 为人写标 **风险（非 FAIL）**——笼统 “first” 需收窄，但未因此推翻写作优势 |
| `results_better`（人写报告的实验更好） | **BLOCKED**（且基线 **FAIL(incomparable)**） | 无论文主表/仓库日志可对；两版 delta 对比对象不同，不可从文案推断实验胜负 |

**铁律提醒**：写作 PASS ≠ 实验更好。未提供表格前，不得宣称「人写实验也更好」。

## 候选冻结

- **A（AI）**：开篇即 execution-aware / WM2 / transition network / latent displacement；主结果为 APIGen-MT-5K 上 Top-1 92.5449%、MRR 0.957328、相对 Res32 reader +1.1505pp；含局限（未一致压过更强基线；open-catalog / online task success 未测）。
- **H（人写）**：问题→缺口→FRIGG→世界模型启发的隐状态转移检索→+6.72pp / −68.22%→分析洞见→agent-native paradigm；含 “the first tool retrieval framework to perform retrieval through latent state transitions…”。
- **论文/表**：本轮未提供 → 数字面只能 web / BLOCKED。

## Surface 逐项

### 1. 写作结构（五步）

| 步骤 | H | A | 证据摘要 |
|---|---|---|---|
| 1 背景/缺口 | **有** | 弱 | H 点明 language-mediated retrieval 未用满隐状态；A 直接进方法细节 |
| 2 本文动作 | **有** | 有 | 均提出 FRIGG；A 另点名 WM2 |
| 3 方法 | **有** | 弱（术语堆叠） | H：预测候选工具对应隐状态变化再排序；A：transition network / displacement / teacher 过密 |
| 4 结果 | 有 | 有 | H：+6.72pp、−68.22%；A：92.5449%、MRR、+1.1505pp、717.9 ms |
| 5 启示 | **有** | 弱 | H：agent-native paradigm；A 偏局限收尾，贡献句弱 |

**结构偏向：H**

### 2. 写作量表（1–5）

| 维度 | H | A | 一句话理由 |
|---|---|---|---|
| Clarity | **5** | 3 | H 弧线清楚；A 缩写与机制名密集 |
| Objective | **5** | 3 | H 缺口+目标锋利；A 目标淹没在实例细节里 |
| Results punch | 4 | 4 | 均有数字；H 冲击力强，A 更细但难扫 |
| Impact | **5** | 3 | H 有范式贡献句；A 贡献定位弱 |
| Claim honesty | **3（风险）** | **4** | H 有未收窄 “first” + 大提升缺上下文；A 有混合结果与未测项 |

按 skill：Clarity+Objective+Impact 均 H 胜，且 H 的 Claim honesty **不是** 1–2 的 FAIL → `writing_better` 可 **PASS**。

### 3. 声明诚实 / 新颖性

本轮 Web/arXiv 检索：

- 公开网络**未找到**与摘要一致的 FRIGG/WM2 工具检索论文（同名 Frigg 为仓库检索 MCP，无关：https://frigg.bnomei.com/concepts/retrieval-and-ranking/）。
- 相近先验存在，使 H 的笼统 “first … latent state transitions” 有风险：
  - Lookahead-R（世界模型式、execution-aware 工具检索）https://arxiv.org/abs/2609.35811
  - MassTool（检测塔隐状态注入检索塔）https://arxiv.org/abs/2507.00487
  - TACIT（工具能力需求可从隐状态线性解码）https://arxiv.org/html/2610.08018

**状态**：H 此面 = **风险（需收窄，未判 FAIL）**；A 此面 = **PASS（表述更克制）**。  
建议 H 改为可辩护窄表述，例如「首个用*预测隐状态位移*对候选工具做检索排序的框架」。

### 4. 数字可核验

| 来源 | 数字 | 本轮核验 |
|---|---|---|
| A | 92.5449%、0.957328、+1.1505pp、717.9 ms | 公开 web **无匹配** → FAIL(可核验性) for A 的实验声明 |
| H | +6.72pp、−68.22% | 公开 web **无匹配** → FAIL(可核验性) for H 的实验声明 |
| 基准名 | APIGen-MT-5K、ToolHop、MCP-Atlas、τ²-Bench | 公开存在 → 基准名 PASS；数字仍 BLOCKED/FAIL |

无主表 → **`results_better` 不能 PASS**。

### 5. 指标可比性

- A：相对 **existing Res32 reader**，Top-1 +1.1505pp  
- H：相对 **direct agent tool selection**，Top-1 +6.72pp，时间 −68.22%

对比对象不同 → **FAIL(incomparable)**。不可用两段摘要互相证明「谁实验更好」。

### 6. 边角探针

- A 缩写堆叠（WM2、Res32、MRR、RTX 3090）影响扫读。
- H 缺主基准名与局限句；headline 数字未标明数据集。
- 两版均未给字数是否符合目标 venue 的证据 → 字数面 SKIP。

## 最小修复清单（给 H，保留写作优势）

1. 收窄或删除笼统 “first”，改成可对照先验的窄 claim。  
2. 从 A 并入：主基准名、实例名 WM2（若正文有）、1–2 个可复核主数字、一句局限（open-catalog / online success 未测）。  
3. 明确基线是「直接选工具」还是「Res32 reader」，避免读者与 A 版数字混比。  
4. 投稿前用主表对 +6.72pp / −68.22% 做数字面 PASS，再谈 `results_better`。

## 原始评分笔记（解释前保留）

- five-move: H 有×5；A 步骤1弱、步骤3弱、步骤5弱  
- rubric H: 5/5/4/5/3；A: 3/3/4/3/4  
- writing_better PASS；results_better BLOCKED + incomparable  

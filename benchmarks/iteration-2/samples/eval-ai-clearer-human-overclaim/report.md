# 验证报告：人写 vs AI 短摘要（排序）

## 总判定

| 主张 | 状态 | 说明 |
|---|---|---|
| `writing_better`（人写写作更好） | **FAIL** | 人写 Claim honesty **FAIL**（绝对 “first” + “beats all … 50% everywhere”，与先验工作冲突 / 不可核验）。skill 规则：诚实性已达投稿破坏级 FAIL 时，即使更「冲」，也不能判人写更好。 |
| `results_better` | 对人写 **FAIL** / 整体实验优劣 **SKIP** | 人写「到处 50%」无表且绝对化；AI 的 −1.2% 速度 / −30% 内存仍无日志可核，但 AI **没有**宣称已验证的全面碾压。不可给人写 `results_better` PASS。 |

**结论：不要宣布人写摘要整体更好。** AI 在诚实度与可界定清晰度上更胜；人写未过投稿安全的声明诚实关。

## 候选冻结

- **H：** “We present the first neural method for sorting integers. It beats all prior work by 50% everywhere.”
- **A：** SortNet，小 Transformer；固定 1e5-key 随机基准上比 timsort 慢 1.2%、峰值内存少 30%；明确不宣称渐近意义上的新奇性。

## 各面

### 结构（五步）

| 步骤 | H | A |
|---|---|---|
| 1 背景/缺口 | 无 / 弱（用 “first” 撑场面，无真缺口） | **有**（排序已充分研究 → 收窄到 ML 系统角度） |
| 2 本文动作 | 弱（未命名的 “neural method”） | **有**（SortNet） |
| 3 方法 | 无 | **有**（小 Transformer、合成数组） |
| 4 结果 | 弱/误导（绝对 50%） | **有**（相对 timsort 的具体取舍） |
| 5 启示 | 无 | **有**（不宣称渐近新颖） |

结构偏向：**A**

### 写作量表（1–5）

| 维度 | H | A | 理由 |
|---|---|---|---|
| 清晰度 | 2 | **4** | H 短但空；A 有命名产物 + 基线 |
| 目标 | 1 | **4** | H 无真缺口；A 写清提出什么与评测设定 |
| 结果冲击 | 1 | **4** | H 的「50%」无可用度量范围；A 有具体取舍 |
| 影响 | 1 | 3 | H 假影响；A 中等系统贡献 |
| 声明诚实 | **1 FAIL** | **5** | 网上：神经排序至少自 1989 起已有（Hopfield 排序 https://doi.org/10.1109/ijcnn.1989.118679；1991 感知机精确排序 https://research.tilburguniversity.edu/en/publications/sorting-with-a-neural-net/）。「first neural method for sorting integers」为假；「beats all … 50% everywhere」不可核验。A 收窄指标并否认古典渐近新颖。 |

对人写 `writing_better`：**FAIL**（诚实 FAIL + A 在清晰/目标/影响上胜出）。

### 诚实 / 新颖性（本轮检索）

相关先验：

- Sorting by Hopfield net (1989) — https://doi.org/10.1109/ijcnn.1989.118679  
- Sorting with a neural net / three-layer perceptron (1991)

→ H 的 “first” **FAIL**。另：已有 “SortNet” 文献偏 learning-to-rank，与整数排序同名拥挤，但 A 的*声明*仍是收窄的。

### 数字

- H「到处 50%」：无数据集/基线/指标 → **FAIL(可核验性)**；绝对措辞 → 诚实 FAIL。  
- A「慢 1.2%」「内存少 30%」「1e5-key」：本轮无日志 → 数字面 **BLOCKED**；但 A 未宣称未验证的 SOTA。

### 可比性

H 的「全球 50%」与 A 的 timsort 延迟/内存取舍无共同协议 → 不能用人写数字宣称优于 AI。

### 边角探针

H 像两条口号，过不了任何要求「声明绑定证据」的会务清单。A 虽「没那么刺激」，但更适合投稿。

## 若坚持改人写稿：最小修复

1. 删掉 “first” 和 “beats all … 50% everywhere”。  
2. 给方法命名，写清基线（如 timsort）、数据规模，同时报赢与输。  
3. 像 A 一样加一句「不宣称渐近新颖」。

## 原始记分

- 量表 H 2/1/1/1/1；A 4/4/4/3/5  
- 人写 `writing_better` FAIL；本 skill 下投稿写作更倾向 AI

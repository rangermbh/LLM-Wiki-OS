---
created: 2026-08-06
updated: 2026-08-06
domain: ai
type: reference
reference_type: talk
authors:
  - 骆昱宇 (Yuyu LUO)
year: ""
tags:
  - research-methodology
  - academic-writing
  - phd-training
  - paper-writing
  - data-visualization
status: seedling
sources:
  - "[[capture/inbox/2026-08-06T120000+0800 博士生科研入门辅导|Capture: 博士生科研入门辅导]]"
captured_from: "capture/inbox/2026-08-06T120000+0800 博士生科研入门辅导.md"
ingested_by: claude-opus-4.7
---

# 博士生科研入门辅导（骆昱宇，HKUST(GZ)）

## Citation

骆昱宇. 博士生科研入门辅导[R]. 香港科技大学（广州）数据科学与分析学域，内部讲义（PPT，70 slides）.

> **来源说明**: 本页面基于 PPT 导出 PDF 的 Capture 创建。原文为幻灯片格式，PyPDF2 仅提取了幻灯片上的文本覆盖层（7,343 字符）。额外 48 页通过 macOS Vision OCR 从页面渲染图中补充了图片内文字（+39,152 字符，57.7% 平均置信度）。以下内容中，来自 OCR 层的具体示例标注了置信度；Text Layer（PyPDF2 原文）直接引用不加标注。OCR Tier2 ⚠️ 内容（30-50% 置信度）仅供参考，不作为独立事实来源。

## Summary

这是骆昱宇博士面向刚入门研究生（硕士/博士）的科研方法论讲义，系统梳理了计算机科学（特别是数据科学、可视化、AI 领域）的科研全流程：从研究方向选择、研究问题的定义与评价、到学术论文的结构化撰写和审稿应对。讲义的核心贡献在于提供了一个可操作的「论文生产流水线」框架和一套「新问题 vs 老问题」的研究价值判断方法论，具有较强的跨领域可迁移性。

## Key Insights

### 1. 论文生产流水线模型

讲义隐含了一条完整的科研生产流水线：

```
研究方向 → 具体问题 → 技术创新 → 实验评估 → 论文写作 → 投稿
```

每一个阶段有独立的判断标准和操作方法。流水线的方向性意味着：上游决策的质量约束会向下游累积——方向选错 → 问题定义偏差 → 技术贡献不足 → 实验设计不合理 → 论文叙事无力。

### 2. 新问题 vs 老问题的二分法

| 维度 | 新问题 | 老问题 |
|------|--------|--------|
| 定义 | 因新需求、新技术出现而产生，尚未被明确定义 | 问题已被清晰定义，有成熟 Benchmark |
| 目标 | 解决没人研究过的问题 | 找到更好的方法 |
| 研究者贡献 | 清晰地定义一个好的新问题 | 新技术、新方法 |
| 条件 | 有真实应用背景、能获得真实数据集、结果好衡量 | 更快、更高、更强（性能/准确率/通用性） |
| 风险 | 问题可能不重要或不可解 | 可能只是 incremental work |

**新问题的完整故事链**: 新需求 → 具体问题 → 技术贡献。每个环节都需要清晰的论证。

**老问题的创新路径**: 提高性能 / 提高准确率 / 提高通用性 / 应用到新环境（新硬件、新场景）/ 满足新需求（隐私、算力等）。

### 3. 研究方向选择的四个条件

好的研究方向应同时满足：
1. **有实际需求** — 重要
2. **能吸引人** — 新颖
3. **有较大的未知空间** — 非显而易见
4. **实验室有好的相关积累** — 基础

### 4. 论文结构化写作框架

**Abstract（摘要）— What-Why-How-So What**:
- What: 解决/研究什么问题？（交代背景）
- Why: 为什么写这篇论文？（新问题 or 新方法？）
- How: 大概怎么做？（Key Idea）
- So What: 实验效果如何？

**Introduction（引言）— 摘要的扩充版，论文的总结版**:
- Why: 研究动机（讲故事），问题背景/应用前景
- How: 基本的 Idea，对问题的观察
- What: 论文解决什么问题
- So What: 创新点（技术创新 + 实验结果）
- Roadmap: 文章结构（非必要，通常与创新点融合）

**写作原则**:
- 结构清晰优于语言优美：总分结构、Leading text、Highlight、贯穿全文的形象化例子
- 把读者当「傻子」：先简单，循序渐进展开；逻辑清晰连贯
- 图文并茂：合理使用图、表和公式；用例子解释难懂的技术部分
- 初稿严禁直接使用 ChatGPT 生成（讲义中反复强调）
- 写完一段读一遍；多使用简单句；用好连接词

### 5. 论文各部分的写作策略

**Overview / Framework**: 总分架构；介绍整个 workflow 和 architecture；凝练每个技术点的用途和解决的问题；图文并茂，自包含。

**Technical（技术创新）**: 先介绍为什么设计这类方法、通俗讲清楚核心思路；有算法则提供伪代码并对应解释；复杂算法增加例子阐述理解。

**Related Work**: 引用相关论文并说出具体区别；多夸奖别人、承认别人贡献；保证语言正确，不曲解原意；严禁直接复制它文内容。

**Conclusion**: 总结文章贡献；不要与引言重复；语调不同（引言读者不知细节，结论读者已读完论文）。

### 6. 审稿人视角

**审稿人希望看到**:
1. Novel Problem: 新问题，尤其是定义一个有用的新问题
2. Novel Method: 解决旧/新问题的新颖有效的方法
3. Nice Story: 好的写作、吸引人的故事、逻辑性强
4. Nice Presentation: 排版美观、插图漂亮

**审稿人讨厌看到**:
1. Old school problem, with simple combination of existing methods
2. Worse Presentation: writing, figures
3. Experiment: without strong baselines, inappropriate settings

### 7. 研究者素养

- 脚踏实地（努力），不能「熊瞎子掰棒子」——持续积累而非不断重启
- 规划能力（远见）——了解相关工作、认清论文质量
- 多总结、多借助工具
- 做研究的大忌：没有兴趣、不了解前人工作、浮躁急于求成、马虎、懒惰
- 建立和谐的师生关系：目标一致、相互欣赏、共同进步、契约精神

### 8. 领域特定示例（来自 OCR 补充层）

以下示例来自 OCR 层（Tier1, 50-70% 置信度），说明讲义中使用的具体研究案例：

**DEEPEYE 自动可视化系统**（Slide 11, OCR 65.9%）[^ocr]:
- 研究问题: 结构化数据自动可视化
- 系统架构: DEEPEYE = 自动可视化 + 用户意图 + 干净数据
- 子方向: 自然语言驱动的 / 数据质量感知的 / 领域知识指导的 / 渐进式可视化 / 全自动可视化 / 问答式可视化
- 发表: SIGMOD'23, TKDE'22, ICDE'18, ICDE'20, VLDB'20

**NL2VIS 研究时间线**（Slide 15, OCR 44.4% ⚠️）[^ocr-low]:
- Natural Language to Visualization 是数据可视化领域的一个活跃子方向
- 时间线从 2013 年（Word2Vec, NIPS'13）到 2017 年（Transformer, NIPS'17）再到 BERT 时代
- OCR 对该页的识别碎片化，准确内容建议参考原始 PDF

[^ocr]: 来自 OCR Tier1（≥50% 置信度）。内容可用于理解研究方向，但具体发表信息建议以 DBLP/Google Scholar 验证为准。
[^ocr-low]: 来自 OCR Tier2 ⚠️（30-50% 置信度）。内容仅作方向性参考，不作为独立事实来源。建议对照原始 PDF。

## Relevance

这份讲义是 AI Domain 中**科研方法论**的实践参考。虽然内容定位为 CS/数据科学/可视化领域的入门指导，但其中的核心框架——论文生产流水线、新问题 vs 老问题二分法、What-Why-How-So What 叙事结构——具有跨子领域的可迁移性。

**对 AI Domain 的价值**:
- 为 AI 领域的科研实践提供了结构化的方法论文献
- 「新问题 vs 老问题」二分法与 AI 研究的实际场景高度吻合（如新架构提出 vs Benchmark 刷榜）
- 「论文结构化写作框架」可直接应用于 AI 论文（ICML/ICLR/NeurIPS 等顶会）

**与其他 Domain 的潜在关联**:
- Knowledge Management Domain: 科研流程本质上是结构化的知识生产过程——研究方向选择 = 知识边界识别，论文写作 = 知识的结构化表达
- Vocational Education Domain: 博士培养属于高等教育最高阶段，讲义中的科研训练方法论与「教学关键要素」有结构类比——研究方向（专业设置）→ 研究问题（课程设计）→ 技术方法（教材）→ 实验评估（实训）

## Related Concepts

- [[../concepts/rag|RAG]] — 讲义中 NL2VIS 示例展示了从规则系统到深度学习再到 Transformer 的技术演进路径，与 RAG 的 Pipeline 演进有结构类比
- [[../concepts/transformer-architecture|Transformer Architecture]] — 讲义多次以 Transformer（NIPS'17）为 NL2VIS 技术时间线的关键节点，是科研方法演进的具体案例
- [[../concepts/ai-agent-memory|AI Agent Memory]] — 无直接关联，但科研流程框架中的「论文生产流水线」可作为思考 AI Agent 如何辅助科研的参考

## Notes

- 本页面基于 PPT 导出 PDF 的 Capture。原始文件中 ~20 张幻灯片为纯图片，PPT 演讲者的口头讲解内容不在 Capture 中
- Slide 模板装饰文字（校名、Logo）在 OCR 中被误识别为噪声文本（如 "IhEnONG NONG" ← "THE HONG KONG"），不影响本页面引用
- 讲义的领域示例偏向数据科学/可视化（DEEPEYE、NL2VIS），但其方法论框架对 AI 各子方向通用
- OCR T2 ⚠️ 页面（特别是 Slide 15, 16, 41, 54-56, 67）包含密集的学术图表和时间线，OCR 提取碎片化，建议 Human 在 Obsidian 中打开原始 PDF 对照

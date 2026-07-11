---
created: 2026-07-12
updated: 2026-07-12
domain: ai
tags:
  - vector-embeddings
  - embeddings
  - semantic-search
  - vector-databases
  - representation-learning
status: growing
sources: []
captured_from: ""
ingested_by: claude-opus-4.7
---

# Vector Embeddings（向量嵌入）

## Definition

Vector Embeddings（向量嵌入）是将离散实体（词语、句子、文档、图像）映射到连续向量空间中的稠密数值表示，使得语义相似的实体在向量空间中几何邻近。

## Core Idea

计算机不理解"意思"——它只理解数字。向量嵌入的核心洞见是：**语义可以被编码为高维空间中的位置**。如果"猫"和"猫科动物"的向量在空间中距离很近，而"猫"和"汽车"距离很远，那么这个向量空间就捕捉到了语义关系。

这一洞见的意义在于：它将语义问题转化为几何问题。判断两个概念是否相似，转化为计算两个向量之间的距离。这使机器能够在不"理解"语言的情况下执行语义搜索、类比推理和相似性匹配——它只是在做数学运算，但结果表现出语义智能。

向量嵌入之所以有效，是因为分布式表示假说（distributional hypothesis）：**出现在相似上下文中的实体具有相似的含义**。嵌入模型通过在海量文本上训练，学习将这一假说编码为向量空间的几何结构。

## Mechanism

向量嵌入的生成和使用涉及四个环节：

**1. 输入转换（Tokenization）**：原始文本被切分为 token（词、子词或字符），作为嵌入模型的基本输入单元。切分策略影响嵌入粒度——过粗丢失细节，过细增加计算开销。

**2. 嵌入生成（Embedding Model）**：token 序列通过嵌入模型（通常是 Transformer 编码器）转换为稠密向量。模型输出一个固定维度的实数向量——例如 768 维或 1536 维——每个维度编码了输入文本的某种抽象特征。维度越高，潜在的表达能力越强，但计算和存储成本也越高。

**3. 向量空间（Vector Space）**：所有向量存在于同一个高维空间中。这个空间的结构由嵌入模型的训练决定——相似的实体在空间中聚集，语义关系（如"国王 - 男人 + 女人 ≈ 女王"）表现为向量运算。这种几何结构是下游检索和推理的基础。

**4. 相似度计算（Similarity Measurement）**：语义相似性通过向量间的距离或角度来度量。最常用的三种方法：

| 方法 | 原理 | 适用场景 |
|------|------|----------|
| 余弦相似度（Cosine Similarity） | 测量两个向量之间的夹角 | 方向比大小重要的场景；最常用的文本相似度度量 |
| 点积（Dot Product） | 向量元素乘积之和 | 向量已归一化时等价于余弦相似度 |
| 欧氏距离（Euclidean Distance） | 向量在多维空间中的直线距离 | 向量大小有意义时使用 |

在 RAG 和 Agent Memory 管道中，检索步骤本质上就是：将查询向量化 → 在向量数据库中找最近邻 → 返回最相似的文档向量所对应的原始内容。

## Relationships

### Master Concepts

本页面为以下 Master Concept 提供**领域证据**：

- [[spaces/master/wiki/concepts/structured-minimal-unit-principle|结构化最小单元原则]] — 嵌入模型的基本输入单元（token/chunk）决定了语义捕捉的粒度上限。"过粗丢失细节，过细增加计算开销"——单元粒度必须与语义边界对齐，是"基本单元质量定义系统能力上限"在语义表示层的实例
- [[spaces/master/wiki/concepts/emergent-structure-from-local-interactions|局部交互涌现全局结构]] — 向量空间的结构不是手动设计的。语义关系（king-man+woman≈queen）从分布假说（共现模式→空间邻近）在数十亿训练样本上的应用中涌现。空间结构是局部共现模式积累的全局产物

### Domain Connections

- [[rag|RAG]] — 向量嵌入是 RAG 管道中**编码阶段**的核心技术。RAG 的质量上限由嵌入质量决定：分块策略影响嵌入粒度，嵌入模型决定语义捕捉能力，向量空间结构决定检索精度。嵌入 → 存储 → 检索形成 RAG 的基础数据流
- [[ai-agent-memory|AI Agent Memory]] — 向量嵌入同样服务于 Agent Memory 管道的**编码阶段**。交互数据通过嵌入模型转换为向量后存入长期记忆，后续通过语义相似度检索召回。RAG 和 Agent Memory 共享同一套编码→检索架构，区别在于嵌入的输入来源（外部文档 vs 交互历史）
- [[transformer-architecture|Transformer Architecture]] — 当前主流的嵌入模型（如 BERT、text-embedding-3、bge）基于 Transformer 编码器架构。自注意力机制使嵌入模型能够根据上下文动态调整 token 表示，是上下文感知嵌入的核心技术

## Applications

- **语义搜索（Semantic Search）**：将用户查询和文档库中的内容分别向量化，通过最近邻搜索找到语义相关而非仅关键词匹配的结果。这是向量嵌入最直接的应用——使搜索从"匹配字符串"升级为"匹配含义"
- **RAG 检索**：在 RAG 管道中，向量嵌入将外部知识库和用户查询映射到同一语义空间，使检索步骤能够找到与查询含义相关的文档片段，而非仅靠关键词命中
- **Agent Memory 检索**：在 Agent 长期记忆系统中，过往交互被向量化存储。新查询通过向量相似度搜索召回相关历史，使 Agent 能够"记住"相关的过往经历
- **推荐系统**：用户和内容被映射到同一嵌入空间，"相似用户喜欢的内容"和"相似内容"都可以通过向量近邻搜索实现
- **多模态检索**：文本和图像可以映射到同一向量空间（如 CLIP），实现"用文字搜图片"或"以图搜图"——跨模态的语义对齐

## Limitations

- **语义压缩损失（Semantic Compression Loss）**：将任意长度的文本压缩为固定维度向量，必然丢失信息。长文档的细微差别、修辞手法、反讽、文化语境在向量化过程中可能被抹平。嵌入是一个有损压缩过程
- **上下文依赖性（Context Dependency）**：同一个词在不同上下文中应有不同的向量表示。"苹果"在科技新闻和水果食谱中应当映射到不同的空间位置。静态嵌入模型（如早期的 Word2Vec）无法处理这种差异；上下文感知模型（如 BERT 系列）改善了这一问题，但仍有局限
- **嵌入模型偏差（Model Bias）**：嵌入模型从训练数据中学习语义关系，如果训练数据存在偏见，向量空间也会继承这些偏见。例如，职业与性别的刻板关联会被编码为向量空间的几何结构
- **领域特异性（Domain Specificity）**：通用嵌入模型在专业领域（医疗、法律、工程）可能表现不佳。领域特定的术语和概念关系需要领域微调的嵌入模型才能准确捕捉
- **检索质量依赖（Retrieval Quality Dependency）**：嵌入是管道的第一环，其质量向下游放大或衰减。低质量的嵌入 → 差的检索结果 → LLM 收到无关上下文 → 响应质量下降。管道的复合效应使得嵌入质量成为系统性能的关键瓶颈
- **维度选择困境**：高维度提供更丰富的语义表达能力，但增加存储和计算成本。低维度更高效，但可能丢失区分细微语义差异的能力。没有普适的最优维度——需要在具体场景中权衡

## Sources

- 本页面为 AI Domain 的基础概念节点，由系统基于已验证知识综合创建，作为 RAG 和 Agent Memory 概念页面的共同依赖

## Notes

- 本页面按 knowledge-distillation.md v1.0.0 标准创建，位于 AI Domain 概念网络的基础层
- 向量嵌入是 RAG 和 Agent Memory 的共享前置概念——两个页面均将其列为 planned relationship，本页面补全了这一知识网络的空白节点
- 保留 Vector Embeddings、Embedding、Tokenization、Transformer、BERT、Word2Vec、CLIP、ANN 等标准英文技术术语

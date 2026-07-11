---
created: 2026-07-12
updated: 2026-07-12
domain: ai
tags:
  - transformer
  - self-attention
  - deep-learning
  - neural-network
  - llm
  - foundation-model
status: seedling
sources: []
captured_from: ""
ingested_by: claude-opus-4.7
---

# Transformer Architecture（Transformer 架构）

## Definition

Transformer 是一种基于自注意力机制（Self-Attention）的神经网络架构，它通过并行处理序列中的所有位置来替代循环神经网络的逐步计算，是当前几乎所有大型语言模型、嵌入模型和跨模态 AI 系统的计算基础。

## Core Idea

在 Transformer 之前，序列建模由 RNN/LSTM 主导——它们逐步处理序列，每个时间步依赖上一步的隐藏状态。这带来了两个致命限制：长距离依赖随序列长度衰减，以及无法并行训练。

Transformer 的核心洞见是：**"Attention Is All You Need"——用注意力机制完全替代循环，让序列中的每个位置可以直接关注序列中的任何其他位置。** 这相当于将信息的流动从"传话游戏"（信息必须经过每个中间节点）变成了"圆桌会议"（每个人可以直接与任何其他人交流）。信息路径长度从 O(n) 变为 O(1)，不仅解决了长距离依赖问题，还使得整个序列可以并行处理，训练速度提升了数个数量级。

这个洞见之所以能成立，是因为自然语言中的语义关系本质上是稀疏且成对的——"it"指代"the cat"时，只需要这两个词之间的直接关联，而不需要遍历它们之间所有词的隐藏状态。自注意力恰好提供了这种直接、可微分的配对关联机制。

## Mechanism

Transformer 的标准架构是一个编码器-解码器（Encoder-Decoder）结构，但在实践中分裂为三种主流变体：

### 核心组件

**1. 自注意力（Self-Attention）**：每个输入 token 被投影为三个向量——查询（Query, Q）、键（Key, K）、值（Value, V）。一个 token 对另一个 token 的"注意力"计算为 Q 与 K 的点积相似度，经 softmax 归一化后作为权重对 V 加权求和。注意力分数矩阵的每个元素 (i, j) 表示第 i 个 token 应"关注"第 j 个 token 的程度。

**2. 多头注意力（Multi-Head Attention）**：单个注意力头可能只捕捉一种关系模式（如语法依赖）。多个注意力头并行计算，每个头关注不同的关系维度（语法角色、共指、语义关联等），输出拼接后经线性变换融合。这使模型能同时从多个表示子空间中学习。

**3. 位置编码（Positional Encoding）**：由于自注意力本身对位置是无感知的（"A loves B"和"B loves A"在注意力矩阵中只体现为不同的 token 对，而非顺序），必须显式注入位置信息。原始方案使用正弦/余弦函数（Sinusoidal PE），后续实践中更常用可学习的位置嵌入。

**4. 前馈网络（Feed-Forward Network, FFN）**：每个注意力层之后接一个两层全连接网络（通常是 `ReLU(W1·x + b1)·W2 + b2`），对每个位置独立应用。FFN 可以被理解为存储模型从训练数据中学习到的知识——注意力层负责"查找相关信息"，FFN 负责"基于查到的信息进行处理"。

**5. 残差连接 + 层归一化（Residual Connection + LayerNorm）**：每个子层（注意力和 FFN）的输出与输入相加（残差），然后做层归一化。残差连接使得梯度可以直接传播到浅层，是训练深层 Transformer 的关键。

### 三种架构变体

| 变体 | 结构 | 代表模型 | 用途 |
|------|------|----------|------|
| Encoder-only | 仅使用编码器（双向注意力） | BERT, RoBERTa, DeBERTa | 文本理解、分类、嵌入生成 |
| Decoder-only | 仅使用解码器（因果/单向注意力） | GPT 系列, LLaMA, Claude | 文本生成、对话、推理 |
| Encoder-Decoder | 完整编码器+解码器 | T5, BART, 原始 Transformer | 翻译、摘要、seq2seq 任务 |

**Encoder-only** 允许每个 token 同时关注左右两侧的所有 token（双向注意力），适合需要全局上下文的任务。当前主流的嵌入模型（text-embedding-3、bge）均基于此变体。

**Decoder-only** 使用因果掩码（causal masking）——每个 token 只能关注它自身和之前的 token，不能"偷看"未来位置。这使得模型可以进行自回归生成：逐一预测下一个 token，每次预测基于已生成的部分。GPT 系列证明了：只要规模足够大，decoder-only 模型可以通过下一个 token 预测这个简单目标学习到推理、翻译、编程等复杂能力。

### 计算特性

自注意力的复杂度为 O(n²·d)，其中 n 是序列长度，d 是隐藏维度。这是 Transformer 的主要计算瓶颈——序列翻倍，计算代价翻四倍。FlashAttention 等优化通过利用 GPU 内存层次结构（平铺 + 重计算）在不改变数学结果的前提下大幅降低实际运行时间和内存占用。

## Relationships

### Master Concepts

本页面为以下 Master Concept 提供**领域证据**：

- [[spaces/master/wiki/concepts/structured-minimal-unit-principle|结构化最小单元原则]] — Token 是 Transformer 的不可约基本单元。每个 token 被投影为 Query/Key/Value 向量，自注意力机制在 token 之间建立精确的配对关系。Token 的粒度和语义完整性直接决定注意力质量——这是"系统能力上限由基本单元质量定义"在数学计算层的实例
- [[spaces/master/wiki/concepts/emergent-structure-from-local-interactions|局部交互涌现全局结构]] — Self-Attention 是该原则最精确的数学实例。每个 token 基于 Q·K 配对相似度独立决定对其他 token 的关注程度，没有中心控制器决定句子的全局语义。句子级理解从 token 间局部注意力交互中涌现

### Domain Connections

- [[vector-embeddings|Vector Embeddings]] — Transformer 编码器是现代嵌入模型的架构基础（BERT、text-embedding-3、bge）。自注意力机制使嵌入模型能够根据上下文动态调整每个 token 的向量表示——同一个"苹果"在科技新闻和水果食谱中获得不同的嵌入向量。没有 Transformer 的自注意力，上下文感知嵌入的精度将大幅降低
- [[rag|RAG]] — Transformer 解码器是 RAG 管道**生成阶段**的推理引擎。增强提示（原始查询 + 检索到的文档片段）被送入 Transformer 解码器，通过自回归生成产生最终响应。Transformer 的推理能力决定了 RAG 系统在利用检索到的信息后能产生多高质量的答案
- [[ai-agent-memory|AI Agent Memory]] — 与 RAG 类似，Agent Memory 管道的**整合阶段**由 Transformer 解码器驱动。检索到的记忆上下文被注入提示后，Transformer 负责基于当前查询和历史记忆进行推理并生成响应。Transformer 对长上下文的建模能力直接决定了记忆系统的有效召回范围
- LangChain / LlamaIndex — 提供在 Transformer 模型之上构建检索管道和 Agent 系统的框架级抽象（pages planned）

## Applications

- **大型语言模型（LLM）**：GPT-4、Claude、Gemini、LLaMA——所有现代 LLM 均基于 Transformer 架构。Decoder-only 变体的自回归生成能力使其成为通用文本生成和推理的标准范式
- **嵌入模型（Embedding Models）**：BERT、text-embedding-3、bge、E5 等模型使用 Transformer 编码器将文本转化为语义向量，是 RAG 和语义搜索管道的第一环
- **多模态模型**：Vision Transformer（ViT）将图像切分为 patch 后直接套用 Transformer 编码器；CLIP 使用双编码器（文本 + 图像）在共享嵌入空间中实现跨模态对齐；GPT-4V 和 Gemini 将视觉编码器与 Transformer 解码器结合，实现多模态理解与生成
- **代码生成与理解**：Codex、Copilot、StarCoder 等将代码视为可被 Transformer 建模的序列语言，实现代码补全、生成和翻译
- **蛋白质结构预测**：AlphaFold 使用 Transformer 变体处理氨基酸序列和结构信息，解决生物信息学中的核心挑战

## Limitations

- **二次复杂度（Quadratic Complexity）**：自注意力的 O(n²) 计算和内存成本是根本瓶颈。长文档、高分辨率图像、长对话历史都导致成本非线性增长。FlashAttention 等优化缓解了内存瓶颈，但不改变 O(n²) 的数学本质。这是 Transformer 在长序列场景中的主要限制
- **上下文窗口有限**：尽管上下文窗口已从 GPT-2 的 1024 tokens 扩展到 Gemini 的百万级，但扩展主要依赖工程优化而非架构突破。超长上下文中，模型对中间位置信息的利用效率下降（"Lost in the Middle"现象）——首尾信息比中间信息更容易被关注到
- **不具有真正的推理能力**：Transformer 的自回归生成本质上是条件概率建模——给定前面的 token 序列，预测下一个最可能的 token。这在模式匹配和知识检索方面表现卓越，但不等于逻辑推理。数学证明、多步规划和因果分析的失败表明，Transformer 学到的更多是统计相关性而非因果结构
- **缺乏显式记忆**：Transformer 的知识存储在模型参数中（参数化知识），没有可分离的显式记忆模块。这使得知识更新只能通过重新训练或微调实现，无法在推理时动态添加新事实。RAG 等外部知识检索方案是对此局限的补偿，而非架构层面的解决
- **位置编码泛化差**：标准位置编码难以泛化到训练时未见过的序列长度。RoPE 等相对位置编码方案改善了外推能力，但模型在超长序列上的性能下降仍然是现实问题
- **幻觉问题（Hallucination）**：Transformer 被训练为"尽可能预测下一个 token"，而非"确保预测的事实准确性"。当知识不足时，模型倾向于生成看似合理但事实错误的输出——这是自回归目标的固有风险，而非可以通过更多训练数据完全解决的问题

## Sources

- 本页面为 AI Domain 的架构基础概念节点，由系统基于已验证知识综合创建。Transformer 是 RAG、Agent Memory 和 Vector Embeddings 三个概念页面的共同架构依赖——嵌入模型基于 Transformer 编码器，生成模型基于 Transformer 解码器
- 原始论文：Vaswani et al., "Attention Is All You Need" (2017)

## Notes

- 本页面按 knowledge-distillation.md v1.0.0 标准创建，构成 AI Domain 概念网络的架构基础层
- Transformer 是三个已有概念页面的共同 planned reference——本页面补全了知识网络的最后一个结构缺口，将 AI Domain 从 3 节点网络扩展为 4 节点网络
- 保留 Transformer、Self-Attention、Multi-Head Attention、Encoder、Decoder、Positional Encoding、Feed-Forward Network 等标准英文技术术语
- 保留英文模型名称（BERT, GPT, LLaMA, Claude, Gemini, T5, BART, RoBERTa, DeBERTa, FlashAttention, RoPE, CLIP, ViT, AlphaFold, Codex, Copilot, StarCoder, E5）
- TODO: 创建 LangChain、LlamaIndex 技术页面（Transformer 上层框架）

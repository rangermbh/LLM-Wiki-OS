---
created: 2026-07-11
updated: 2026-07-12
domain: ai
status: active
---

# AI Domain Schema

## Scope

Artificial Intelligence: machine learning, deep learning, NLP, computer vision, reinforcement learning, LLMs, AI alignment, AI safety, and related topics.

## Directory Map

```
ai/
├── schema.md         # This file — domain definition
├── index.md          # Knowledge index
├── log.md            # Change log
├── raw/              # Processed captures (read-only)
├── wiki/
│   ├── concepts/     # Core concepts and definitions
│   ├── methods/      # Techniques, algorithms, procedures
│   ├── technologies/ # Tools, frameworks, infrastructure
│   ├── references/   # Papers, articles, books
│   └── entities/     # People, organizations, projects
└── sources/          # External reference materials
```

## Topics

- Machine Learning fundamentals
- Deep Learning architectures
- Natural Language Processing
- Large Language Models
- AI Alignment & Safety
- Prompt Engineering
- AI Tools & Infrastructure

## Knowledge Types

页面分类依据为**认知姿态（epistemic stance）**，而非主题名称或目录归属。

| 认知姿态 | 核心问题 | 目录 |
|----------|----------|------|
| 理解层（Concept） | 这是什么？它为什么有效？ | `wiki/concepts/` |
| 执行层（Method） | 怎么做？什么时候用/不用？ | `wiki/methods/` |
| 工具层（Technology） | 用什么工具/系统实现？ | `wiki/technologies/` |
| 来源层（Reference） | 信息从哪来？ | `wiki/references/` |
| 实体层（Entity） | 谁/哪个组织/哪个项目？ | `wiki/entities/` |

**关键规则**：同一现实对象可以存在多个知识类型页面，各自持有不同的认知姿态。例如 RAG——当研究其架构和机制时是 Concept；当编写其实现步骤和最佳实践时是 Method；当比较 LangChain 与 LlamaIndex 的 RAG 实现时是 Technology。分类取决于页面目的，而非对象名称。

### 当前实例

| Type | Directory | Examples (existing) | Examples (planned) |
|------|-----------|---------------------|--------------------|
| Concepts | `wiki/concepts/` | Transformer Architecture, Vector Embeddings, RAG, AI Agent Memory | Tokenization, Attention Mechanism, Fine-tuning |
| Methods | `wiki/methods/` | — | Fine-tuning, Chain-of-Thought, RLHF |
| Technologies | `wiki/technologies/` | — | PyTorch, LangChain, CUDA |
| References | `wiki/references/` | — | Papers, books, articles |
| Entities | `wiki/entities/` | — | OpenAI, Hinton, NeurIPS |

## Cross-Domain Connections

<!-- Forward references: these pages will be created as the knowledge base grows. -->

- *Emergence* — relates to emergent behaviors in large models (→ future Master concept)
- *Research domain* — AI research methodology (→ future Domain)
- *Feedback Loops* — training dynamics, RL, self-play (→ future Master concept)

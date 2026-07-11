---
created: 2026-07-12
updated: 2026-07-12
status: active
---

# Language Policy（语言策略）

本文件定义 LLM Wiki OS 如何处理英文和中文在系统文档、Agent 指令、公开仓库和知识页面中的使用。

Version: 1.0.0

---

## 1. 核心原则：知识意义与语言表达分离

LLM Wiki OS 维护**一个**知识系统，但通过**多个**语言表达层面向不同社区。

```
┌─────────────────────────────────┐
│     Knowledge Meaning Layer     │  ← 概念是语言无关的
│   (concepts, relationships,     │
│    mental models, principles)   │
├─────────────────────────────────┤
│  Language Expression Layer      │
│  ┌──────────┐  ┌──────────────┐ │
│  │ English  │  │  中文        │ │  ← 同一知识的不同表达
│  │ (en)     │  │  (zh-CN)     │ │
│  └──────────┘  └──────────────┘ │
│       ↑ 未来可扩展更多语言        │
└─────────────────────────────────┘
```

**核心约束**：

- 不维护两套独立的知识系统。English 和 Chinese 是同一底层知识的两种表达。
- 概念、关系和心智模型在意义层定义一次，在表达层呈现为多种语言。
- 当英文术语比中文翻译更精确时，保留英文原词——技术准确性优先于语言纯粹性。

这意味着：中文**不是**英文的从属翻译层。中文是一等公民的语言表达层，面向中文社区的学习、讨论和实践。

---

## 2. 仓库与社区

LLM Wiki OS 面向两个语言社区，通过不同的仓库入口提供服务。

### GitHub（国际社区）

| 维度 | 策略 |
|------|------|
| 默认语言 | English |
| README | `README.md`（英文） |
| 中文入口 | `README.zh-CN.md` |
| 受众 | 国际开源协作者、英文用户 |

### Gitee（中文社区）

| 维度 | 策略 |
|------|------|
| 默认语言 | 中文 |
| README | `README.zh-CN.md` 作为默认展示 |
| 英文入口 | `README.md` 保留供参考 |
| 受众 | 中文用户学习、讨论和实践 |

**关键约束**：

- 两个仓库代表**同一个项目**，不是 fork 或独立衍生。
- 知识内容（`spaces/`、`capture/`）完全相同。
- 系统文件（`CLAUDE.md`、`protocol/`、`templates/`）完全相同。
- 仅入口文档（README）在不同平台上有不同的默认语言展示。

---

## 3. 系统层语言

**系统层（System Layer）** 包含：

- `CLAUDE.md` — Agent 行为宪法
- `protocol/` — 协议文件（federation、lifecycle、routing、distillation 等）
- `.claude/commands/` — 命令定义（ingest、update、lint、promote、reflect）
- `templates/` — 模板文件
- `FEDERATION.md` — Federation 治理清单
- `docs/project-state.md` — 项目运行时状态
- `docs/document-map.md` — 文档地图

### 机器可读标识符

以下标识符**保持英文，永不翻译**：

| 类别 | 示例 | 原因 |
|------|------|------|
| 文件名 | `CLAUDE.md`, `knowledge-distillation.md`, `ai-agent-memory.md` | 文件系统兼容性、Agent 路径解析 |
| YAML keys | `status`, `source_type`, `captured_from`, `ingested_by` | Agent 解析、模板匹配 |
| 元数据值 | `raw`, `processed`, `seedling`, `growing`, `evergreen` | 状态机契约，翻译会破坏生命周期验证 |
| 命令 | `/ingest`, `/update`, `/lint`, `/promote`, `/reflect` | 命令解析 |
| 协议标识符 | `RAW INPUT`, `INGESTED`, `CURATED`, `ABSTRACTED` | 跨协议引用稳定性 |
| 知识类型 | `concepts`, `methods`, `technologies`, `references`, `entities` | 目录名、模板匹配 |
| Master 类别 | `models`, `principles`, `concepts`, `frameworks` | 目录名、模板匹配 |
| Git commit 类型 | `capture`, `update`, `reflect`, `promote`, `maintenance` | Git 日志聚合和搜索 |

这些标识符是**机器可读的系统契约**。翻译会破坏 Agent 解析、模板匹配和 Git 日志聚合。

### Agent 指令语言

Agent 操作指令（`CLAUDE.md`、command 定义、protocol 文件）以**英文为操作语言**，确保 Agent 行为的一致性和可预测性。协议文件中的人类可读说明可使用中文补充。

### 人类文档语言

面向人类的系统文档（`docs/` 目录中的说明性文件）以**中文为主**，技术术语保留英文并附中文说明：

- 首次出现的重要术语使用 `English（中文）` 格式
- 文件名和路径保留英文原文
- 技术概念的解释使用中文

---

## 4. 知识层语言

**知识层（Knowledge Layer）** 包含：

- Domain Wiki 页面（`spaces/{domain}/wiki/`）
- Master Wiki 页面（`spaces/master/wiki/`）

### 策略：中文为主的解释性语言

Domain Wiki 页面以**中文为主要解释语言**，保留标准英文技术术语。

目标：**提升认知可及性（cognitive accessibility），而非字面翻译。**

### 保留英文原词的技术术语

当英文术语比任何中文翻译都更精确、更通用或更简洁时，保留英文原词：

- **广泛使用的缩写**：RAG、LLM、API、GPU、CPU、URL、HTTP
- **精确的技术术语**：Embedding、Transformer、Vector Database、Semantic Search
- **算法/数据结构名**：HNSW、IVF、k-NN、FLAT
- **框架/工具名**：LangChain、LlamaIndex、Redis、Obsidian

### 术语呈现格式

| 场景 | 格式 | 示例 |
|------|------|------|
| 首次出现 | `English（中文全称）` | RAG（Retrieval-Augmented Generation，检索增强生成） |
| 广泛认知的缩写 | 直接使用 | LLM、API、GPU |
| 较生僻的缩写 | 首次给出全称 | HNSW（Hierarchical Navigable Small World） |
| 概念标题 | 中英并列 | AI Agent Memory（AI Agent 记忆） |

### 核心规则

- Wiki 页面的 Definition（定义）应提供清晰的中文解释
- 技术准确性优先于语言纯粹性——当英文术语更精确时，保留英文
- 不使用生造的中文翻译。如果中文社区普遍使用英文原词（如"RAG"而非"检索增强生成"），则优先使用英文原词

---

## 5. 来源语言策略

采集文件（`capture/inbox/`）中的原始内容不可修改，其语言也是如此。

### 保留

- 原始语言（英文来源保持英文，中文来源保持中文）
- 来源署名和出处信息
- 原始内容的完整性

### 蒸馏转换

当 `/ingest` 将采集文件蒸馏为 Knowledge Concept 时：

- 知识页面以中文撰写（遵循 §4 知识层语言策略）
- 技术术语保留英文原词
- 来源追溯性不可丢失（`captured_from` + `sources` 字段）

### 禁止

- 修改采集文件的原始语言内容
- 用翻译替换原始来源
- 在蒸馏过程中丢失原文的概念含义

---

## 6. 未来多语言扩展

当前 LLM Wiki OS 支持 English 和 Chinese（zh-CN）两种语言。架构设计应允许未来添加更多语言表达层：

```
未来状态（示例）：

┌─────────────────────────────────┐
│     Knowledge Meaning Layer     │
├─────────────────────────────────┤
│  Language Expression Layer      │
│  ┌────────┐ ┌──────┐ ┌──────┐  │
│  │ en     │ │zh-CN │ │ ja   │  │  ← 同一知识，三种语言表达
│  └────────┘ └──────┘ └──────┘  │
└─────────────────────────────────┘
```

### 扩展原则

| 原则 | 说明 |
|------|------|
| 知识独立于语言 | 概念、关系、心智模型在意义层定义，不绑定到特定语言 |
| 语言层可增量添加 | 新语言不需要重新构建知识结构，只需添加该语言的表达 |
| 系统层保持英文 | 标识符、命令、协议标识符不随语言层扩展而改变 |
| 入口文档本地化 | 每个新语言需要对应的 README 和入口文档 |

当前不需要实现多语言扩展。此节仅为架构前瞻，确保设计决策不会阻塞未来的多语言需求。

---

## 附录 A：与 project-state.md 的关系

本文件是 LLM Wiki OS 语言策略的**权威定义**。

`docs/project-state.md §9` 中的 Language Policy 部分为本文件的早期版本（3 层策略：System/Human Documentation/Knowledge）。本文件是对其的正式化和扩展，新增内容：

- 知识意义层与语言表达层的概念分离（§1）
- 双仓库社区策略（§2）
- 来源语言保留策略（§5）
- 未来多语言扩展架构（§6）

当两者存在差异时，以本文件为准。

---

## 附录 B：当前语言覆盖状态

| 组件 | English | 中文 |
|------|---------|------|
| README.md | 完整 | 完整（README.zh-CN.md） |
| FEDERATION.md | 完整 | — |
| CLAUDE.md | 完整 | — |
| protocol/ | 完整（7 个文件） | — |
| templates/ | 完整（10 个文件） | 概念模板含中文引导注释 |
| commands/ | 完整（5 个文件） | — |
| docs/ | 部分 | 中文为主的说明性文档 |
| Domain Wiki（AI） | — | 2 个概念页面（rag、ai-agent-memory） |
| Domain Wiki（KM） | — | 无页面 |
| Master Wiki | — | 无页面 |

---

*Language Policy v1.0.0 — 2026-07-12*

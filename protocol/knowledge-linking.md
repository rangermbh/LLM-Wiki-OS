---
created: 2026-07-11
updated: 2026-07-11
status: active
---

# Knowledge Linking Protocol（知识链接协议）

本协议定义 LLM Wiki OS 如何创建和维护与 Obsidian 兼容的 wiki 链接。链接是知识关系的结构化表达——每个链接都应指向一个存在的（或已规划的）概念页面。

---

## 1. Principles（原则）

### 链接代表知识关系

Wiki 链接不是装饰。每条 `[[link]]` 声明两个概念之间存在有意义的关系。如果关系不明确，不应创建链接。

### 避免知识重复

创建链接前，先搜索目标域中是否已有对应页面。优先链接已有概念，而非创建重复页面。

### 保留来源可追溯性

每个 Domain Wiki 页面必须通过 `sources` 字段和 `captured_from` 字段保留指向原始 capture 文件的链接。知识链不可断裂。

### 链接应是可解析的

每个链接在 Obsidian 中必须能够解析为实际文件。不可解析的裸链接会创建孤立笔记，污染 Vault 根目录。

---

## 2. Link Resolution Rules（链接解析规则）

在创建任何 `[[link]]` 之前，Agent 必须执行以下解析流程：

### Step 1: 搜索已有页面

```
在目标 Domain 的 wiki/ 目录中搜索目标概念。
搜索范围：spaces/<domain>/wiki/concepts/
          spaces/<domain>/wiki/methods/
          spaces/<domain>/wiki/technologies/
          spaces/<domain>/wiki/references/
          spaces/<domain>/wiki/entities/
```

### Step 2: 解析目标路径

| 搜索结果 | 处理方式 |
|----------|----------|
| 找到 1 个精确匹配 | 使用完整相对路径创建链接 |
| 找到多个匹配 | 选择最相关的，在 Notes 中标注其他候选 |
| 未找到 | 检查其他 Domain 是否有对应页面 |
| 完全不存在 | 按照 §4 Missing Concept Handling 处理 |

### Step 3: 使用显式路径链接

禁止在未验证目标是否存在的情况下创建裸链接。详见 §3。

---

## 3. Link Format（链接格式）

### 禁止的格式

```markdown
[[RAG]]
[[Vector Database]]
[[Transformer]]
```

**原因**：Obsidian 会在 Vault 根目录创建 `RAG.md`、`Vector Database.md` 等孤立文件，破坏项目结构。

### 要求的格式

**方式一：完整路径链接（推荐）**

```markdown
[[spaces/ai/wiki/concepts/rag|RAG]]
[[spaces/ai/wiki/technologies/vector-database|Vector Database]]
[[spaces/ai/wiki/concepts/transformer-architecture|Transformer]]
```

**方式二：短名称链接（Vault 搜索）**

使用不含路径的文件名，由 Obsidian 在整个 Vault 中搜索匹配：

```markdown
[[rag|RAG]]
[[vector-embeddings|Vector Embeddings]]
```

**重要约束**：短名称链接依赖 Obsidian 的 Vault 全局搜索——它不是相对路径。仅在目标文件**已确认存在**时才安全。若目标不存在，Obsidian 会在 Vault 根目录创建孤立文件。Agent 在使用此格式前必须已通过步骤 2e 的搜索确认目标存在。

**方式三：相对路径链接**（同 Domain 内跨类型链接时使用）

从 `spaces/ai/wiki/concepts/` 链接到 `spaces/ai/wiki/technologies/`：

```markdown
[[../technologies/vector-database|Vector Database]]
```

### 链接到 Capture 文件

```markdown
[[capture/inbox/filename|Display Name]]
```

来源链接使用完整路径，因为 capture 文件和 wiki 页面不在同一目录树下。

### 格式选择优先级

1. 同 Domain 同类型（如 concepts → concepts）：短名称链接 `[[target|Display]]`（仅当目标已确认存在；对应方式二）
2. 同 Domain 不同类型（如 concepts → technologies）：相对路径 `[[../technologies/target|Display]]`（对应方式三）
3. 跨 Domain 链接：完整路径 `[[spaces/<domain>/wiki/<type>/target|Display]]`
4. 链接到 Capture 层：完整路径 `[[capture/inbox/filename|Display]]`
5. 链接到 Master：完整路径 `[[spaces/master/wiki/<category>/target|Display]]`

---

## 4. Missing Concept Handling（缺失概念处理）

当目标概念尚不存在时，**不得创建裸链接或空根文件**。

### 选项 A: 创建计划概念条目

在 Domain 的 index.md 对应区域添加注释标记：

```markdown
<!-- planned: [[wiki/concepts/rag|RAG]] — 待后续 ingestion 创建 -->
```

### 选项 B: 添加 TODO 标记

在 wiki 页面 Notes 区域记录：

```markdown
## Notes

- TODO: 创建 [[spaces/ai/wiki/concepts/rag|RAG]] 概念页面（当前缺失）
```

### 选项 C: 暂不链接

如果概念关系不够强，不创建链接。等待未来的 ingestion 自然引入该概念。

### 禁止的操作

- 在 Vault 根目录创建空 `.md` 文件以满足链接解析
- 创建内容仅为 `# TODO` 的占位页面
- 使用不存在的裸 `[[Concept]]` 链接

---

## 5. Growth Boundary Classification（增长边界分类）

当目标概念尚不存在时，并非所有缺失概念具有同等的创建优先级。本节定义三种缺失概念类型及其创建权限。

### Type A — Capture-driven `(page planned)`

**定义**：外部 capture 已存在于 `capture/inbox/`。页面创建属于正常 `/ingest` 流程的一部分。`(page planned)` 标记表示 "capture 已存在但尚未处理"。

**创建权限**：Agent 可通过 `/ingest` 执行。

**判断条件**：
- `capture/inbox/` 中存在对应的原始采集文件
- 该 capture 以该概念为核心主题

### Type B — Network-required `(page required)`

**定义**：现有知识网络依赖该节点闭合解释链。缺失该节点导致已有页面的机制描述或关系描述不完整。

**创建权限**：Agent 提议，Human 批准。

**判断条件（必须同时满足）**：
1. **Mechanism incompleteness**：已有页面的 Mechanism 或 Relationships 节描述了一个不存在的节点作为依赖项
2. **Not merely referential**：该引用是结构性的（"X 是 Y 的前置条件 / 互补机制"），而非列举性的（"参见 X"、"延伸阅读 X"）

**弱形式**：Type B 存在弱形式——缺失节点不在理论理解层（离开它无法理解当前概念），而在实践闭合层（当前概念的机制链描述了需要该节点来完成的操作）。弱形式 Type B 仍需 Human 批准。

### Type C — Speculative Candidate `(page candidate)`

**定义**：Agent 根据领域知识判断相关，但无 external capture 且非现有网络必需。

**创建权限**：需要 Human 确认，或未来 external capture 触发。

**判断条件**：
- 无 `capture/inbox/` 中的对应原始采集文件
- 已有页面的机制和关系描述不依赖该节点
- 引用属于 "相关概念"、"延伸阅读" 或 "未来可能连接"

### Decision Matrix

| Signal | Type A | Type B | Type C |
|--------|--------|--------|--------|
| External capture in inbox? | Yes | Maybe | No |
| Existing page mechanism broken without it? | N/A | **Yes** | No |
| Referenced in ≥2 existing pages? | Maybe | Usually | Maybe |
| Agent internal knowledge only? | No | No | **Yes** |

### Marker to Type Mapping

| Marker | Type | Creation Authority |
|--------|------|--------------------|
| `(page planned)` | Type A — Capture-driven | Agent (`/ingest`) |
| `(page required)` | Type B — Network-required | Agent proposes, Human approves |
| `(page candidate)` | Type C — Speculative Candidate | Human confirmation or future capture |

---

## 6. Domain Boundary Rules

链接应尊重 Domain 结构。不同 Domain 的知识类型路径不同：

| Domain | Concepts | Methods | Technologies | References | Entities |
|--------|----------|---------|--------------|------------|----------|
| AI | `spaces/ai/wiki/concepts/` | `spaces/ai/wiki/methods/` | `spaces/ai/wiki/technologies/` | `spaces/ai/wiki/references/` | `spaces/ai/wiki/entities/` |
| Knowledge Management | `spaces/knowledge-management/wiki/concepts/` | `spaces/knowledge-management/wiki/methods/` | `spaces/knowledge-management/wiki/technologies/` | `spaces/knowledge-management/wiki/references/` | `spaces/knowledge-management/wiki/entities/` |
| Master | `spaces/master/wiki/models/` | `spaces/master/wiki/principles/` | `spaces/master/wiki/concepts/` | `spaces/master/wiki/frameworks/` | — |

### 跨 Domain 链接

当 AI Domain 的概念与 Knowledge Management Domain 的概念相关时，使用完整路径：

```markdown
AI → KM:
[[spaces/knowledge-management/wiki/concepts/personal-knowledge-management|PKM]]

KM → AI:
[[spaces/ai/wiki/concepts/ai-agent-memory|AI Agent Memory]]
```

跨 Domain 链接是 `/promote` 的信号——如果多个 Domain 之间存在密集链接，可能表示存在跨域模式需要提升至 Master。

---

## 7. Obsidian Integration（Obsidian 集成）

LLM Wiki OS 使用以下 Obsidian 功能进行知识导航：

| 功能 | 用途 |
|------|------|
| Wiki Links (`[[link]]`) | 页面间的显式知识关系 |
| Backlinks（反向链接） | 自动发现哪些页面引用了当前页面 |
| Graph View（图谱视图） | 可视化知识网络结构 |
| Link Autocomplete | 输入 `[[` 时自动补全已有页面路径 |

**Obsidian 负责可视化。Protocol 控制知识结构。**

Agent 创建的链接必须遵循本协议的格式规则。Obsidian 的自动补全和反向链接功能依赖正确的路径格式——裸链接会破坏这些功能。

### 未解析链接的后果

在 Obsidian 中，未解析链接（指向不存在文件的链接）显示为不同颜色，但：
- 它们仍出现在 Graph View 中（作为孤立节点）
- 点击未解析链接会**在 Vault 根目录创建新文件**
- 根目录的零散文件破坏项目结构，难以恢复

**预防优于修复。创建每个链接前验证目标是否存在。**

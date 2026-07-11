---
created: 2026-07-11
updated: 2026-07-11T23:00:00+08:00
status: active
---

# LLM Wiki OS Project State

## 1. Project Goal

**LLM Wiki OS** is a personal knowledge operating system. It organizes knowledge into a federation of semi-autonomous wikis, maintained by Claude Code, edited in Obsidian, and versioned by Git.

| Component | Role |
|-----------|------|
| Obsidian | Knowledge editing interface |
| Claude Code | Wiki Maintainer Agent |
| Git | Knowledge evolution history |
| Markdown | Knowledge storage format |

**Core idea**: "一主多辅" (One Master, Many Domains) — a Master Wiki for the personal world model, and Domain Wikis for specialized knowledge. Knowledge flows upward: Capture → Domain → Master. Only cross-domain patterns reach Master.

---

## 2. Architecture

### Three Layers

```
Master Wiki        spaces/master/       Personal world model
Domain Wikis       spaces/*/            Domain-specific knowledge
Capture Layer      capture/             All external inputs
```

### Master Wiki (`spaces/master/`)

Stores: personal world model, cross-domain abstractions, principles, mental models.

Four categories: models, principles, concepts, frameworks.

Master is NOT a knowledge index. It is NOT a summary of all domains. It only contains knowledge that transcends individual domains.

### Domain Wikis (`spaces/<domain>/`)

Current domains:

| Domain | Path | Scope |
|--------|------|-------|
| AI | `spaces/ai/` | ML, DL, NLP, LLMs, AI alignment, AI safety |
| Knowledge Management | `spaces/knowledge-management/` | PKM, Obsidian workflows, LLM-assisted knowledge, information organization |

Each domain has: `schema.md`, `index.md`, `log.md`, `raw/`, `wiki/` (with 5 knowledge types: concepts, methods, technologies, references, entities), `sources/`.

### Capture Layer (`capture/`)

Entry points: `inbox/` (raw markdown), `attachments/` (binary files).

Sources: Obsidian Web Clipper, manual markdown, PDF exports, AI conversations, other materials. Raw content is immutable.

---

## 3. Design Decisions

These decisions are settled. Do not reopen without explicit human request.

| # | Decision | Rationale |
|---|----------|-----------|
| D1 | Master is not a knowledge index | Master stores only cross-domain abstractions. Domain indices handle knowledge lookup. |
| D2 | Domain is specialized knowledge space | Each domain is self-contained with its own schema, index, and wiki structure. |
| D3 | No physical isolation between domains | All domains live under `spaces/` in a single Git repo. Federation is logical, not physical. |
| D4 | Git as evolution history | Every knowledge change is versioned. Commit types (capture/update/reflect/promote/maintenance) encode the nature of each change. |
| D5 | Human has final cognitive authority | Agent proposes, analyzes, and prepares. Human decides. Master Wiki changes always require human approval. |
| D6 | Raw content is never modified | Captures in `inbox/` are immutable sources of truth. Only frontmatter metadata may be updated. |
| D7 | Agent prefers update over create | Before creating a new wiki page, the agent must search for existing pages that can be updated instead. |
| D8 | 5 knowledge types per domain | concepts, methods, technologies, references, entities — each with its own template and wiki subdirectory. |

---

## 4. Current Implementation Status

### Completed

| Component | Detail |
|-----------|--------|
| Git repository | Initialized, 5 commits |
| Directory structure | 27 directories, all layers present |
| CLAUDE.md | Operating Constitution v2, 516 lines |
| FEDERATION.md | Members, governance, authority model |
| Protocol docs | 5 documents (federation, git convention, metadata schema, domain routing, knowledge lifecycle) |
| Templates | 10 templates (1 raw + 5 domain + 4 master) |
| Commands | 5 commands (ingest, update, lint, promote, reflect) |
| Domains | 2 domains (AI, Knowledge Management) |
| Master Wiki | 4 categories, schema, index, log |
| Obsidian integration | Vault configured, 14 core plugins enabled, wikilinks, frontmatter |
| .gitignore | Obsidian workspace, OS files, temp files excluded |
| .gitkeep files | 21 files protecting empty directories |

### In Progress

| Component | Detail |
|-----------|--------|
| Ingestion governance | Protocol docs created, `/ingest` updated. Not yet exercised with real content. |

### Not Yet Started

| Component | Detail |
|-----------|--------|
| First real knowledge ingestion | No captures in inbox, no wiki pages created |
| Health check automation | `/lint` and `/reflect` never run against content |
| Cross-domain promotion | `/promote` never executed (needs content in 2+ domains first) |

---

## 5. Current Phase

**Phase**: Knowledge Activation

The architecture is complete. The system is ready for real knowledge. Next steps:

1. Create first capture in `capture/inbox/`
2. Run `/ingest` end-to-end
3. Populate both domains with initial wiki pages
4. Run `/lint` on real content
5. Detect first cross-domain pattern → `/promote`

---

## 6. Git History

| Commit | Message |
|--------|---------|
| `2b975ce` | genesis: initialize LLM Wiki OS |
| `5cc0106` | upgrade: LLM Wiki Federation OS v1.0 |
| `6364b02` | maintenance: add knowledge-management domain |
| `6867a14` | maintenance: add knowledge ingestion readiness report |
| `6b268e1` | maintenance: add ingestion governance layer |

Branch: `master`. Remote: not configured.

---

## 7. Operating Principles

### Claude Code Role

Wiki Maintainer Agent. Responsibilities:
- Read and process captures into Domain Wiki pages
- Update and maintain existing wiki content
- Run quality checks (structural, knowledge, federation)
- Detect cross-domain patterns and propose Master Wiki entries
- Review system health and suggest improvements
- Never modify Master Wiki directly
- Never delete knowledge without human approval

### Human Role

Final cognitive authority. Responsibilities:
- Create captures from external sources
- Review and approve Domain Wiki pages created by agent
- Accept or reject Master Wiki proposals
- Make final decisions on architecture changes
- Define what counts as "understanding," not the agent

### Interaction Model

Agent analyzes → Agent suggests → Agent prepares → Human decides.

Agent never redefines the human's worldview automatically. When uncertain, the agent explains options, recommends an approach, and waits for approval.

---

## 8. Documentation Map（文档地图）

完整的项目文档导航见：

- [[docs/document-map|LLM Wiki OS 文档地图]]

该文档为人类和 Agent 提供结构化的文档索引，涵盖入口文件、协议层、模板层、采集层、知识层、验证历史和维护原则。

---

## 9. Language Policy（语言策略）

LLM Wiki OS 是双语系统（中英双语）。语言选择取决于内容所在层。

### System Layer（系统层）

**保持英文**。以下标识符不翻译：

| 类别 | 示例 |
|------|------|
| 文件名 | `CLAUDE.md`, `project-state.md`, `ai-agent-memory.md` |
| YAML keys | `status`, `source_type`, `captured_from`, `ingested_by`, `ingested_to` |
| 元数据值 | `raw`, `processed`, `seedling`, `growing`, `evergreen` |
| 命令 | `/ingest`, `/update`, `/lint`, `/promote`, `/reflect` |
| 协议标识符 | `RAW INPUT`, `INGESTED`, `CURATED`, `ABSTRACTED` |
| 知识类型 | `concepts`, `methods`, `technologies`, `references`, `entities` |
| Master 类别 | `models`, `principles`, `concepts`, `frameworks` |
| Git commit 类型 | `capture`, `update`, `reflect`, `promote`, `maintenance` |

**原因**：这些标识符是机器可读的系统契约。翻译会破坏 Agent 解析、模板匹配和 Git 日志聚合。

### Human Documentation Layer（人类文档层）

**中文为主**，关键术语保留英文并附中文说明。

示例格式：
- Knowledge Lifecycle（知识生命周期）
- Federation Protocol（联邦协议）
- Capture Layer（采集层）
- Domain Wiki（领域 Wiki）
- Master Wiki（主 Wiki）

**规则**：
- 首次出现的重要术语使用 `English（中文）` 格式
- 文件名和路径保留英文原文，不翻译
- 技术概念的解释使用中文

### Knowledge Layer（知识层）

**中文说明为主**，保留标准英文技术术语。

保留英文原词的技术术语示例：
- AI Agent
- RAG（Retrieval-Augmented Generation）
- Vector Database
- Embedding
- Memory Architecture
- Transformer
- LLM（Large Language Model）
- HNSW（Hierarchical Navigable Small World）
- k-NN（k-Nearest Neighbors）
- API

**规则**：
- 广为人知的英文缩写直接使用（RAG、LLM、API）
- 较生僻的缩写首次出现时给出全称
- Wiki 页面的 Definition（定义）部分应提供清晰的中文解释
- 技术准确性优先于语言纯粹性——当英文术语比中文翻译更精确时，保留英文

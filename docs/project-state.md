---
created: 2026-07-11
updated: 2026-07-12T23:59
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
| Protocol docs | 6 documents (federation, git convention, metadata schema, domain routing, knowledge lifecycle, knowledge distillation) |
| Templates | 10 templates (1 raw + 5 domain + 4 master) |
| Commands | 5 commands (ingest, update, lint, promote, reflect) |
| Domains | 2 domains (AI, Knowledge Management) |
| Master Wiki | 4 categories, schema, index, 2 concepts (activated) |
| Obsidian integration | Vault configured, 14 core plugins enabled, wikilinks, frontmatter |
| .gitignore | Obsidian workspace, OS files, temp files excluded |
| .gitkeep files | 21 files protecting empty directories |

### In Progress

| Component | Detail |
|-----------|--------|
| — | 无进行中项目。S06 已完成，等待 S07 方向确定。 |

### Completed Since Last Update

| Component | Detail |
|-----------|--------|
| S06 System Stabilization | Schema drift 修复（AI + KM）、12 条 Domain→Master backlinks、Master Wiki Governance 创建、首次结构 lint（0 broken links, 0 orphans）、document-map 同步 |
| S05 Reflect Report | `/reflect` 执行：6 项发现（3 medium, 3 low），系统健康确认，S06 readiness 验证 |

### Not Yet Started

| Component | Detail |
|-----------|--------|
| S07 Knowledge Expansion | 方向待 Human 确定：KM Domain Growth / AI Domain Deferred Activation / Third Domain |
| Health check automation | 首次 lint 已手动执行；自动化 lint 触发规则尚未建立 |

---

## 5. Current Phase

**Phase**: S06 Knowledge System Stabilization & Governance Baseline — Completed. S07 direction pending.

AI Domain: Phase 1 Frozen (4 concepts). KM Domain: Activated (2 concepts). Master Wiki: Activated (2 concepts + governance).

Domain→Master backlink 网络完整（12 条双向链路）。Master Wiki 治理框架已建立。首次结构 lint 通过（0 broken links, 0 orphans）。

Next milestone: S07 direction (待 Human 确定)。

Phase 演进：
1. ~~Architecture Building~~ ✓ (2026-07-11)
2. ~~Knowledge Activation~~ ✓ (2026-07-11, first ingest)
3. ~~Knowledge Quality Improvement~~ ✓ (2026-07-12, distillation layer + language policy)
4. ~~Knowledge Scaling~~ ✓ (2026-07-12, AI Domain v2 + KM Domain bootstrap)
5. ~~KM Domain Expansion~~ ✓ (2026-07-12, Zettelkasten Method + taxonomy stabilization)
6. ~~Cross-Domain Abstraction~~ ✓ (2026-07-12, /promote × 2 + Master Wiki Construction)
7. ~~Knowledge System Stabilization~~ ✓ (2026-07-12, schema drift fix + backlinks + governance + first lint)
8. **S07: (未定义)** ← 当前过渡点

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

语言策略的完整定义见：**`docs/language-policy.md`**（Language Policy v1.0）。

该文件取代本 section 此前的内容，新增了以下维度：

- 知识意义层与语言表达层的概念分离
- 双仓库社区策略（GitHub / Gitee）
- 来源语言保留策略
- 未来多语言扩展架构

核心原则不变：系统标识符保持英文（机器可读契约），知识页面中文为主（技术术语保留英文）。

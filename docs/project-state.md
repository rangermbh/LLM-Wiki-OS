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
| Claude Code | Wiki Maintainer Agent / Cognitive Companion |
| Git | Knowledge evolution history |
| Markdown | Knowledge storage format |

**Core idea**: "一主多辅" (One Master, Many Domains) — a Master Wiki for the personal world model, and Domain Wikis for specialized knowledge. Knowledge flows upward: Capture → Domain → Master. Only cross-domain patterns reach Master.

### Long-term Vision (S09 Strategic Redefinition)

**LLM Wiki OS is ultimately a Personal Intelligence Support System**, not merely a Knowledge Management System.

**Core value**: Help individuals connect world knowledge, personal experience, and continuous reflection to form better cognition and improve future decisions.

**Personal Intelligence Loop**:

```
Goal → External Knowledge → Understanding → Experience → Reflection → Cognition → Decision → Outcome → New Reflection
```

**Current coverage**: LLM Wiki OS already serves External Knowledge and Understanding. Future exploration focuses on: Knowledge → Reflection → Cognition.

**AI Role — Cognitive Companion**:
- Remember — surface relevant knowledge and past reflections
- Connect — find patterns across knowledge, experience, and decisions
- Challenge — question assumptions and identify blind spots
- Reflect — prompt deeper thinking about outcomes and their implications
- Support — provide cognitive infrastructure without taking ownership

**Human Authority** (non-negotiable):
- Goal Ownership — Human sets direction and priorities
- Cognition Ownership — Human does the thinking and synthesis
- Decision Ownership — Human makes the choices

**Boundaries** (what this system will NOT become):
- Personal Profile System
- Life Management OS
- Everything OS
- AI Decision Maker

**Principle**: Support Human Intelligence, Never Replace Human Cognition.

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
| D9 | Growth Boundary Classification | Three types for missing concept references: Type A — Capture-driven (page planned, Agent may execute), Type B — Network-required (page required, Agent proposes + Human approves), Type C — Speculative Candidate (page candidate, Human confirmation or future capture). Defined in knowledge-linking.md §5. |
| D10 | Strategic Redefinition: Personal Intelligence Support System | S09 Strategic Sync. LLM Wiki OS long-term goal redefined from Knowledge Management System to Personal Intelligence Support System. Core value: connect world knowledge, personal experience, and continuous reflection → better cognition → improved decisions. AI role: Cognitive Companion (Remember, Connect, Challenge, Reflect, Support). Human retains Goal/Cognition/Decision Ownership. Boundaries: NOT Profile System, Life OS, Everything OS, or AI Decision Maker. Principle: Support Human Intelligence, Never Replace Human Cognition. |

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
| — | *No active work. S09 Strategic Sync complete. Awaiting S10 direction.* |

### Completed Since Last Update

| Component | Detail |
|-----------|--------|
| S06 System Stabilization | Schema drift 修复（AI + KM）、12 条 Domain→Master backlinks、Master Wiki Governance 创建、首次结构 lint（0 broken links, 0 orphans）、document-map 同步 |
| S07 System Reorientation | Steps 1-3 complete: Emergent Organization + Growth Boundary Classification + MOC Curation Method + Closure. KM Domain Foundation Complete declared. |
| S08 Step 1 AI Domain Health Review | Full health review: topology (2-tier DAG, 4 nodes), concept maturity (3 growing + 1 seedling), growth boundary (freeze appropriate), candidate evaluation (0 Type A, 0 Type B, 9 Type C). Recommendation: Maintain Current Freeze — accepted. Report: `reports/S08-AI-Domain-Health-Review.md` |
| S08 Step 2 Governance Synchronization | project-state.md (Sections 4/5/6), session-snapshot.md (Sections 1/5/8), document-map.md (Section 7) synchronized. Section 6 vs 10 evaluation: remain separate. 10/10 consistency checks passed. Report: `reports/S08-Governance-Synchronization.md` |
| S08 Closure | Phase complete. S08 closed. Reports archived. |
| S09 Strategic Sync | Long-term vision redefined: Personal Intelligence Support System. AI role: Cognitive Companion. Strategic boundaries established. Design Decision D10 recorded. |

### Not Yet Started

| Component | Detail |
|-----------|--------|
| S08 Steps 3+ | 待 Human 决定 S08 后续步骤 |
| Digital Garden | Type C (speculative candidate)，需 Human 确认或 future capture 触发 |

---

## 5. Current Phase

**Phase**: S09 Strategic Sync — **Complete.** Long-term vision redefined: Personal Intelligence Support System. Strategic direction recorded. No implementation.

AI Domain: Phase 1 Frozen (4 concepts, Pull-based Growth). KM Domain: Foundation Complete (3 concepts + 1 method, Pull-based Growth). Master Wiki: Activated (2 concepts + governance).

Domain→Master backlink 网络完整（13 条双向链路：5 KM + 8 AI）。KM Domain Principle → Method → Result → Curation 推导链已闭合。Growth Boundary 三级分类 (Type A/B/C) 已建立。0 Type A, 0 Type B, 1 Type C (Digital Garden)。Lint 通过（0 broken links, 0 orphans）。S08 Step 1 健康审查通过：AI Domain 4 节点网络稳定，freeze 维持。

Next milestone: S10 方向确定（待 Human 决定）。Strategic foundation laid — future work explores Knowledge → Reflection → Cognition path.

Phase 演进：
1. ~~Architecture Building~~ ✓ (2026-07-11)
2. ~~Knowledge Activation~~ ✓ (2026-07-11, first ingest)
3. ~~Knowledge Quality Improvement~~ ✓ (2026-07-12, distillation layer + language policy)
4. ~~Knowledge Scaling~~ ✓ (2026-07-12, AI Domain v2 + KM Domain bootstrap)
5. ~~KM Domain Expansion~~ ✓ (2026-07-12, Zettelkasten Method + taxonomy stabilization)
6. ~~Cross-Domain Abstraction~~ ✓ (2026-07-12, /promote × 2 + Master Wiki Construction)
7. ~~Knowledge System Stabilization~~ ✓ (2026-07-12, schema drift fix + backlinks + governance + first lint)
8. **S07: System Reorientation** ✓ (2026-07-12, Steps 1-3 complete: Emergent Organization + Growth Boundary + MOC Integration + Closure。KM Domain Foundation Complete, Pull-based Growth)
9. **S08: AI Domain Health Review** ✓ (2026-07-12, Steps 1-3 complete: Health Review + Governance Synchronization + Closure。AI Domain freeze 维持，governance docs 同步)
10. **S09: Strategic Sync** ✓ (2026-07-12, Strategic Redefinition: Personal Intelligence Support System。Long-term vision recorded, no implementation)

---

## 6. Git History

| Commit | Message |
|--------|---------|
| `2b975ce` | genesis: initialize LLM Wiki OS |
| `5cc0106` | upgrade: LLM Wiki Federation OS v1.0 |
| `f13bf11` | update: ai - add AI Agent Memory concept (first ingestion) |
| `3b85702` | S06 complete: schema stabilization, master governance, and lint baseline |
| `c313759` | docs: record repository governance baseline |
| `d470085` | docs: establish knowledge growth boundary classification |
| `93bbed2` | update: add MOC curation method layer to KM Domain |
| `e2b1fcb` | maintenance: close S07 stabilization and declare KM foundation complete |
| `a2c9b07` | maintenance: record Structured Management Layer as deferred improvement (Type C) |

Branch: `master`. Remote: `origin` (GitHub). Mirror: Gitee (manual pull).
Last synchronized: 2026-07-12 (S08 Step 2)

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

---

## 10. Repository Governance

### Primary Repository

| 属性 | 值 |
|------|------|
| 平台 | GitHub |
| 仓库 | rangermbh/LLM-Wiki-OS |
| 角色 | Development source of truth |

### Mirror Repository

| 属性 | 值 |
|------|------|
| 平台 | Gitee |
| 仓库 | LLM-Wiki-OS |
| 角色 | Accessibility and backup mirror |

### Synchronization

| 属性 | 值 |
|------|------|
| 方向 | GitHub → Gitee |
| 模式 | Manual Pull Sync |

### Development Rule

**GitHub only development** — 所有开发活动（commit、branch、PR、review）仅在 GitHub 进行。

Gitee 限制：
- 无直接 commit
- 无 feature 开发
- 无分支管理

### Milestone Sync

在每个稳定里程碑完成后：

1. 在 GitHub 上创建 tag
2. 在 Gitee 上触发手动 Pull Sync

---

## 11. Deferred Improvements

已记录但当前不实施的改进候选项。按 S07 Growth Boundary Classification 标记类型。

| # | Item | Type | Description | Trigger Signal |
|---|------|------|-------------|----------------|
| I1 | Structured Management Layer Evaluation | Type C | 评估 Obsidian Database 等结构化能力作为 Wiki Graph Layer 的互补层。用于 Projects、Courses、Research papers、Tasks 等具有明确生命周期和状态的对象。Wiki Graph Layer 保留 concepts/methods/relationships，Structured Management Layer 管理 entities/status/lifecycle。两者互补，非替代。 | 高频项目管理需求 / 教学资源生命周期管理 / 科研对象状态跟踪 / Entity 管理需求增长 |

### I1 详细约束

- 禁止将 concepts/ 或 methods/ 转换为数据库表
- 禁止用 metadata table 替代 semantic graph
- 禁止因数据库能力引入而创建新 protocol
- 保持观察状态，等待触发信号

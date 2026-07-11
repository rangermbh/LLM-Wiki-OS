---
created: 2026-07-12
updated: 2026-07-12
status: active
---

# LLM Wiki OS Session Snapshot

## 1. Current Project Phase

**Knowledge Quality Improvement**

项目已完成架构搭建和首次知识摄取验证，当前阶段的核心关注点是：**提升 Domain Wiki 页面的知识质量**。

具体而言：当前 `/ingest` 生成的页面接近文章摘要（Article Summary），而非长期知识节点（Knowledge Concept）。正在引入 Knowledge Distillation 层来解决这个问题。

阶段演进路径：

```
Architecture Building  ✓  (2026-07-11)
Governance             ✓  (2026-07-11)
Knowledge Activation   ✓  (2026-07-11, first ingest)
Knowledge Quality Improvement  ← 当前阶段
Knowledge Scaling      (future)
Cross-Domain Abstraction  (future)
```

---

## 2. Project Context

### 项目目标

LLM Wiki OS 是一个个人知识操作系统。它将知识组织为半自治 Wiki 的 Federation，由 Claude Code 维护，在 Obsidian 中编辑，由 Git 版本控制。

核心理念："一主多辅"——一个 Master Wiki 存储个人世界模型，多个 Domain Wiki 存储领域知识。知识向上流动：Capture → Domain → Master。只有跨领域模式才能到达 Master。

### 三层架构

| 层 | 位置 | 职责 |
|----|------|------|
| Capture Layer | `capture/` | 所有外部输入的临时着陆区。原始内容不可变。 |
| Domain Wiki | `spaces/{domain}/` | 领域特定知识。自包含，有自己的 schema、index、log 和 5 种知识类型。 |
| Master Wiki | `spaces/master/` | 跨领域个人世界模型。仅存储原理、心智模型、跨领域概念和框架。 |

### Federation 理念

- 每个 Domain 是半自治的知识空间
- Master 不是知识索引——只存储跨领域抽象
- 知识向上流动，Agent 权限向上递减
- 人类拥有最终认知权威

### Agent 与 Human 权限边界

| 操作 | Human | Agent |
|------|-------|-------|
| 创建 Domain 页面 | 批准 | 执行 |
| 编辑 Domain 页面 | 批准 | 执行 |
| 删除 Domain 页面 | 批准 | 提议 |
| 创建 Master 页面 | **必须批准** | 仅提议 |
| 编辑 Master 页面 | **必须批准** | 仅提议 |
| 修改架构 | **必须批准** | 仅提议 |
| 修改 capture 原始内容 | **禁止** | **禁止** |

---

## 3. Completed Milestones

### 架构与治理（Architecture & Governance）

- [x] Federation 架构（三层模型）
- [x] CLAUDE.md（Agent 操作宪法，516 行）
- [x] FEDERATION.md（成员、治理、权限模型）
- [x] 双语文档（README.md + README.zh-CN.md）
- [x] Git 提交规范（5 种 commit 类型）

### 协议层（Protocol Layer）

- [x] `LLM-Wiki-Federation-Protocol.md` — Federation 协议核心
- [x] `metadata-schema.md` — 元数据规范
- [x] `domain-routing.md` — 领域路由规则
- [x] `knowledge-lifecycle.md` — 知识生命周期（微观状态转换）
- [x] `knowledge-linking.md` — 知识链接协议（wikilink 治理）
- [x] `knowledge-flow.md` — 知识流动协议（宏观流动路径）
- [x] `Git-Commit-Convention.md` — Git 提交规范
- [x] `knowledge-distillation.md` — 知识蒸馏协议（v1.0.0，最新）

### 命令系统（Command System）

- [x] `/ingest` — Process Capture Inbox（含 Distillation 阶段）
- [x] `/update` — Update Existing Wiki Pages
- [x] `/lint` — Knowledge Quality Check
- [x] `/promote` — Propose Master Wiki Content
- [x] `/reflect` — Review and Suggest Improvements

### 领域与知识（Domains & Knowledge）

- [x] AI Domain（schema、index、log、wiki 结构完整）
- [x] Knowledge Management Domain（schema、index、log、wiki 结构完整）
- [x] Master Wiki（4 种类型：models、principles、concepts、frameworks）
- [x] 首次真实知识摄取：AI Agent Memory 概念页面
- [x] 第二次摄取：RAG 概念页面
- [x] `/update` 验证：种子页面 → 成长页面转换、wikilink 修复
- [x] `/ingest` 管道端到端验证

### 基础设施（Infrastructure）

- [x] Obsidian Vault 集成（14 个核心插件）
- [x] 模板系统（10 个模板：1 raw + 5 domain + 4 master）
- [x] Web Clipper 集成
- [x] `.gitignore`（Obsidian 工作区、OS 文件、临时文件）
- [x] `.gitkeep` 文件保护空目录

---

## 4. Current System Validation

### 已验证流程

```
外部知识（Web Clip / 手动）
        ↓
capture/inbox/  (status: raw)
        ↓
/ingest
  ├── Routing     → 领域路由（domain-routing.md）
  ├── Distillation → 知识蒸馏（knowledge-distillation.md）
  └── Creation    → Wiki 页面创建（domain-concept-template.md）
        ↓
Domain Wiki 页面  (status: seedling)
        ↓
/update
  ├── wikilink 验证（knowledge-linking.md）
  ├── 成熟度评估（knowledge-lifecycle.md）
  └── 内容刷新
        ↓
status: growing → evergreen (需人类批准)
```

### 测试案例

#### 案例 1: AI Agent Memory

| 维度 | 状态 | 详情 |
|------|------|------|
| 来源 | Redis Blog (2026) | `capture/inbox/AI agent memory types, architecture & implementation.md` |
| 路由 | AI Domain / concept | 正确 — AI Agent 记忆架构属于 AI 领域 |
| 初始状态 | seedling | `/ingest` 创建，含完整模板结构 |
| 首次 /update | growing | 修复 5 个裸 wikilink，添加 planned 注释，满足 growth 条件 |
| 当前状态 | growing | 含到 rag.md 的实时 wikilink |
| wikilink 健康 | 1 个 live link（→ rag.md），4 个 planned | 无裸链接、无孤儿文件 |

#### 案例 2: RAG（检索增强生成）

| 维度 | 状态 | 详情 |
|------|------|------|
| 来源 | AWS Docs (2026) | `capture/inbox/什么是 RAG？— 检索增强生成 AI 详解.md`（中文） |
| 路由 | AI Domain / concept | 正确 — 文章解释 RAG 架构本身（非 PKM 应用），Primary Intent → AI |
| 首次 /ingest | seedling | 创建为文章摘要风格页面 |
| 蒸馏重写 | seedling（内容更新） | 按 knowledge-distillation.md 标准重写，中文为主，含完整 Limitations |
| wikilink 健康 | 1 个 live link（→ ai-agent-memory），2 个 planned | 无裸链接 |

### 验证结论

- **Routing**: 正常 — 两个案例的领域路由和知识类型选择均正确
- **Lifecycle**: 正常 — raw → processed → seedling → growing 状态转换正确执行
- **Wikilink Governance**: 正常 — 裸链接被检测并修复，跨层链接完整路径正确
- **Distillation**: 首次应用 — RAG 页面从文章摘要重写为知识概念，新标准已验证

---

## 5. Current Discussion Context

### 当前重点：Knowledge Quality Governance

#### 现状

Domain Wiki 页面质量偏 Article Summary。首个 `/ingest` 生成的页面（ai-agent-memory、rag v1）明显保留了来源文章的结构和措辞。

#### 问题诊断

| 症状 | 根因 |
|------|------|
| 页面结构跟随来源文章顺序 | `/ingest` 缺少知识提炼阶段 |
| 营销语言/背景信息残留 | 没有明确的"蒸馏规则"指导 Agent |
| 中文来源→英文页面信息损失 | 双语策略在知识层尚未统一 |
| Limitations 区域缺失 | 旧模板未包含此区域 |

#### 已实施修复（2026-07-12）

1. **新增 `protocol/knowledge-distillation.md`** — 定义 Article Summary vs Knowledge Concept 的 7 个维度区分、8 个必需区域、蒸馏规则和质量检查清单
2. **修改 `/ingest` 命令** — 管道增加 Distillation 阶段：Capture → Routing → Distillation → Concept Creation
3. **重构 Concept 模板** — 新结构：Definition → Core Idea → Mechanism → Relationships → Applications → Limitations → Sources → Notes
4. **重写 rag.md** — 作为新标准的首次应用，中文为主，完整覆盖所有区域

#### 当前讨论

引入 Knowledge Distillation 层后的下一步：

- ~~是否需要重写 ai-agent-memory.md 以匹配新标准？~~ ✓ 已于 2026-07-12 完成迁移
- ~~中文知识输出策略：AI Domain 是否全面转向中文为主？~~ ✓ 已由 `docs/language-policy.md` v1.0 定义
- 其他 domain template（method、technology、reference、entity）是否需要类似的蒸馏标准？
- 蒸馏质量如何自动化验证（`/lint` 是否应包含蒸馏质量检查）？

---

## 6. Pending Decisions

| # | 问题 | 状态 | 上下文 |
|---|------|------|--------|
| D1 | ai-agent-memory.md 是否按新标准重写 | ✅ 已解决 | 2026-07-12 按 knowledge-distillation.md 迁移为中文知识概念，8-section 结构 |
| D2 | AI Domain 知识语言策略 | ✅ 已解决 | `docs/language-policy.md` v1.0 定义：知识层中文为主，技术术语保留英文 |
| D3 | 其他 domain template 的蒸馏标准 | 待决定 | 当前仅 concept template 更新。method/technology/reference/entity 是否有类似需求？ |
| D4 | `/lint` 蒸馏质量检查 | 待决定 | 是否在 `/lint` 中增加 Article Summary 检测规则？ |
| D5 | Master Wiki 首次抽象提案 | 待决定 | 当 AI Domain ≥3 个概念页面后，是否需要首次 `/promote`？ |
| D6 | Knowledge Management Domain 首次内容 | 待决定 | KM Domain 目前无 Wiki 页面，需要合适的采集文件 |

---

## 7. System Constraints

以下原则不可违反，任何变更必须在这些约束内进行：

### 数据完整性

- **Raw Layer immutable** — `capture/inbox/` 原始内容永不可修改。仅可更新 frontmatter 元数据。
- **不可删除** — 知识页面的删除需要人类确认。优先归档（→ `archive/`）而非删除。

### 权限边界

- **Agent 不可直接修改 Master Wiki** — Master 变更必须通过 `/promote` 提案 + 人类批准。
- **Agent 权限向上递减** — Capture → Domain（执行权）→ Master（仅提案权）。
- **人类拥有最终认知权威** — Agent 提议、分析、准备；人类决定。

### 知识结构

- **禁止裸 wikilink** — `[[RAG]]` 等格式会创建 Vault 根目录孤儿文件。必须使用完整路径或已验证的短名称。
- **禁止空占位页面** — 不创建内容仅为 `# TODO` 或为满足链接解析的空页面。
- **优先更新而非创建** — 搜索已有页面，避免知识重复。

### 架构

- **不跳过知识生命周期阶段** — Capture → Domain → Cross-domain Pattern → Master Model，不跳跃。
- **Master 不是知识索引** — Master 仅存储跨领域抽象，不复制 Domain 内容。

---

## 8. Next Recommended Actions

基于当前项目状态，推荐以下步骤（按优先级排列）：

### 优先级 1: 统一 AI Domain 知识页面质量

1. **重写 ai-agent-memory.md** — 按 knowledge-distillation.md 标准，中文为主，补充 Limitations 等缺失区域
2. **决策 AI Domain 语言策略** — 中文为主 or 保持英文？统一后写入 project-state.md Language Policy

### 优先级 2: 扩展知识覆盖

3. **处理 KM Domain 首次采集** — 寻找或创建 Knowledge Management 领域的采集文件，运行 `/ingest`
4. **扩展 AI Domain 概念** — Vector Embeddings、Transformer Architecture 是 RAG 和 Agent Memory 的共同依赖，创建这些概念页面可形成密集知识网络

### 优先级 3: 质量系统完善

5. **运行 `/lint` 验证** — 对已有 2 个概念页面执行质量检查，验证 structural/knowledge/federation 三个维度的健康度

### 后续（不紧急）

6. 跨领域模式检测 → 首次 `/promote`（需要 AI Domain ≥3 页面 + KM Domain ≥1 页面）
7. `/reflect` 系统健康审查（每周）
8. 其他 domain template 蒸馏标准扩展

---

## 9. Maintenance Rule

**此文件不是项目规范文档。**

用途：
- 保存当前讨论上下文
- 帮助新会话快速恢复项目状态
- 记录阶段性决策背景和未决问题

**长期规范**仍以以下文件为准：
- `README.md` / `README.zh-CN.md` — 项目概述
- `docs/project-state.md` — 项目状态和设计决策（D1-D8）
- `protocol/` — 系统规则和协议
- `CLAUDE.md` — Agent 操作宪法
- `FEDERATION.md` — Federation 治理

此文件应在每次重要讨论或决策后更新，保持与项目实际状态同步。

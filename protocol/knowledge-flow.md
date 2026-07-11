---
created: 2026-07-11
updated: 2026-07-11
status: active
---

# Knowledge Flow Protocol（知识流动协议）

本协议定义 LLM Wiki Federation 中知识在各层之间的完整流动路径。它是对 `knowledge-lifecycle.md`（微观状态转换）、`domain-routing.md`（领域路由）、`knowledge-linking.md`（链接治理）和 `FEDERATION.md`（权限模型）的综合——描述知识**如何**以及**何时**在系统中移动。

---

## 1. Flow Architecture（流动架构）

```
                         ┌─────────────────────┐
                         │    External World    │
                         │  (Web, books, AI,    │
                         │   conversations...)  │
                         └──────────┬──────────┘
                                    │ human captures
                                    ▼
┌──────────────────────────────────────────────────────────────┐
│                     CAPTURE LAYER                            │
│                                                              │
│   capture/inbox/                                             │
│   ┌────────┐     /ingest      ┌────────────┐                │
│   │  raw   │ ───────────────► │ processed  │                │
│   └────────┘                  └────────────┘                │
│       │                             │                        │
│       │ content extraction          │ traceability           │
│       ▼                             ▼                        │
│   immutable record              captured_from               │
│                                 ingested_to                 │
└──────────────────────────────────────────────────────────────┘
                                    │
                                    │ /ingest
                                    ▼
┌──────────────────────────────────────────────────────────────┐
│                     DOMAIN LAYER                             │
│                                                              │
│   spaces/<domain>/wiki/<type>/                               │
│   ┌──────────┐   /update    ┌─────────┐   human    ┌────────┐│
│   │ seedling │ ───────────► │ growing │ ─────────► │evergreen│
│   └──────────┘              └─────────┘            └────────┘│
│       │                          │                      │    │
│       │ new knowledge            │ refinement           │    │
│       ▼                          ▼                      ▼    │
│   basic structure          comprehensive            stable   │
│                                                              │
│   Cross-domain signals (3+ pages, 2+ domains):               │
│       └──────────────────────┬──────────────────────────┘    │
│                              │ /promote                      │
└──────────────────────────────┼──────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     MASTER LAYER                             │
│                                                              │
│   spaces/master/wiki/<category>/                             │
│   ┌──────────┐   human     ┌──────────┐   human   ┌────────┐│
│   │ proposed │ ──────────► │ accepted │ ────────► │ active ││
│   └──────────┘             └──────────┘           └────────┘│
│       │                          │                      │    │
│       │ agent drafts             │ human approves       │    │
│       ▼                          ▼                      ▼    │
│   ┌──────────┐              integrated              fully    │
│   │ rejected │              into model              active   │
│   └──────────┘                                              │
│   (kept as record)                                           │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. Flow Stages（流动阶段）

### Stage 1: Capture（采集）

| 属性 | 说明 |
|------|------|
| 触发 | 人类将文件放入 `capture/inbox/` |
| 输入 | 外部知识（Web Clip、手动笔记、PDF 导出、AI 对话等） |
| 输出 | `capture/inbox/*.md`，`status: raw` |
| 权限 | 人类创建；Agent 不修改原始内容 |
| 协议 | `templates/raw-template.md` 定义 frontmatter 格式 |

原始采集文件是不可变的——它们是系统的"事实来源"。Agent 仅可在 frontmatter 中添加处理状态和追溯链接。

### Stage 2: Ingestion（摄取）

| 属性 | 说明 |
|------|------|
| 触发 | 人类运行 `/ingest` |
| 输入 | `capture/inbox/*.md` with `status: raw` |
| 输出 | Domain Wiki 页面（`status: seedling`） |
| 权限 | Agent 执行；人类批准领域路由决策 |
| 协议 | `protocol/domain-routing.md`（领域选择）<br>`protocol/knowledge-linking.md`（链接解析）<br>`protocol/knowledge-lifecycle.md`（状态：raw → processed） |

摄取是知识进入结构化系统的**第一个质量门**。Agent 必须：
1. 路由到正确的领域
2. 选择正确的知识类型
3. 搜索已有页面（优先更新，避免重复）
4. 解析所有 wikilink（禁止裸链接）
5. 创建 wiki 页面
6. 更新领域索引和日志
7. 标记采集为已处理

### Stage 3: Curation（策展）

| 属性 | 说明 |
|------|------|
| 触发 | 人类运行 `/update` 或定期维护 |
| 输入 | 现有的 Domain Wiki 页面 |
| 输出 | 改进的 Domain Wiki 页面（`seedling → growing → evergreen`） |
| 权限 | Agent 执行编辑；人类批准 `growing → evergreen` 转换 |
| 协议 | `protocol/knowledge-lifecycle.md`（状态转换条件）<br>`protocol/knowledge-linking.md`（链接验证） |

策展是知识逐渐成熟的过程。每次 `/update` 都会评估：
- 内容完整性（是否仍有注释占位符？）
- 链接健康度（是否有裸链接、断开链接或缺失链接？）
- 成熟度（是否满足下一状态的准入条件？）
- 新鲜度（来源是否已过时？）

### Stage 4: Abstraction（抽象）

| 属性 | 说明 |
|------|------|
| 触发 | Agent 检测到跨领域模式（≥3 个页面，≥2 个领域） |
| 输入 | 来自多个领域的 Domain Wiki 页面 |
| 输出 | Master Wiki 提案（`status: proposed`） |
| 权限 | Agent 仅可提议；人类必须批准 |
| 协议 | `.claude/commands/promote.md`（提案流程）<br>`FEDERATION.md` 权限模型 |

这是 Federation 中**权限最高的门**。Agent 永远不能直接将内容写入 Master Wiki——它必须起草一份 Promotion Proposal 并等待人类批准。Master 不是知识索引；它仅包含跨领域抽象。

### Stage 5: Integration（整合）

| 属性 | 说明 |
|------|------|
| 触发 | 人类批准 Master 提案 |
| 输入 | Master 页面（`status: proposed`） |
| 输出 | Master 页面（`status: accepted → active`） |
| 权限 | 仅人类可操作 |
| 协议 | `protocol/knowledge-lifecycle.md`（Master 状态转换） |

从 `accepted` 到 `active` 的转换代表真正的整合——该概念已融入用户的个人世界模型，并积极影响跨领域思维和决策。

---

## 3. Commands as Flow Operators（命令作为流动操作符）

每个命令在知识流动中扮演特定角色：

```
                     /ingest          /update           /promote
                        │                │                  │
   CAPTURE ─────────────┤                │                  │
                        ▼                ▼                  │
                     DOMAIN ───────── DOMAIN ───────────────┤
                     (create)       (refine)                │
                                                            ▼
                                                         MASTER
                                                         (propose)
                                                            │
                        /lint               /reflect         │
                          │                     │            │
                          ▼                     ▼            ▼
                      ALL LAYERS           ALL LAYERS    HUMAN DECISION
                      (validate)           (review)      (accept/reject)
```

| 命令 | 流动方向 | 知识状态变化 | 频率 |
|------|----------|-------------|------|
| `/ingest` | Capture → Domain | `raw` → `processed`（采集）<br>新建 `seedling`（Domain） | 按需（新采集到达时） |
| `/update` | Domain 内部 | `seedling` → `growing`<br>内容更新、链接修复 | 按需或定期 |
| `/lint` | 全层（只读） | 无状态变化；生成健康报告 | 按需 |
| `/promote` | Domain → Master | Master 页面设为 `proposed` | 按模式触发 |
| `/reflect` | 全层（审查） | 可能触发 `/update` 或 `/promote` | 每周 |

---

## 4. Decision Points（决策点）

知识流动路径上的每个关键决策点都有明确的权限边界：

```
[Capture arrives]
      │
      ▼
 ╔═══════════════════╗
 ║ Route to domain?  ║  ← Agent (domain-routing.md)
 ╚═══════╤═══════════╝
          │ unclear? → Ask human
          ▼
 ╔═══════════════════╗
 ║ Create or update? ║  ← Agent (search first, prefer update)
 ╚═══════╤═══════════╝
          │
          ▼
 ╔═══════════════════╗
 ║ Resolve wikilinks ║  ← Agent (knowledge-linking.md)
 ╚═══════╤═══════════╝
          │
          ▼
   [Domain page created/updated]
          │
          ▼
 ╔═══════════════════╗
 ║ seedling→growing? ║  ← Agent OR Human
 ╚═══════╤═══════════╝
          │
          ▼
 ╔═══════════════════╗
 ║ growing→evergreen?║  ← Human only
 ╚═══════╤═══════════╝
          │
          ▼
 ╔═══════════════════╗
 ║ Promote to Master?║  ← Agent proposes, Human decides
 ╚═══════╤═══════════╝
          │
          ▼
 ╔═══════════════════╗
 ║ accepted → active?║  ← Human only
 ╚═══════════════════╝
```

关键原则：**Agent 权限随着知识向上流动而递减**。Agent 对 Capture → Domain 的摄取拥有完全执行权。在 Domain 内部拥有共同权限。对 Domain → Master 仅拥有提案权。对 Master 内部状态变更没有权限。

---

## 5. Quality Gates（质量门）

每个流动阶段都有准入条件：

### Gate 1: Capture → Domain（摄取门）

- [ ] 采集有完整的 frontmatter（`source`、`source_type`、`created`）
- [ ] Agent 已搜索已有页面，未找到重复内容
- [ ] 领域路由已确定且记录在案
- [ ] 所有 wikilink 已解析（禁止裸链接）
- [ ] 页面至少包含：定义、关键点、来源引用

### Gate 2: seedling → growing（成长门）

- [ ] 所有模板区域已填充（无注释占位符）
- [ ] 至少有一个来自索引或其他页面的入链
- [ ] 来源采集已引用
- [ ] 内容已至少审查一次

### Gate 3: growing → evergreen（稳定门）

- [ ] 内容已稳定 ≥ 3 个月
- [ ] 无开放的 TODO 或未解答问题
- [ ] 多条出入链（>3）
- [ ] 与实际使用进行了验证
- [ ] **需要人类批准**

### Gate 4: Domain → Master（抽象门）

- [ ] 跨领域模式已识别（≥3 个源页面，≥2 个领域）
- [ ] 模式已清晰阐述
- [ ] 回链至所有源 Domain 页面
- [ ] **需要人类批准**

---

## 6. Feedback Loops（反馈回路）

知识并非仅向上流动。以下回路确保下游知识为上游知识提供信息：

### Top-Down: Master Informs Domain

当 Master 概念变为 `active` 后，领域页面可以通过链接引用它：

```markdown
详见：[[spaces/master/wiki/models/emergence|Emergence]]
```

Master 概念可以作为领域特定知识的高级框架。Agent 在 `/update` 运行期间可以建议此类连接。

### Lateral: Cross-Domain Links

不同领域的页面可以通过显式路径相互引用：

```markdown
[[spaces/ai/wiki/concepts/ai-agent-memory|AI Agent Memory]]
```

跨领域链接密度是 Agent 在 `/reflect` 期间跟踪的信号——密集集群可能预示着一个新兴的跨领域概念适合进行 `/promote`。

### Corrective: Master Contradiction

如果 Domain 页面与 Master 概念相矛盾（`FEDERATION.md` → Federation Signals），Agent 将标记以供人类审查。这会导致：
1. Domain 页面需要更新（新证据修正了理解）
2. Master 概念需要完善（Domain 知识揭示了局限性）

---

## 7. Flow Integrity Rules（流动完整性规则）

### 可追溯性链

每条知识必须保持指向其来源的完整追溯链：

```
Master 页面
  └── sources: 指向 Domain 页面
       └── captured_from: 指向 Capture 文件
            └── source: 指向外部 URL 或来源
```

链条中的任何一环断裂都会使知识无法验证。Agent 必须在 `/lint` 期间检查追溯完整性。

### 不可变性边界

```
采集层：      原始内容不可变
Domain 层：   内容可编辑（经批准）
Master 层：   内容仅可由人类编辑
```

### 状态转换完整性

每次状态变更必须记录在：
1. 页面的 `updated` frontmatter 字段
2. 领域的 `log.md`
3. Git 提交日志（适当类型）

---

## 附录：快速参考

| 我想理解... | 查阅 |
|-------------|------|
| 知识在微观层面如何变化 | `protocol/knowledge-lifecycle.md` |
| 采集被路由到哪个领域 | `protocol/domain-routing.md` |
| wikilink 应如何格式化 | `protocol/knowledge-linking.md` |
| 谁被授权做什么 | `FEDERATION.md` 权限模型 |
| Agent 如何执行摄取 | `.claude/commands/ingest.md` |
| Agent 如何提议 Master 内容 | `.claude/commands/promote.md` |
| 系统当前状态 | `docs/project-state.md` |

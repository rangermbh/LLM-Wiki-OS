---
created: 2026-07-12
updated: 2026-07-12T23:00
status: active
---

# LLM Wiki OS Session Snapshot

## 1. Current Project Phase

**S07 System Reorientation — Complete**

S07 全部三步已完成：

- Step 1: Emergent Organization concept 创建 — KM Domain Principle → Method → Result 推导链建立
- Step 1.5: Growth Boundary Classification — Type A/B/C 三级分类体系建立
- Step 2: MOC Integration & Curation Layer — KM Domain 首个 methods/ 页面创建，Principle → Method → Result → Curation 推导链闭合
- Step 3: Closure & Stabilization — Schema 修复，KM Domain Foundation Complete 声明

KM Domain 状态：**Foundation Complete, Pull-based Growth**（4 pages: 3 concepts + 1 method）

系统三层完整状态：

```
Master Wiki:   Activated (2 concepts + governance — 治理框架已建立)
Domain Wikis:  AI (4 concepts, Phase 1 Frozen) + KM (3 concepts + 1 method, Foundation Complete, Pull-based Growth)
Capture Layer: 4 captures processed, inbox clean
```

阶段演进路径：

```
Architecture Building           ✓  (2026-07-11)
Governance                      ✓  (2026-07-11)
Knowledge Activation            ✓  (2026-07-11)
Knowledge Quality Improvement   ✓  (2026-07-12)
Knowledge Scaling               ✓  (2026-07-12)
Cross-Domain Abstraction        ✓  (S05, 2026-07-12)
System Stabilization            ✓  (S06, 2026-07-12)
System Reorientation            ✓  (S07, 2026-07-12, Steps 1-3 complete)
  ├── Emergent Organization     ✓  concepts/ 第三概念页
  ├── Growth Boundary Class.    ✓  Type A/B/C 三级分类建立
  ├── Map of Content (MOC)      ✓  methods/ 首个页面, Type B→live
  └── Closure & Stabilization   ✓  Schema 修复, Foundation Complete 声明
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

### 治理文档（Governance Documents）

- [x] `docs/language-policy.md` — 语言策略 v1.0（中英双语治理、知识意义与语言表达分离）

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
- [x] Knowledge Distillation 层：协议 + 模板 + 命令管道 + 首次迁移
- [x] AI Domain Knowledge Network v2：4 个概念节点（Transformer → Vector Embeddings → RAG, Agent Memory）
- [x] AI Domain Phase 1 Freeze：网络稳定审查通过，pull-based growth rule 生效，进入 maintenance / expansion-ready 模式
- [x] KM Domain Bootstrap：Note Atomicity 作为首个锚点概念创建（principle-type）。Domain 从空域激活，知识生命周期首次在第二领域验证
- [x] KM Domain Expansion：Zettelkasten Method 创建（method-type concept）。KM Domain 2 concepts，Principle → Method 推导链建立。知识分类稳定性分析完成（5 种知识类型的认知姿态：理解/执行/使用/引用/识别）
- [x] KM Taxonomy Stabilization：5 个 wiki 目录的语义边界定义。Zettelkasten Method 分类确认（concepts/ — 理解姿态，非 methods/ — 执行姿态）。模板结构分析确认分类一致性
- [x] 第二次 `/promote`：正式跨领域模式检测。2 个 Candidate Pattern 均通过四维评估（Structural Isomorphism / Evidence Independence / Abstraction Stability / Master Boundary）。Pattern B 依赖 Pattern A 的前置关系确认
- [x] **Master Wiki Construction（首批）**：2 个 Master Concept 创建（structured-minimal-unit-principle + emergent-structure-from-local-interactions），6 个 Domain source pages 跨 AI + KM 双域验证。Master Wiki 从空壳进入 Activated 状态

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
| wikilink 健康 | 3 个 live link（→ ai-agent-memory, → vector-embeddings, → transformer-architecture） | 无裸链接、无 orphan |
| 2026-07-12 /update | seedling → growing | 满足全部 3 个提升条件：8 section 完整、4 incoming links（index + 3 concepts）、source 可追溯 |

#### 案例 3: Vector Embeddings（向量嵌入）

| 维度 | 状态 | 详情 |
|------|------|------|
| 来源 | 系统综合创建 | 作为 RAG 和 Agent Memory 的共同前置概念，由系统基于已验证知识创建 |
| 路由 | AI Domain / concept | 正确 — 向量嵌入是 AI 领域的基础表示技术 |
| 初始状态 | seedling | 按 knowledge-distillation.md 标准创建，8-section 结构完整 |
| wikilink 健康 | 3 个 live link（→ rag, → ai-agent-memory, → transformer-architecture） | 无裸链接、无 orphan |
| 网络影响 | 激活 2 个 planned references | rag.md 和 ai-agent-memory.md 中的 planned 链接转为 live |
| 2026-07-12 /update | seedling → growing | 满足 2 个文字条件（8 section 完整、4 incoming links）；source capture 条件不适用（系统综合页面）。网络集成度 + 多概念依赖替代 capture 可追溯性 |

### 知识网络结构（AI Domain v2）

```
                          ┌──────────────────────────┐
                          │  Transformer Architecture │
                          │  (架构基础层 — 自注意力计算)  │
                          └────────────┬─────────────┘
                                       │ Encoder → 上下文感知嵌入
                                       │ Decoder → 自回归生成
                        ┌──────────────┴──────────────┐
                        ▼                             ▼
              ┌─────────────────────┐   ┌─────────────────────────┐
              │  Vector Embeddings  │   │   RAG + Agent Memory    │
              │  (语义表示层)         │   │  (应用管道层 — 共享四阶段)  │
              └──────────┬──────────┘   └────────────┬────────────┘
                         │                           │
                         │  编码阶段核心技术            │  生成/整合阶段推理引擎
                         └───────────┬───────────────┘
                                     │ 共享 Transformer 基础设施
                                     │ Encoder → Embedding, Decoder → Generation
```

### Master Wiki Validation（2026-07-12）

首批 Master Concepts 创建后的验证状态：

| 维度 | 状态 | 详情 |
|------|------|------|
| Source pages | 6/6 linked | 4 AI + 2 KM，全部通过完整路径 wikilink 指向 Domain source pages |
| Cross-domain evidence | 2 domains | AI + KM 独立验证，无共同理论传承 |
| Master→Master relationship | 双向 live | structured-minimal-unit-principle ↔ emergent-structure-from-local-interactions，前置依赖关系一致 |
| Domain→Master backlinks | 0/6 | Domain pages 尚未添加指向 Master Concepts 的反向链接（planned） |
| Abstraction quality | ✅ | 两页均不包含 AI 技术实现细节或 Zettelkasten 操作教程。Pattern B emergence 边界明确限制 |
| Template conformance | ✅ | 均使用 master-concept-template.md 结构：Definition / Why This Matters / Cross-Domain Evidence / Relationships / Implications / Boundaries |
| Lifecycle status | accepted | Human 已批准第二次 /promote 评估，两个 Concept 均标记为 accepted |

### KM Domain Internal Validation

| 维度 | 状态 | 详情 |
|------|------|------|
| Concept pages | 3 | Note Atomicity (principle-type) + Zettelkasten Method (method-type) + Emergent Organization (result-type) |
| Method pages | 1 | Map of Content — curation method for emergent knowledge structures |
| Internal wikilinks | 12 live | Full bidirectional network across 4 pages. Principle → Method → Result → Curation chain closed |
| Taxonomy consistency | ✅ | Concepts in concepts/ (understanding stance), Method in methods/ (execution stance). MOC validates taxonomy flow |
| Principle→Method→Result→Curation chain | ✅ | Note Atomicity (WHAT) → Zettelkasten Method (HOW to link) → Emergent Organization (WHAT EMERGES) → Map of Content (HOW to curate). Complete lifecycle |
| Domain→Master backlinks | 5 | 5 KM→Master backlinks: 3 concepts × 2 Master + 1 method × 1 Master. Bidirectional traceability maintained |
| Growth Boundary health | ✅ | 0 Type A, 0 Type B (MOC created), 1 Type C (Digital Garden). No unresolved network-required nodes |

### 验证结论

- **Routing**: 正常 — 7 个概念页面的领域路由和知识类型选择均正确（4 AI + 3 KM）
- **Lifecycle**: 正常 — raw → processed → seedling → growing 状态转换正确执行。AI: 3 growing + 1 seedling。KM: 3 seedling
- **Wikilink Governance**: 正常 — AI 网络 9 live / 2 planned / 0 bare / 0 orphan。KM 网络 5 live / 2 planned / 0 bare / 0 orphan。Master 网络 2 internal live / 12 Domain→Master src links / 6 Domain→Master backlinks
- **Distillation**: 已验证 — 5 个 Domain 概念页面 + 2 个 Master 概念页面均满足质量检查清单
- **Master Boundary**: 已验证 — 首次 Knowledge Flow 完整执行：Capture → Domain → Cross-domain Pattern → Master Model

### S05 Cross-Domain Pattern Detection（2026-07-12）

首次跨领域模式检测执行。分析范围：AI Domain (4 concepts) + KM Domain (1 concept)。

**检测结果摘要**：

| 类型 | 数量 | 详情 |
|------|------|------|
| Observations | 2 | Pipeline Architecture 同构、Storage/Processing Separation |
| Candidate Patterns | 2 | Structured Minimal Unit Principle、Emergent Structure from Local Interactions |
| Master Concepts | 0 | 条件不满足——KM Domain 仅 1 个 concept，需要至少更多 KM 页面来验证跨领域稳定性 |

**关键判断**：

两个 Candidate Patterns 均已达到 `proposed` 级证据门槛（≥3 source pages, ≥2 domains），但 KM Domain 只有 1 个页面限制了统计置信度。KM Domain 第二概念（Zettelkasten Method 或 Emergent Organization）将为此分析提供关键的三点验证。

详细报告见 S05 输出中的 "Cross-Domain Pattern Detection Report"。

---

### Phase 1 Freeze Record（2026-07-12）

#### Freeze 条件

| 条件 | 状态 |
|------|------|
| Anchor Concepts 完整 | ✅ 2 anchors（Transformer + Vector Embeddings）支撑 2 derivatives |
| 依赖链闭合 | ✅ Transformer → Embeddings → (RAG ∥ Agent Memory) |
| 0 relationship 错误 | ✅ 9 条双向交叉验证通过 |
| 0 结构缺口（当前范围） | ✅ 所有 planned refs 已激活 |
| 无过度扩展 | ✅ pull-based 增长收敛，concepts 目录 4 页面 |
| Wikilink 健康 | ✅ 9 live / 2 planned / 0 bare / 0 orphan |

#### Topology（frozen）

```
Transformer Architecture  [seedling]     ← 架构基础层
       │
       ├── Encoder ──→ Vector Embeddings [growing]  ← 语义表示层
       │                    │
       └── Decoder ──→ ┌────┴────┐
                       ▼         ▼
                    RAG    Agent Memory
                 [growing]  [growing]     ← 应用管道层
```

#### Deferred Concept Candidates

| 候选 | 类型 | 触发条件 |
|------|------|----------|
| Tokenization | concept | capture 以 tokenization 为核心主题时 |
| Attention Mechanism | concept | 内容超出 Transformer Mechanism section 承载能力时 |
| Fine-tuning | concept | capture 覆盖此主题 + 需与 RAG 区分时 |
| Vector Database | technology | capture 覆盖具体向量数据库技术时 |
| LangChain | technology | capture 或 planned ref 触发 |
| LlamaIndex | technology | capture 或 planned ref 触发 |

#### Growth Rule（effective）

```
pull-based only:
  - 新 concept 仅当已有页面的 planned ref 明确指向时创建
  - 禁止推测性概念创建
  - 禁止为"知识完整性"而主动扩展

technologies/methods/entities:
  - 由 capture 或 planned ref 驱动
  - 优先更新已有页面，而非新增
```

---

## 5. Current Discussion Context

### S05 Master Wiki Construction — 已完成（2026-07-12）

S05 Cross-Domain Abstraction 的核心目标已达成：

1. **首次 `/promote` 执行**：从 2 Candidate Patterns + 2 Observations 中识别出 2 个满足 Master promotion 门槛的跨领域模式
2. **第二次 `/promote` 正式评估**：四维评估（Structural Isomorphism / Evidence Independence / Abstraction Stability / Master Boundary）确认两个模式达到 promotion 标准
3. **KM Domain Expansion**：Zettelkasten Method 创建，KM Domain 从 1→2 concepts，为第二次 /promote 提供双点验证
4. **KM Taxonomy Stabilization**：5 个知识类型的认知姿态定义（理解/执行/使用/引用/识别），concepts vs methods 边界澄清
5. **Master Wiki Construction**：2 个 Master Concept 创建，Master Wiki 从空壳进入 Activated 状态

### 当前焦点：S07 Complete — System Reorientation 完成

S07 全部三步已执行完毕：

**Step 1 — Emergent Organization**: KM Domain 第三概念页创建。Principle → Method → Result 推导链建立。跨 Note Atomicity + Zettelkasten Method 双源合成。

**Step 1.5 — Growth Boundary Classification**: Type A/B/C 三级增长分类建立并集成到 knowledge-linking.md §5。全部 planned reference 重新分类。Type B 标记建立（MOC → page required）。

**Step 2 — MOC Integration & Curation Layer**: KM Domain 首个 methods/ 页面创建（map-of-content.md）。三个概念页的 3 条 Type B (page required) 标记转为 live wikilinks。Principle → Method → Result → Curation 推导链闭合。Lint 验证通过：0 errors, 0 warnings。

**Step 3 — Closure & Stabilization**: KM schema 修复（MOC 从 planned 移至 existing）。KM Domain 声明为 **Foundation Complete, Pull-based Growth**。S07 阶段关闭。

KM Domain 当前状态：
- 4 pages: 3 concepts (all seedling) + 1 method (seedling)
- Complete chain: Note Atomicity → Zettelkasten Method → Emergent Organization → MOC
- 0 Type A, 0 Type B, 1 Type C (Digital Garden)
- 12 intra-domain live wikilinks, 5 KM→Master backlinks
- All pages seedling — organic growth expected through use, not forced promotion

### S06 Infrastructure Closure

Repository infrastructure stabilized:

- GitHub established as primary repository (development source of truth)
- Gitee configured as manual pull mirror (accessibility and backup)
- Single source of truth defined (GitHub only development, Gitee read-only mirror)
- Milestone sync workflow: GitHub tag → Gitee manual Pull Sync

### S06 关键决策记录

| # | 决策 | 影响 |
|---|------|------|
| Schema 从"目录归属"升级为"认知姿态分类" | 同一对象可跨类型存在多个页面 | 解决 RAG 在 AI schema 中的分类矛盾 |
| governance.md 作为 Master 唯一治理文件 | 不创建新 protocol/ 文件 | 遵守 Avoid Protocol Proliferation 原则 |
| Domain→Master backlink 使用 `### Master Concepts` 小节 | 表达 "provides evidence for" 语义 | 建立 Domain↔Master 双向可追溯性 |
| 首次 lint 范围限制为结构健康 | 不做知识质量自动判断 | 先建立基线，再逐步深化 |

### S07 Step 1 关键决策记录

| # | 决策 | 影响 |
|---|------|------|
| Emergent Organization 置入 concepts/（非 methods/） | 描述涌现现象（WHAT EMERGES），非操作方法 | 完成 Principle → Method → Result 推导链 |
| MOC 规划为 methods/（非 concepts/） | 定义为具体操作技术，降低 concept bloat | 将验证 taxonomy 的 concepts/ → methods/ 流动 |

### S07 Step 1.5 关键决策记录

| # | 决策 | 影响 |
|---|------|------|
| Growth Boundary 三级分类 (Type A/B/C) | 区分 Capture-driven / Network-required / Speculative Candidate 三种创建模式 | 正式化此前隐式的多模式创建实践。Agent 不再将所有 planned reference 视为同等可创建 |
| Type B 需 Human 批准 | Network-required 页面创建前必须获得 Human 确认 | 阻止 Agent 单方面基于"网络完整性"扩展 Domain |
| Type C 需 Human 确认或 future capture | Speculative Candidate 不能仅由 Agent 创建 | 将 Agent 领域知识判断从创建权降级为建议权 |
| 分类写入 knowledge-linking.md §5 | 增长边界规则作为链接协议的正式组成部分 | 不创建新 protocol 文件，符合 Avoid Protocol Proliferation |
| MOC 标记为 Type B (weak form) | 实践闭合层缺失（非理论理解层） | 弱形式 Type B 仍是 Type B——需要 Human 批准，但紧急度低于强形式 |
| Digital Garden + AI deferred → Type C | 所有 speculative 引用统一降级 | 语义精度提升：planned ≠ required ≠ candidate |

### S07 Step 2 关键决策记录

| # | 决策 | 影响 |
|---|------|------|
| MOC 置入 methods/（非 concepts/） | MOC 的本质是操作流程（HOW to curate），非概念理解（WHAT is curation） | 验证 taxonomy 的 concepts/ → methods/ 流动。concepts/ 已有三个页面完整描述了 MOC 的概念本质，methods/ 提供操作层 |
| MOC 的 descriptive-not-prescriptive 性质 | MOC 描述已涌现的结构，不预设知识应该属于哪里 | 将 MOC 从"一种组织方法"重新定位为"对涌现结构的策展操作"，使方法论基础从分类转向观察和策展 |
| MOC → emergent-structure-from-local-interactions (Master) | 方法 → 概念 的操作化映射（abstract mechanism → operational method），非证据提供 | 新增第 13 条 KM→Master backlink，不同于 concepts/ 的 "provides evidence for" 语义——这是 "operationalization of" |
| KM Domain Foundation Complete 声明 | 4-page 理论链闭合，0 Type B remaining，pull-based growth rule 生效 | KM Domain 从 Active Expansion 转入 Maintenance / Pull-based Growth 模式。未来扩展由 capture 或 human 决策驱动，非 agent 自主扩展 |

---

## 6. Pending Decisions

| # | 问题 | 状态 | 上下文 |
|---|------|------|--------|
| D1 | ai-agent-memory.md 是否按新标准重写 | ✅ 已解决 | 2026-07-12 按 knowledge-distillation.md 迁移为中文知识概念，8-section 结构 |
| D2 | AI Domain 知识语言策略 | ✅ 已解决 | `docs/language-policy.md` v1.0 定义：知识层中文为主，技术术语保留英文 |
| D3 | 其他 domain template 的蒸馏标准 | 待决定 | 当前仅 concept template 更新。method/technology/reference/entity 是否有类似需求？ |
| D4 | `/lint` 蒸馏质量检查 | 待决定 | 是否在 `/lint` 中增加 Article Summary 检测规则？ |
| D5 | Master Wiki 首次抽象提案 | ✅ 已解决 | S05 第二次 /promote 通过。2 个 Master Concept 已创建（2026-07-12），Master Wiki Activated |
| D6 | Knowledge Management Domain 首次内容 | ✅ 已解决 | Note Atomicity + Zettelkasten Method + Emergent Organization 概念页面创建（2026-07-12），KM Domain 已激活。Domain 现有 3 个概念页面，Principle→Method→Result 完整推导链建立 |
| D7 | 系统综合页面的 lifecycle 评估标准 | 待决定 | vector-embeddings 和 transformer-architecture 无 capture source（系统综合创建）。`knowledge-lifecycle.md` seedling→growing 的"source capture referenced"条件对此类页面不适用。当前用"网络集成度 + 多概念依赖"替代，但协议未显式覆盖此边缘案例 |
| D8 | Master Concepts 的分类位置 | 已记录 | 两个 Pattern 均放在 concepts/ 而非 principles/。当前决策：Master Wiki 以概念网络为核心构建。未来可在 principles/ 中创建应用导向的版本。此决策待更多 Master 内容后重新评估 |
| D9 | Domain→Master backlinks | ✅ 已解决 | S06 中已为 6 个 Domain source pages 添加 12 条 backlink（2 Master Concepts × 6 pages），使用 `### Master Concepts` 小节 + `provides evidence for` 语义。Domain↔Master 双向可追溯性已建立 |
| D10 | Structured Management Layer | 已记录（Deferred） | 2026-07-12 记录为 Type C 改进候选。评估 Obsidian Database 作为 Structured Management Layer 的可能性。等待触发信号。详见 `docs/project-state.md` §11 I1 |

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

S07 System Reorientation 已完成。系统处于稳定等待状态。

### 短期（等待 Human 方向）

1. **Digital Garden** — Type C (speculative candidate)。4 个页面引用，最自然的 KM Domain 下一步扩展。需要 Human 确认或 external capture 触发。
2. **KM Domain seedling → growing** — 当前 4 页全部 seedling。通过实际使用和 `/update` 周期自然提升，不建议强制 promotion。

### 中期（取决于 Human 方向）

3. **AI Domain deferred candidates** — Type C，当 capture 触发时创建（pull-based only）
4. **Cross-domain pattern mining** — S08 候选：AI↔KM 新增模式检测（MOC↔RAG retrieval strategy 结构类比等）
5. **System governance review** — 7 个 protocol 一致性审查，documentation drift 检查

### 长期（不紧急）

6. 其他 domain template 蒸馏标准扩展（D3）
7. `/lint` 蒸馏质量检查规则（D4）
8. 系统综合页面的 lifecycle 评估标准写入 protocol（D7）

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

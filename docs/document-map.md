---
created: 2026-07-11
updated: 2026-07-11
status: active
---

# LLM Wiki OS 文档地图（Document Map）

本文档为人类和维护 Agent 提供 LLM Wiki OS 项目的完整文档导航。主要使用中文说明，保留英文技术术语、文件名、命令和标识符不变。

---

## 1. 入口文件（Entry Points）

项目根目录的三个入口文件，定义了系统的身份、规则和用户指南。

| 文件 | 用途 | 适用对象 |
|------|------|----------|
| `README.md` | 项目概述、架构简介、快速开始指南 | 人类（首次接触项目时阅读） |
| `FEDERATION.md` | Federation 成员清单、治理模型、权限表、加入/退出机制 | 人类和 Agent（了解 Federation 结构和权限边界） |
| `CLAUDE.md` | Agent 行为宪法（Operating Constitution），定义 Agent 的身份、职责、决策框架和编辑规则 | Agent（每次会话自动加载）；人类（了解 Agent 的行为约束） |

**阅读顺序建议**：README.md → FEDERATION.md → CLAUDE.md

---

## 2. 项目状态（Project State）

| 文件 | 用途 |
|------|------|
| `docs/project-state.md` | 当前系统快照和上下文恢复入口 |

`project-state.md` 记录了项目的完整运行时状态：架构设计、设计决策（D1-D8）、实现进度、当前阶段、Git 历史和运维原则。它是会话恢复的**首要读取文件**——Agent 在新会话中应首先读取此文件以恢复上下文。

---

## 3. 协议层（Protocol Layer）

协议层定义了系统的规则和治理框架。所有协议文件位于 `protocol/` 目录。

| 文件 | 用途 |
|------|------|
| `protocol/LLM-Wiki-Federation-Protocol.md` | Federation 协议核心：宏观知识生命周期（RAW INPUT → INGESTED → CURATED → ABSTRACTED）、各层职责、跨层操作规范 |
| `protocol/metadata-schema.md` | 元数据规范：所有页面类型的 YAML frontmatter 字段定义、允许值和约束 |
| `protocol/domain-routing.md` | 领域路由规则：如何将 capture 分配到正确的 Domain Wiki |
| `protocol/knowledge-lifecycle.md` | 知识生命周期：微观状态转换（raw → processed → seedling → growing → evergreen → proposed → accepted → active）及每个状态的准入/退出标准 |
| `protocol/knowledge-flow.md` | 知识流动协议：知识在 Federation 各层之间的完整流动路径（采集 → 摄取 → 策展 → 抽象 → 整合）、命令作为流动操作符、决策点与权限边界、质量门、反馈回路 |
| `protocol/Git-Commit-Convention.md` | Git 提交规范：5 种 commit 类型（capture / update / reflect / promote / maintenance）及其语义 |

协议文件定义了**系统规则**——修改协议即改变系统行为，需要人类批准。

---

## 4. 模板层（Templates Layer）

模板定义了知识创建的格式规范。所有模板位于 `templates/` 目录。

### Raw 模板（采集层）

| 文件 | 用途 |
|------|------|
| `templates/raw-template.md` | Capture 文件模板：定义 `source_type`、`source`、`tags`、`status: raw` 等原始采集元数据 |

### Domain 模板（领域知识层）

| 文件 | 用途 |
|------|------|
| `templates/domain-concept-template.md` | 概念（Concept）页面模板：定义、关键点、关系、来源、笔记 |
| `templates/domain-method-template.md` | 方法（Method）页面模板：步骤、适用场景、前置条件、来源 |
| `templates/domain-technology-template.md` | 技术（Technology）页面模板：概述、核心特性、使用场景、来源 |
| `templates/domain-reference-template.md` | 参考（Reference）页面模板：书目信息、关键摘要、引用关系 |
| `templates/domain-entity-template.md` | 实体（Entity）页面模板：人物/组织/项目的描述、关系、来源 |

### Master 模板（主 Wiki 层）

| 文件 | 用途 |
|------|------|
| `templates/master-model-template.md` | 心智模型（Mental Model）模板：模型描述、适用域、跨域连接 |
| `templates/master-principle-template.md` | 原则（Principle）模板：启发式规则、适用范围、反例 |
| `templates/master-concept-template.md` | 跨域概念（Cross-domain Concept）模板：抽象定义、域间映射 |
| `templates/master-framework-template.md` | 个人框架（Personal Framework）模板：结构化方法论、组件、使用指南 |

模板定义了**知识格式**——修改模板影响未来所有新页面的结构。

---

## 5. 采集层（Capture Layer）

外部知识通过采集层进入系统，在被 `/ingest` 处理之前保持原始状态。

```
capture/
├── inbox/          # 原始 Markdown 采集文件（Web Clip、手动笔记、AI 对话等）
└── attachments/    # 二进制附件（图片、PDF 等）
```

**规则**：

- `inbox/` 中的原始内容不可修改
- 仅可更新 frontmatter 元数据（status、日期、链接）
- `/ingest` 命令将 capture 处理为 Domain Wiki 页面后，标记 `status: processed`

**Obsidian Web Clipper 模板**：`.obsidian/templates/llm-wiki-capture.md`

---

## 6. 知识层（Knowledge Layer）

知识层是系统生成和维护的知识结构，位于 `spaces/` 目录。

```
spaces/
├── master/                       # Master Wiki：跨域个人世界模型
│   └── wiki/
│       ├── models/               # 心智模型
│       ├── principles/           # 原则/启发式规则
│       ├── concepts/             # 跨域概念
│       └── frameworks/           # 个人框架
├── ai/                           # AI 领域 Wiki
│   ├── schema.md                 # 领域结构定义
│   ├── index.md                  # 领域知识索引
│   ├── log.md                    # 领域操作日志
│   └── wiki/
│       ├── concepts/             # AI 概念
│       ├── methods/              # AI 方法
│       ├── technologies/         # AI 技术
│       ├── references/           # AI 参考文献
│       └── entities/             # AI 实体（人物/组织/项目）
├── knowledge-management/         # 知识管理领域 Wiki
│   └── ...（结构同 AI 领域）
└── (future domains...)           # 未来可扩展更多领域
```

每个领域自包含：有自己的 schema、index、log 和 5 种知识类型的 wiki 子目录。

Master Wiki **不是**知识索引——它只存储跨领域抽象。

---

## 7. 验证历史（Validation History）

系统的审计报告和验证记录分布在两个目录中：

### `reports/` — 系统健康报告

| 文件 | 内容 |
|------|------|
| `reports/bootstrap-validation.md` | 项目引导阶段的验证报告 |
| `reports/SYSTEM_STATUS_REPORT.md` | 系统状态报告 |
| `reports/SYSTEM_VALIDATION_REPORT.md` | 综合系统验证报告 |
| `reports/system-alignment-check.md` | 系统对齐检查 |
| `reports/system-alignment-report.md` | 系统对齐详细报告 |
| `reports/knowledge-ingestion-readiness.md` | 知识摄取就绪评估 |

### `docs/` — 集成审计报告

| 文件 | 内容 |
|------|------|
| `docs/SYSTEM_INTEGRATION_AUDIT.md` | 跨层集成审计（24 通过 / 12 问题） |
| `docs/POST_FIX_VALIDATION.md` | 修复后验证报告 |

**原则**：历史报告应作为系统演化记录加以保留，不应删除。

---

## 8. 维护原则（Maintenance Principles）

### 导航优于删除

- 通过文档地图和索引提供导航，而非通过删除"过时"文件来简化结构
- 历史报告和审计文档是系统演化证据，默认保留

### 保留系统演化历史

- Git 记录代码演化，报告记录决策演化
- 验证报告串联起来构成系统的"成长日记"

### 使用 archive/ 处理废弃材料

- 如果某个文档确实不再需要，移至 `archive/` 而非直接删除
- 保留追溯能力

### 保持协议标识符稳定

- 协议文件名、YAML key、命令名、模板名一旦稳定就不再更改
- 如需变更，在所有受影响文件中同步更新并记录迁移路径

### 双语维护

- 系统层（文件名、标识符、命令）保持英文
- 人类文档层使用中文为主，技术术语保留英文
- 详见 `docs/project-state.md` 中的 Language Policy

---

## 附录：快速导航

| 我想... | 去这里 |
|---------|--------|
| 了解项目是什么 | `README.md` |
| 了解谁能做什么 | `FEDERATION.md` |
| 了解 Agent 行为规则 | `CLAUDE.md` |
| 恢复项目上下文 | `docs/project-state.md` |
| 查看知识生命周期 | `protocol/knowledge-lifecycle.md` |
| 查看知识流动路径 | `protocol/knowledge-flow.md` |
| 查看元数据字段定义 | `protocol/metadata-schema.md` |
| 创建新 wiki 页面 | `templates/` （选择对应模板） |
| 查看系统健康报告 | `reports/` |
| 查看集成审计 | `docs/SYSTEM_INTEGRATION_AUDIT.md`、`docs/POST_FIX_VALIDATION.md` |
| 了解当前项目状态和阶段 | `docs/project-state.md` |
| 了解语言使用策略 | `docs/project-state.md` → Language Policy |

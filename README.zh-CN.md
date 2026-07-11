# LLM Wiki OS

基于 Obsidian、Claude Code 和 Git 构建的个人知识操作系统。

## 这是什么？

LLM Wiki OS 是一个**个人知识操作系统**，结合人类策展与 AI Agent 维护。它以联邦架构组织知识，使信息从原始采集流向结构化 Wiki 页面，最终汇聚为跨领域心智模型。

## 核心理念

**知识联邦（Knowledge Federation）。** 知识存在于半自治的领域中，而非单一巨型 Wiki。每个领域自包含。跨领域模式被提升至主知识库——但仅在它们在多个上下文中被证明有用之后。

**人类 + Agent 协作（Human + Agent Collaboration）。** 你负责采集和策展。Agent 负责摄取、维护和发现。人类始终拥有最终决定权——Agent 仅可提议，不能单方面决策。

**知识生命周期管理（Knowledge Lifecycle Management）。** 每条知识都遵循定义好的生命周期：`raw → processed → seedling → growing → evergreen`。主知识库条目经过 `proposed → accepted → active`。每次状态转换都有明确的标准和权限边界。

## 架构

```
外部输入（Web、书籍、对话……）
        │
        ▼
┌───────────────────┐
│   采集层           │  ← 原始、不可变记录
│   capture/inbox/  │
└───────┬───────────┘
        │ /ingest
        ▼
┌───────────────────┐
│   领域知识库       │  ← 经策展的领域知识
│   spaces/{domain}/ │    seedling → growing → evergreen
└───────┬───────────┘
        │ /promote
        ▼
┌───────────────────┐
│   主知识库         │  ← 跨领域模型与原则
│   spaces/master/  │    人类控制
└───────────────────┘
```

- **采集层（Capture Layer）** — 所有输入信息的临时着陆区。原始内容永不可修改。
- **领域知识库（Domain Wiki）** — 按领域组织的结构化知识（AI、知识管理等）。每个领域包含 5 种知识类型：concepts、methods、technologies、references、entities。
- **主知识库（Master Wiki）** — 跨领域抽象：心智模型、原则、概念和个人框架。仅可由人类编辑。

## 当前功能

- **Web Clipper 采集** — 通过 Obsidian Web Clipper 直接将网页内容保存至 `capture/inbox/`
- **摄取管线** — `/ingest` 将采集内容路由至正确领域、选择模板并创建结构化 Wiki 页面
- **领域路由** — 基于内容分析自动分配领域，对模糊主题有边界区域处理机制
- **元数据治理** — 所有页面使用标准化的 YAML frontmatter，具有定义好的验证规则
- **知识链接治理** — 受控的 wikilink 使用显式路径，防止 Obsidian 根目录产生孤立文件
- **知识流动管理** — 从外部来源到主知识库的完整可追溯链，每个转换点设有质量门

## 项目结构

```
.
├── CLAUDE.md                  # Agent 行为宪法
├── FEDERATION.md              # Federation 成员、权限模型、信号
├── capture/inbox/             # 原始采集文件（内容不可变）
├── spaces/
│   ├── master/wiki/           # 跨领域世界模型
│   ├── ai/wiki/               # AI 领域知识
│   └── knowledge-management/  # 知识管理领域
├── protocol/                  # Federation 协议文档（6 份）
├── templates/                 # 页面模板（raw、domain × 5、master × 4）
├── .claude/commands/          # Agent 命令定义
├── docs/                      # 文档与审计报告
├── reports/                   # 健康检查报告
└── archive/                   # 已废弃内容
```

## 快速开始

### 前置条件

- [Obsidian](https://obsidian.md)（启用社区插件）
- [Claude Code](https://claude.ai/code)
- Git

### 设置步骤

1. **克隆**此仓库并在 Obsidian 中作为 Vault 打开
2. **配置插件** — 在 Obsidian 设置中启用社区插件（Web Clipper 模板位于 `.obsidian/templates/llm-wiki-capture.md`）
3. **开始采集** — 使用 Web Clipper 或将文件放入 `capture/inbox/`
4. **运行摄取** — 在 Claude Code 中运行 `/ingest` 将采集内容处理为结构化 Domain Wiki 页面
5. **审阅** — 在 Obsidian 中查看生成的页面。编辑、链接、完善

### 维护命令

| 命令 | 用途 |
|------|------|
| `/ingest` | 将原始采集处理为领域 Wiki 页面 |
| `/update` | 审查并刷新已有 Wiki 页面 |
| `/lint` | 运行质量检查（断链、孤立页面、过时内容） |
| `/promote` | 为跨领域模式提议主知识库条目 |
| `/reflect` | 每周系统健康审查 |

## 设计原则

- **原始即神圣** — 采集内容永不可修改
- **主知识库靠赢得** — 只有跨领域模式才能进入主知识库
- **联邦优于单体** — 每个领域自包含
- **人在回路中** — Agent 提议，人类决策
- **Git 为骨干** — 每次知识变更均被版本记录

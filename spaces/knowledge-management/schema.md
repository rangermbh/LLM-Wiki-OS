---
created: 2026-07-11
updated: 2026-07-12
domain: knowledge-management
status: active
---

# Knowledge Management Domain Schema

## Scope

Personal Knowledge Management (PKM): knowledge organization systems, Obsidian workflows, LLM-assisted knowledge processing, information architecture, note-taking methodologies, knowledge graph design, and related topics.

## Directory Map

```
knowledge-management/
├── schema.md         # This file — domain definition
├── index.md          # Knowledge index
├── log.md            # Change log
├── raw/              # Processed captures (read-only)
├── wiki/
│   ├── concepts/     # Core concepts and definitions
│   ├── methods/      # Techniques, workflows, procedures
│   ├── technologies/ # Tools, plugins, platforms
│   ├── references/   # Papers, books, articles
│   └── entities/     # People, projects, communities
└── sources/          # External reference materials
```

## Topics

- Personal Knowledge Management (PKM) paradigms
- Obsidian workflows and plugin ecosystems
- LLM-assisted knowledge creation and curation
- Information organization and taxonomy design
- Knowledge graph theory and practice
- Note-taking methodologies (Zettelkasten, PARA, MOC, etc.)
- Digital garden philosophy
- Knowledge lifecycle management
- Federated wiki architectures

## Knowledge Types

页面分类依据为**认知姿态（epistemic stance）**，而非主题名称或目录归属。

| 认知姿态 | 核心问题 | 目录 |
|----------|----------|------|
| 理解层（Concept） | 这是什么？它为什么有效？ | `wiki/concepts/` |
| 执行层（Method） | 怎么做？什么时候用/不用？ | `wiki/methods/` |
| 工具层（Technology） | 用什么工具/系统实现？ | `wiki/technologies/` |
| 来源层（Reference） | 信息从哪来？ | `wiki/references/` |
| 实体层（Entity） | 谁/哪个组织/哪个项目？ | `wiki/entities/` |

**关键规则**：同一现实对象可以存在多个知识类型页面。例如 Zettelkasten Method——当研究其机制和原则时是 Concept（当前）；当编写具体操作步骤和工作流时是 Method（未来可创建）。

### 当前实例

| Type | Directory | Examples (existing) | Examples (planned) |
|------|-----------|---------------------|--------------------|
| Concepts | `wiki/concepts/` | Note Atomicity, Zettelkasten Method | Map of Content, Digital Garden, Emergent Organization |
| Methods | `wiki/methods/` | — | Progressive Summarization, Zettelkasten Workflow |
| Technologies | `wiki/technologies/` | — | Obsidian, Dataview Plugin |
| References | `wiki/references/` | — | How to Take Smart Notes, Building a Second Brain |
| Entities | `wiki/entities/` | — | Niklas Luhmann, Tiago Forte |

### 分类说明

- **Note Atomicity** 归类为 Concept：它定义知识单元的性质（边界约束、完整性要求、标题契约）——这是"知识单元应该是什么"的理解，而非"如何操作"的步骤
- **Zettelkasten Method** 归类为 Concept（当前）：本页面讨论其机制、原则和设计哲学。如果未来创建操作步骤版本（每日工作流、索引维护流程），那将放入 Methods

## Cross-Domain Connections

<!-- Forward references: these pages will be created as the knowledge base grows. -->

- *LLM as Knowledge Curator* — bridges with AI domain (→ future cross-domain concept)
- *Federation Architecture* — this repository's own architecture is a PKM case study (→ future Master model)
- *Information Compression* — connects AI (compression theory) with PKM (summarization) (→ future Master principle)

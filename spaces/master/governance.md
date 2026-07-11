---
created: 2026-07-12
type: governance
status: active
---

# Master Wiki Governance

Master Wiki 的治理规则。本文件不创建新协议——它固化 S05 中已实际验证的规则。

---

## 1. Master Concept 生命周期

```
Candidate Pattern
      ↓
Evidence Validation
      ↓
Human Approval
      ↓
Master Concept (accepted)
      ↓
Periodic Review
```

### 1.1 Candidate Pattern

**触发条件**：`/promote` 命令在至少两个领域中发现结构同构。

**输出**：Promotion Proposal，包含：
- 跨领域模式描述
- 至少两个领域的证据摘要
- 结构同构论证
- 边界条件初稿

**状态**：`proposed`

### 1.2 Evidence Validation

**验证内容**：
- 每个引用来源确实展示了声称的结构
- 跨领域映射不是表面类比（共享术语但机制不同）
- 存在至少两个独立来源（来自不同领域、不同时间、独立创建）

**验证人**：Agent 执行，结果记录在 proposal 中。

**状态**：`validated` 或退回 `candidate`

### 1.3 Human Approval

**要求**：所有 Master Concept 创建需人类批准。

**批准依据**：Promotion Proposal + Evidence Validation 结果。

**状态**：`accepted` 或退回 `candidate`

### 1.4 Master Concept (Active)

**活跃期要求**：
- 保持 source 链接有效
- 在 Periodic Review 中重新验证

**状态**：`accepted`

### 1.5 Periodic Review

**触发**：每次 `/reflect` 或 `/promote` 执行时顺带审查已接受的 Master Concepts。

**检查**：
- Source pages 是否仍然存在且内容一致
- 是否有新的跨领域证据可以加强或削弱概念
- 边界条件是否仍然准确

**结果**：保持 `accepted`、更新内容、或标记 `review_needed`

---

## 2. Promotion Criteria

Candidate Pattern 必须满足以下全部条件才能进入 Evidence Validation：

| # | 条件 | 说明 |
|---|------|------|
| C1 | 至少两个独立来源 | 来源必须来自不同领域、不同时间、独立创建。共享创始人或理论传承不算独立 |
| C2 | 至少覆盖两个领域 | Domain Wiki 层级。同一领域内的两个子领域不算 |
| C3 | 结构同构验证 | 不是表面类比。必须证明两个领域的底层机制共享相同的结构模式，而非仅共享术语 |
| C4 | 去术语化后仍成立 | 如果用领域中性的语言重新表述模式，其逻辑是否仍然有效？如果去掉领域特定术语后模式崩塌，则不是真正的跨领域模式 |
| C5 | 明确边界条件 | 清楚说明模式在什么条件下成立、什么条件下不成立。没有边界的普遍主张不是 Master Concept |

**反例**：
- "AI 中的 token 和笔记中的段落都是信息块" → 不满足 C3（表面类比，机制不同）
- "所有复杂系统都有涌现" → 不满足 C5（无边界条件）

---

## 3. Master Concept 必须包含的结构

每个 `accepted` Master Concept 页面必须包含以下全部章节：

| 章节 | 内容 |
|------|------|
| **Domain-independent definition** | 不使用任何领域术语的核心定义 |
| **Cross-Domain Evidence** | 每个来源领域的独立证据，带 wikilink 回 Domain 页面 |
| **Structural mechanism** | 模式如何运作——不是隐喻，是机制 |
| **Limitations / Boundaries** | 模式在什么条件下失效 |
| **Relationships** | 与其他 Master Concepts 的关系 |

**质量检查**：如果去掉 "Cross-Domain Evidence" 章节后从定义和机制仍然可以理解这个概念，说明定义达到了领域独立性要求。

---

## 4. 与 Domain Wiki 的关系

### 4.1 知识流方向

```
Domain Evidence → Master Concept
```

Domain 页面通过 backlink 表达 `provides evidence for` 关系。Master 页面通过 `sources` frontmatter 和 Cross-Domain Evidence 章节表达 `derived from` 关系。

### 4.2 双向可追溯性

- **向上追溯**：从 Domain 页面的 "Master Concepts" 小节可以找到它支撑的 Master Concepts
- **向下追溯**：从 Master 页面的 "Cross-Domain Evidence" 章节可以找到所有来源 Domain 页面

### 4.3 修改传播

- Domain 页面被修改时：不影响 Master Concept 的 `accepted` 状态，但在下次 Periodic Review 中标记为需要重新验证
- Master Concept 被修改时：不需要同步修改 Domain 页面（backlink 方向不变）

---

## 5. Master Wiki 子目录使用规则

| 目录 | 用途 | 何时创建 |
|------|------|----------|
| `wiki/concepts/` | 跨领域概念 | 当两个以上领域展示结构同构时 |
| `wiki/models/` | 心智模型 | 当跨领域概念可以组合为预测性模型时 |
| `wiki/principles/` | 启发式原则 | 当跨领域概念可以提炼为可操作的决策规则时 |
| `wiki/frameworks/` | 个人框架 | 当多个原则和模型可以集成为结构化方法论时 |

**规则**：只在实际有内容可放时才创建子目录中的页面。不预先填充。

---

## 6. 治理原则

1. **Evidence-driven expansion** — 不提前创建概念。Master Wiki 的增长由 Domain 层的证据积累驱动
2. **Minimal governance** — 只固化已实际验证的规则。不为尚未遇到的情况预先立法
3. **Avoid protocol proliferation** — 本文件是 Master Wiki 的唯一治理文件。不创建额外的 protocol/ 文件来覆盖相同内容
4. **Structure before scale** — 在扩展 Master 内容之前，确保治理框架和验证流程可靠

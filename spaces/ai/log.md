---
created: 2026-07-11
updated: 2026-07-12
---

# AI Domain Log

| Date | Event | Description |
|------|-------|-------------|
| 2026-07-11 | genesis | AI domain initialized |
| 2026-07-11 | ingest | Routed "AI agent memory types, architecture & implementation" → wiki/concepts/ai-agent-memory.md. Domain: AI. Type: concept. |
| 2026-07-11 | update | ai-agent-memory.md: fixed bare wikilinks (5 non-existent concepts converted to planned references per knowledge-linking protocol), promoted status seedling→growing (all sections filled, incoming link from index, source capture referenced), added TODO for missing concept pages |
| 2026-07-11 | update | index.md: added planned entries for RAG, Vector Embeddings, Transformer Architecture, LangChain, LlamaIndex |
| 2026-07-11 | update | schema.md: reviewed — cross-domain forward references correctly use italic + "(→ future ...)" notation, no wikilink violations |
| 2026-07-11 | ingest | Routed "什么是 RAG？— 检索增强生成 AI 详解" → wiki/concepts/rag.md. Domain: AI. Type: concept. Primary intent: RAG architecture explanation → AI domain. |
| 2026-07-12 | update | ai-agent-memory.md: migrated from Article Summary to Knowledge Concept per knowledge-distillation.md v1.0.0. Restructured from 5-section (EN) to 8-section (ZH-primary). Added Core Idea, Mechanism, Applications, Limitations. Stripped Redis marketing language. Status unchanged (growing). |
| 2026-07-12 | ingest | Created wiki/concepts/vector-embeddings.md as foundational AI concept node. Connects to RAG (encoding stage dependency) and Agent Memory (shared encoding-retrieval architecture). Activated planned references in rag.md and ai-agent-memory.md. |
| 2026-07-12 | update | rag.md: fixed RAG↔Agent Memory relationship wording. Changed from subset framing ("RAG 是 Agent 记忆架构中的检索+集成半区") to sibling/shared-pattern framing ("独立的同级概念...共享四阶段管道架构"). |
| 2026-07-12 | ingest | Created wiki/concepts/transformer-architecture.md as foundational AI architecture concept node. Serves as the architectural base for all three existing concepts: encoder → Vector Embeddings, decoder → RAG & Agent Memory generation/integration phases. Activated planned references in rag.md, ai-agent-memory.md, and vector-embeddings.md. |
| 2026-07-12 | update | rag.md: lifecycle assessment — promoted seedling→growing. All three criteria met: 8 sections complete, 4 incoming links (index + 3 concepts), source capture referenced. Page has been re-distilled per knowledge-distillation.md and cross-linked in the v2 network. |
| 2026-07-12 | update | vector-embeddings.md: lifecycle assessment — promoted seedling→growing. Two of three literal criteria met (all sections complete, 4 incoming links); source capture criterion inapplicable (system-synthesized foundational concept). Network integration and multi-concept usage substitute for capture traceability in this edge case. |
| 2026-07-12 | freeze | **AI Domain Phase 1 Freeze**. Network review passed: 2 anchors + 2 derivatives, 0 relationship errors, 0 structural gaps. 4-concept topology frozen. Pull-based growth rule effective. Domain enters maintenance / expansion-ready mode. Deferred candidates: Tokenization, Attention, Fine-tuning (concepts); Vector Database, LangChain, LlamaIndex (technologies). |
| 2026-07-12 | classify | S07 Step 1.5 Growth Boundary Classification。AI index deferred candidates 统一标记为 Type C (page candidate)：将 `<!-- planned:` 改为 `<!-- candidate:`（LangChain, LlamaIndex）。Freeze 注释增加显式 Type C 标记。 |

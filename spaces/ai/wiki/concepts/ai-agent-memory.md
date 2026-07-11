---
created: 2026-07-11
updated: 2026-07-11
domain: ai
tags:
  - ai-agent-memory
  - long-term-memory
  - vector-databases
  - llm
  - agent-architecture
status: seedling
sources:
  - "[[capture/inbox/AI agent memory types, architecture & implementation]]"
captured_from: "capture/inbox/AI agent memory types, architecture & implementation.md"
ingested_by: claude-opus-4.7
---

# AI Agent Memory

## Definition

AI agent memory is the infrastructure layer that turns stateless language models into stateful systems capable of remembering past interactions, maintaining context across sessions, and building on experience over time.

## Key Points

- LLMs are inherently stateless — each request starts from scratch without memory infrastructure
- Memory systems provide session persistence, cross-session learning, and selective context access that context windows alone cannot deliver
- Short-term (working) memory maintains context within a single interaction; resets when the session ends
- Long-term memory persists across sessions using vector databases for semantic similarity search
- Specialized memory types include episodic (past experiences with temporal details), semantic (factual knowledge), and procedural (task workflows)
- The four-stage architecture is: Encoding (vector embeddings) → Storage (vector database indexing) → Retrieval (approximate k-NN similarity search) → Integration (context augmentation into LLM prompt)
- HNSW indexing provides high recall at low latency for small-to-mid-size datasets; IVF is more memory-efficient at billion-vector scale
- Production systems typically start with short-term and long-term memory, adding specialized types only when operational value justifies the complexity

## Relationships

- [[rag|RAG]] — retrieval-augmented generation is the retrieval+integration half of the memory architecture
- [[vector-embeddings|Vector Embeddings]] — the encoding layer that converts data into semantic vectors
- [[transformer-architecture|Transformer Architecture]] — the underlying model architecture that consumes retrieved context
- [[langchain|LangChain]] / [[llamaindex|LlamaIndex]] — frameworks that implement agent memory patterns

## Sources

- [[capture/inbox/AI agent memory types, architecture & implementation]] — Redis blog, Jim Allen Wallace, 2026

## Notes

- Start with short-term + long-term memory; add episodic/semantic/procedural only as needed — premature specialization adds complexity without proportional value
- The quality of each memory pipeline stage affects the next: poor chunking → poor embeddings → poor retrieval → poor responses
- In-memory architectures (Redis) deliver microsecond latency but cost more per GB; choose based on whether latency compounds across multiple agent reasoning steps

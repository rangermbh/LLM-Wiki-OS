# Domain Routing Rules

This document defines how the Wiki Maintainer Agent determines which Domain(s) a capture or new piece of knowledge belongs to.

Version: 1.0.0

---

## 1. Routing Decision Tree

When processing a capture during `/ingest`, follow this decision tree:

```
Read capture content
        │
        ▼
Does it match a registered Domain scope?
        │
   ┌────┴────┐
   │ YES     │ NO
   ▼         ▼
Single    Propose new Domain
domain?   OR ask human where
   │      to file it
   │
┌──┴──┐
│ ONE │ MANY
▼     ▼
Route  Multi-Domain
to     handling
that   (see §3)
domain
```

## 2. Domain Scope Boundaries

### Active Domains

| Domain | Scope | Keywords | Anti-Scope |
|--------|-------|----------|------------|
| AI | Artificial Intelligence | ML, deep learning, NLP, computer vision, RL, LLMs, AI alignment, AI safety, prompt engineering, neural networks, transformers, diffusion models, AGI | Software engineering unrelated to AI, hardware without AI context, AI business without technical content |
| Knowledge Management | Personal Knowledge Management | PKM, Obsidian, note-taking, Zettelkasten, PARA, MOC, digital garden, knowledge graph, information architecture, spaced repetition, progressive summarization | Enterprise KM, library science (unless personal), database design |

### Boundary Zones (Overlap)

These topics touch BOTH domains. Apply the Primary Intent Rule (§4).

| Topic | Touches | Resolution |
|-------|---------|------------|
| LLM-assisted note-taking | AI + KM | Primary intent: if focus is on the note-taking workflow → KM. If focus is on the LLM technique → AI. |
| Knowledge graph construction with LLMs | AI + KM | Primary intent: if about the graph structure/organization → KM. If about the ML model that builds graphs → AI. |
| Prompt engineering for personal knowledge tools | AI + KM | Primary intent: if about the prompts themselves → AI. If about the PKM tool integration → KM. |
| RAG for personal knowledge bases | AI + KM | Primary intent: if about the RAG architecture → AI. If about setting up a personal RAG system → KM. |

---

## 3. Multi-Domain Handling

If a capture clearly belongs to 2+ domains:

### Rule: One Primary, Cross-Links

1. **Choose the primary domain** based on the capture's main focus.
2. **Create the wiki page** in the primary domain.
3. **Cross-link** from the secondary domain's index or a relevant page.
4. **Record in both domains' logs** that a cross-domain connection was made.

### Example

Capture: "Using GPT-4 to automatically organize Obsidian notes"

- Primary: Knowledge Management (the workflow and tool use)
- Secondary: AI (the LLM technique)
- Action: Create page in `spaces/knowledge-management/wiki/methods/llm-note-organization.md`, add cross-link from `spaces/ai/index.md` under "Applications" or a relevant section.

### When to Create in BOTH Domains

Only if the capture contains **substantially different content** for each domain — e.g., a long article with an AI theory section AND a separate PKM methodology section. In this case, create two pages, each covering their domain's portion, and cross-link them.

---

## 4. Primary Intent Rule

When a topic sits in a boundary zone:

1. Ask: **"What problem is this knowledge trying to solve?"**
2. If the answer is a Knowledge Management problem → route to KM.
3. If the answer is an AI/technical problem → route to AI.
4. If the answer is ambiguous → ask the human.

### Test Questions

| Capture | Primary Intent Question | Likely Domain |
|---------|------------------------|---------------|
| "How to fine-tune Llama 3" | Is this about model training or about using a fine-tuned model in a workflow? | AI (training) vs KM (workflow) |
| "Obsidian Dataview query examples" | Is this about the query language or about knowledge organization patterns? | KM (both interpretations lead to KM) |
| "Vector embeddings explained" | Is this about the ML concept or about using embeddings in a PKM tool? | AI (concept) vs KM (application) |

---

## 5. Unknown Domain Handling

If a capture does NOT match any registered Domain scope:

### Thresholds

| Condition | Action |
|-----------|--------|
| First capture on a new topic | Ask human: "This doesn't match any domain. Create a new domain or file elsewhere?" |
| Second capture on the same new topic | Propose creating a new domain with suggested scope. |
| Third capture on the same new topic | Strongly recommend new domain creation. |

### New Domain Proposal Template

When proposing a new domain, provide:
1. Suggested domain name (lowercase, hyphenated)
2. Scope statement (2-3 sentences)
3. 3-5 topic areas
4. Cross-domain connections to existing domains

---

## 6. Content Type Selection

Once the domain is determined, select the appropriate template:

| Content Characteristic | Template |
|------------------------|----------|
| Defines or explains an idea | domain-concept |
| Describes a procedure or technique | domain-method |
| Reviews or documents a tool/platform | domain-technology |
| Summarizes a paper, book, or article | domain-reference |
| Describes a person, org, or project | domain-entity |

### Ambiguous Content

If a capture contains multiple content types (e.g., explains a concept AND describes a method):
- Create **one page** using the dominant type
- Mention the secondary aspect in the Notes section
- If both are substantial, create two linked pages

---

## 7. Routing Audit Trail

Every routing decision must be recorded:

1. In the Domain's `log.md`: `| date | ingest | Routed "capture-name" to wiki/<type>/<page-name>. Primary intent: <reasoning>. |`
2. In the created page's `captured_from` field: trace back to capture file.
3. In the capture file's `ingested_to` field: wikilink to created page(s).

---

*Protocol version 1.0.0, aligned with FEDERATION.md and CLAUDE.md*

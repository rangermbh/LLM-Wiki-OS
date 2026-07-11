# LLM Wiki Federation Protocol

## Federation Philosophy

The LLM Wiki OS is a **personal knowledge operating system** built on the principle of **federated curation**. Rather than a single monolithic wiki, knowledge is organized into a federation of semi-autonomous spaces, each with its own scope, rules, and maintainers.

## Three-Layer Model

```
┌─────────────────────────────────┐
│         MASTER WIKI             │
│   Cross-domain models &         │
│   long-term principles          │
├─────────────────────────────────┤
│         DOMAIN WIKIS            │
│   spaces/ai/  spaces/research/  │
│   Domain facts, methods, tools  │
├─────────────────────────────────┤
│         CAPTURE LAYER           │
│   Raw inputs, unfiltered        │
└─────────────────────────────────┘
```

## Master / Domain Boundary

### Master Wiki Scope (Upward Flow)

Master Wiki contains **only** knowledge that transcends individual domains:

- **Models**: Personal mental models distilled from multiple domains (e.g., "Compression as Understanding")
- **Principles**: Long-term heuristics validated across contexts (e.g., "Prefer Simplicity Over Completeness")
- **Concepts**: Abstract ideas that bridge domains (e.g., "Emergence", "Feedback Loops")

### Domain Wiki Scope (Lateral Flow)

Each Domain Wiki is a **complete, self-contained knowledge base** for one domain:

- Facts and definitions
- Methods and techniques
- Tools and configurations
- References and sources

### Boundary Enforcement

- Master MAY reference Domain pages via wikilinks.
- Domain pages MAY reference Master concepts.
- Master MUST NOT duplicate Domain content.
- Domain pages MUST NOT make claims about other domains.

## Knowledge Lifecycle

```
RAW INPUT  →  PROCESSED  →  CURATED  →  ABSTRACTED
(capture/)    (domain/)     (domain/)    (master/)
```

### 1. Capture (capture/inbox/)

- All external information enters here.
- Immutable. Source of truth for what was captured.
- Examples: web clips, PDFs, AI conversation exports, manual notes.

### 2. Processing (Domain raw → wiki)

- Read raw captures, extract structured knowledge.
- Create Domain wiki pages.
- Link to existing pages.
- Update Domain index and log.

### 3. Curation (Domain wiki maintenance)

- Regular quality checks (lint).
- Update outdated information.
- Resolve conflicting pages.
- Archive superseded content.

### 4. Abstraction (Domain → Master)

- When a pattern appears across 3+ Domain pages, propose a Master entry.
- Human reviews and approves/rejects.
- Master entry links back to source Domain pages.

## Federation Signals

When the Maintainer Agent detects:

| Signal | Action |
|--------|--------|
| 3+ domains share a pattern | Propose Master model/principle |
| Domain page contradicts Master | Flag for human review |
| Domain page is stale (>6 months) | Suggest review |
| Capture inbox has new items | Offer to process |
| Orphan page (no incoming links) | Suggest linking or archiving |

## Protocol Version

- Version: 1.0.0
- Created: 2026-07-11
- Maintainer: Claude Code (Wiki Maintainer Agent)

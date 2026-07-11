---
created: 2026-07-11
updated: 2026-07-11
status: active
---

# Master Wiki Schema

## Purpose

The Master Wiki stores **cross-domain abstractions**: personal cognitive models, long-term principles, and universal concepts that transcend any single knowledge domain.

## What Belongs Here

- **Models**: Personal mental models validated across multiple domains
- **Principles**: Long-term heuristics and decision rules
- **Concepts**: Abstract ideas that bridge domains

## What Does NOT Belong Here

- Domain-specific facts (→ Domain Wiki)
- Knowledge indices or directories (→ Domain `index.md`)
- Reference lists or bibliographies (→ Domain `sources/`)
- Raw captures or notes (→ `capture/`)

## Directory Map

```
wiki/
├── models/       # Mental models (e.g., "Compression as Understanding")
├── principles/   # Decision heuristics (e.g., "Prefer Simplicity")
└── concepts/     # Abstract bridging ideas (e.g., "Emergence")
```

## Page Lifecycle

```
proposed → accepted → active → superseded
                ↓
             rejected
```

- `proposed`: Agent suggested, awaiting human review
- `accepted`: Human approved, now part of Master
- `active`: Currently in use and referenced
- `superseded`: Replaced by a newer model (moved to archive/)
- `rejected`: Human declined the proposal

## Quality Rules

1. Every Master page must cite source Domain pages
2. No page without cross-domain evidence
3. Minimum 3 source pages from 2+ domains for acceptance
4. Regular review for staleness (every 6 months)

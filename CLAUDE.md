# CLAUDE.md — LLM Wiki OS Maintainer Agent

You are the **Wiki Maintainer Agent** for this LLM Wiki OS repository.

## System Architecture

This repository implements a **3-layer LLM Wiki Federation**:

```
Capture Layer  (capture/)     → Raw, unprocessed external inputs
Domain Layer   (spaces/*/)     → Curated domain-specific knowledge
Master Layer   (spaces/master/) → Cross-domain models, principles, concepts
```

## Core Rules

### Master Wiki Rules

- **Read-only by default.** You may NOT edit `spaces/master/` autonomously.
- You may only **suggest** updates to Master Wiki. The human decides.
- Master Wiki stores: cross-domain patterns, long-term principles, personal cognitive models.
- Master Wiki does NOT store: domain facts, knowledge indices, reference lists.

### Domain Wiki Rules

- You MAY create and edit Domain Wiki pages (spaces/*/wiki/).
- Every Domain must maintain: `schema.md`, `index.md`, `log.md`.
- Domain `raw/` files are read-only — never modify them.
- Each Domain is self-contained. Cross-references via wikilinks are encouraged.

### Capture Rules

- `capture/inbox/` and `capture/attachments/` are **raw content**.
- NEVER modify, rewrite, or delete files in capture/.
- You MAY read capture files to process them into Domain Wiki pages.
- After processing, the human marks them as processed (you do not move them).

### Modification Permissions

| Area | Create | Edit | Delete |
|------|--------|------|--------|
| capture/ | No | No | No |
| spaces/*/wiki/ | Yes | Yes | No (archive only) |
| spaces/*/raw/ | No | No | No |
| spaces/*/sources/ | Yes | Yes | No (archive only) |
| spaces/master/ | No (suggest) | No (suggest) | No |
| templates/ | Yes | Yes | No |
| .claude/ | Yes | Yes | No |
| protocol/ | Yes | No | No |

### Knowledge Quality Standards

- Each concept page must have: definition, relationships, sources.
- No orphan pages — every page must be linked from an index or another page.
- No duplicate pages — search before creating.
- Use wikilinks `[[page-name]]` for internal references.
- Metadata via YAML frontmatter: `created`, `updated`, `tags`, `status`.

## Workflow Commands

See `.claude/commands/` for detailed command definitions:
- `ingest` — Process capture inbox
- `update` — Update existing wiki pages
- `lint` — Check knowledge quality
- `promote` — Propose Master Wiki content
- `reflect` — Review and suggest improvements

## Granularity Standards

- **lob** (large): > 5 paragraphs, multi-section analysis.
- **mid** (medium): 5-10 sentences.
- **bob** (small): A single atomic idea in 1-4 sentences. Prefer this.

# LLM Wiki Federation

## Manifest

This is the **LLM Wiki Federation** — a personal knowledge operating system organized as a federation of semi-autonomous wikis governed by a shared Operating Constitution (`CLAUDE.md`).

**Founded**: 2026-07-11
**Maintainer**: Claude Code (Wiki Maintainer Agent)
**Constitution**: `CLAUDE.md`
**Protocol**: `protocol/LLM-Wiki-Federation-Protocol.md`

## Federation Members

### Master Wiki

| Attribute | Value |
|-----------|-------|
| Path | `spaces/master/` |
| Role | Cross-domain personal world model |
| Authority | Human only (Agent proposes, never directly edits) |
| Content | Models, Principles, Concepts, Frameworks |

### Domain Wikis

| Domain | Path | Status | Scope |
|--------|------|--------|-------|
| AI | `spaces/ai/` | active | Artificial Intelligence, ML, DL, NLP, LLMs, AI Safety |

## Governance

### Authority Model

| Action | Human | Agent |
|--------|-------|-------|
| Create Domain pages | Approve | Execute |
| Edit Domain pages | Approve | Execute |
| Delete Domain pages | Approve | Propose |
| Create Master pages | **Must approve** | Propose only |
| Edit Master pages | **Must approve** | Propose only |
| Delete Master pages | **Must approve** | Propose only |
| Modify architecture | **Must approve** | Propose only |
| Modify capture files | **Forbidden** | **Forbidden** |

### Decision Framework

1. When new information arrives: identify source → find related knowledge → determine destination (Capture / Domain / Master Proposal) → explain reasoning.
2. When uncertain: do not guess, explain options, recommend approach, wait for approval.
3. Master Wiki changes always require a Promotion Proposal.

### Federation Signals

| Signal | Response |
|--------|----------|
| 3+ domains share a pattern | Propose Master model/principle |
| Domain page contradicts Master | Flag for human review |
| Domain page stale >6 months | Suggest review |
| Capture inbox has new items | Offer to process |
| Orphan page detected | Suggest linking or archiving |

## Joining the Federation

New domains are created by the human. To add a domain:

1. Create `spaces/<domain>/` with `schema.md`, `index.md`, `log.md`
2. Create knowledge subdirectories under `wiki/`: concepts, methods, technologies, references, entities
3. Register the domain in this manifest
4. Agent verifies: schema validity, template compatibility, index consistency

## Leaving the Federation

Domains may be archived by moving to `archive/domains/<domain>/`. Master Wiki references to archived domains must be updated.

## Version

- Manifest version: 1.0.0
- Compatible Protocol: v1.0.0
- Compatible Constitution: CLAUDE.md v2 (2026-07-11)

---
created: 2026-07-11
updated: 2026-07-11
status: complete
audit_type: system-integration
scope: full-stack
---

# System Integration Audit

Full-stack cross-layer audit of the LLM Wiki OS — every document, template, command, and protocol checked for alignment.

**Date**: 2026-07-11
**Auditor**: Claude Code (Wiki Maintainer Agent)
**Method**: Read every file. Trace every reference. Verify every field name, status value, lifecycle transition, and directory path.

---

## 1. Architecture Status

| Layer | Files | Directories | Status |
|-------|-------|-------------|--------|
| CLAUDE.md | 1 (517 lines) | — | Active |
| Protocol | 5 docs | — | Active |
| Commands | 5 docs | — | Active |
| Templates | 10 docs | — | Active |
| Master Wiki | schema, index, log | 4 wiki dirs | Active (empty) |
| Domain: AI | schema, index, log | 5 wiki dirs + raw + sources | Active (empty) |
| Domain: KM | schema, index, log | 5 wiki dirs + raw + sources | Active (empty) |
| Capture | — | inbox, attachments | Active (empty) |
| Archive | — | root .gitkeep only | Active (empty) |
| .gitkeep coverage | 21 files | All empty dirs protected | Complete |

**Overall**: Structure is sound. All required directories exist. All .gitkeep files are in place. No missing files at the structural level.

---

## 2. Passed Checks

The following checks passed with no issues found:

| # | Check | Detail |
|---|-------|--------|
| P1 | CLAUDE.md ↔ protocol docs | High-level architecture matches across all docs |
| P2 | Domain count ≥ 2 | 2 domains active (AI, KM) — federation can detect cross-domain patterns |
| P3 | FEDERATION.md domain list | Matches actual `spaces/` directories |
| P4 | domain-routing.md domain table | Matches FEDERATION.md members |
| P5 | domain-routing.md content type matrix | 5 types → 5 templates, consistent with `/ingest` matrix |
| P6 | knowledge-lifecycle.md status values | Match metadata-schema.md allowed values exactly |
| P7 | metadata-schema domain status enum | seedling, growing, evergreen, superseded — matches lifecycle |
| P8 | metadata-schema master status enum | proposed, accepted, active, superseded, rejected — matches lifecycle |
| P9 | Template count | 1 raw + 5 domain + 4 master = 10 templates — complete coverage |
| P10 | Master wiki categories | 4 (models, principles, concepts, frameworks) — schema ↔ index ↔ dirs ↔ templates aligned |
| P11 | Domain wiki types | 5 (concepts, methods, technologies, references, entities) — schema ↔ index ↔ dirs ↔ templates aligned |
| P12 | `/ingest` references section | Lists all 5 relevant docs |
| P13 | `/ingest` limits | Raw content immutability, no Master creation, 1-3 page cap |
| P14 | `/lint` severity levels | ERROR/WARN/INFO — clear and actionable |
| P15 | `/promote` limits | Never sets status beyond `proposed`, human-only acceptance |
| P16 | `/reflect` advisory-only | Does not modify files |
| P17 | `/update` Master restriction | Does not modify Master pages |
| P18 | Git commit types | 5 types (capture/update/reflect/promote/maintenance) — CLAUDE.md ↔ Git-Convention.md aligned |
| P19 | `.gitignore` | Covers Obsidian workspace, OS files, temp files |
| P20 | Knowledge traceability chain | Capture → domain (captured_from) → master (sources) — chain defined in metadata-schema §5 |
| P21 | All domain indexes | Same 6-section structure (Concepts, Methods, Technologies, References, Entities, Sources) |
| P22 | All domain logs | Same table format, genesis entries present |
| P23 | Master schema quality rules | 3+ sources from 2+ domains — matches `/promote` requirements |
| P24 | All wikilink references use `[[page]]` syntax | Obsidian-compatible |

---

## 3. Problems Found

### Critical

#### C1 — raw-template.md uses `type` instead of `source_type`

**Location**: `templates/raw-template.md` line 4 vs `protocol/metadata-schema.md` §1

**Detail**: The metadata schema defines `source_type` as a required enum field (`web-clip`, `manual-note`, `pdf-export`, `ai-conversation`, `other`). The raw template uses `type: ""` — a different field name entirely. This means:

- Every capture created from the template will have a non-standard `type` field
- The required `source_type` field will be missing from every capture
- `/ingest` won't find `source_type` when it looks for it
- Any future metadata validation tool would flag every capture as invalid

**Fix**: Change `type: ""` to `source_type: ""` in `templates/raw-template.md` line 4. Add a comment listing allowed values.

---

#### C2 — FEDERATION.md authority table contradicts the ingestion pipeline

**Location**: `FEDERATION.md` lines 42-43 vs `CLAUDE.md` Raw Rules, `protocol/knowledge-lifecycle.md` §2, `.claude/commands/ingest.md` step 3

**Detail**: FEDERATION.md authority table states:

```
| Modify capture files | **Forbidden** | **Forbidden** |
```

This flatly prohibits both human and agent from modifying capture files under any circumstances. However:

- **CLAUDE.md Raw Rules** (lines 214-218): "Allowed: add metadata, add processing status, create references"
- **knowledge-lifecycle.md** (§2 raw→processed): "Agent sets `status: processed`, `processed_date`, and `ingested_to` links"
- **`/ingest`** step 3: explicitly instructs the agent to update capture frontmatter with `status: processed`, `processed_date`, `ingested_to`, `agent_version`
- **project-state.md**: documents this as a resolved design decision (agent updates frontmatter, never raw content)

The FEDERATION.md table was written for the general case but fails to distinguish between "modify raw content section" (truly forbidden) and "modify frontmatter metadata" (required for pipeline operation). Since FEDERATION.md is the authority table, any agent that reads it literally would refuse to run `/ingest` step 3.

**Fix**: Change the table row from one blanket entry to two rows:
```
| Modify capture raw content | **Forbidden** | **Forbidden** |
| Modify capture frontmatter | Approve | Execute (status, dates, links only) |
```

---

### High

#### H1 — Protocol lifecycle terminology conflicts with detailed lifecycle

**Location**: `protocol/LLM-Wiki-Federation-Protocol.md` lines 51-55 vs `protocol/knowledge-lifecycle.md` §1-2

**Detail**: Two different lifecycle vocabularies exist in the protocol layer:

**Federation Protocol** (4 macro-stages):
```
RAW INPUT → PROCESSED → CURATED → ABSTRACTED
(capture/)   (domain/)   (domain/)   (master/)
```

**Knowledge Lifecycle** (9 micro-stages):
```
raw → processed (capture layer)
seedling → growing → evergreen (domain layer)
proposed → accepted → active → superseded/rejected (master layer)
```

The conflict: "PROCESSED" in the federation protocol maps to domain layer, but "processed" in the detailed lifecycle is a capture-layer status. An agent reading both documents would be unsure whether "processed" means a capture file with updated frontmatter (lifecycle) or a Domain Wiki page that exists (federation protocol).

The 4 macro-stages are conceptually useful as an overview but use terminology that collides with the 9-stage field values agents actually operate on.

**Fix**: Either:
- (Recommended) Align the federation protocol's macro-stages to use distinct terms that don't collide with field values: `INGESTED → DOMAIN → CURATED → ABSTRACTED`, or
- Add a mapping table in the federation protocol showing how the 4 macro-stages decompose into the 9 micro-stages.

---

#### H2 — `/promote` hardcodes a single Master template

**Location**: `.claude/commands/promote.md` line 19

**Detail**: Step 3a reads:
> Write a proposed Master page using `templates/master-model-template.md`.

But Master Wiki has 4 categories, each with its own template:
- `templates/master-model-template.md` — mental models
- `templates/master-principle-template.md` — heuristics/decision rules
- `templates/master-concept-template.md` — abstract bridging ideas
- `templates/master-framework-template.md` — analysis frameworks

An agent following this command literally would always use the model template regardless of what kind of pattern it detected. A cross-domain principle would be shoehorned into a model template.

**Fix**: Change step 3a to:
> Write a proposed Master page using the appropriate template from `templates/`:
> - `master-model-template.md` for mental models
> - `master-principle-template.md` for principles/heuristics
> - `master-concept-template.md` for cross-domain concepts
> - `master-framework-template.md` for personal frameworks

---

### Medium

#### M1 — Domain templates missing `captured_from` and `ingested_by` placeholders

**Location**: All 5 domain templates vs `.claude/commands/ingest.md` step 2e

**Detail**: `/ingest` step 2e explicitly requires setting these fields:
- `captured_from`: relative path to capture file
- `ingested_by`: agent version string

But none of the 5 domain templates include these fields. Metadata-schema marks them as optional (NO), but since `/ingest` requires them for every ingested page, templates should include placeholders to ensure the agent doesn't forget.

**Affected files**:
- `templates/domain-concept-template.md`
- `templates/domain-method-template.md`
- `templates/domain-technology-template.md`
- `templates/domain-reference-template.md`
- `templates/domain-entity-template.md`

**Fix**: Add to each domain template's frontmatter:
```yaml
captured_from: ""
ingested_by: ""
```

---

#### M2 — `domain-reference-template.md` and `domain-entity-template.md` missing `sources` field

**Location**: `templates/domain-reference-template.md`, `templates/domain-entity-template.md`

**Detail**: The other 3 domain templates (concept, method, technology) all include `sources: []` in their frontmatter. Reference and entity templates omit it. While `sources` is optional per metadata-schema, its absence is inconsistent and means agents using these templates are less likely to add source traceability for references and entities.

**Fix**: Add `sources: []` to both templates' frontmatter.

---

#### M3 — `/update` doesn't reference knowledge-lifecycle.md or define status transition criteria

**Location**: `.claude/commands/update.md`

**Detail**: `/update` is the command that advances page maturity (seedling → growing), but it doesn't:
- Reference `protocol/knowledge-lifecycle.md`
- Specify criteria for seedling → growing transition (all sections filled, at least one incoming link, source capture referenced)
- Specify that growing → evergreen requires human approval
- Include any status-related steps

An agent running `/update` might refine content but never advance the status.

**Fix**: Add to `/update` step 2: "e. Evaluate page maturity against knowledge-lifecycle.md criteria. If criteria for next status are met, update `status` and note the transition in the domain log."

---

#### M4 — Domain `raw/` and `sources/` directories have unclear purpose

**Location**: `spaces/ai/raw/`, `spaces/ai/sources/`, `spaces/knowledge-management/raw/`, `spaces/knowledge-management/sources/`

**Detail**: Each domain schema's directory map lists:
```
raw/              # Processed captures (read-only)
sources/          # External reference materials
```

However:
- No command writes to domain `raw/`. All captures stay in `capture/inbox/`.
- No command writes to domain `sources/`. Domain indexes list `Sources` section but no protocol defines what goes in this directory vs what goes in wiki pages.
- The description "Processed captures (read-only)" is confusing — processed captures live in `capture/inbox/` with `status: processed`.

These directories exist with .gitkeep files but have no operational purpose in the current architecture. They represent undefined future functionality.

**Fix**: Either:
- Document their purpose in a protocol doc (what goes there, when, by whom), or
- Remove them if they serve no intended purpose (cleaner).

---

### Low

#### L1 — `/lint` doesn't reference metadata-schema.md

**Location**: `.claude/commands/lint.md`

**Detail**: `/lint` step 1d checks for "Missing metadata: pages without frontmatter, tags, or status." But the command doesn't reference `protocol/metadata-schema.md` to know which fields are required vs optional. An agent would need to know which fields to check.

**Fix**: Add `protocol/metadata-schema.md` to a references section in `/lint`.

---

#### L2 — No `archive/domains/` directory structure

**Location**: `archive/` directory

**Detail**: FEDERATION.md "Leaving the Federation" section says domains should be archived to `archive/domains/<domain>/`. However, only `archive/.gitkeep` exists — the `domains/` subdirectory hasn't been created. When a domain eventually needs archiving, the directory won't exist.

**Fix**: Create `archive/domains/.gitkeep` for forward compatibility.

---

#### L3 — Master template `tags` field inconsistent with domain templates

**Location**: All 4 master templates vs 5 domain templates

**Detail**: Domain templates all include `tags: []` in frontmatter. Master templates do not. Metadata-schema says `tags` is optional for Master pages, so this isn't technically wrong. But it's a minor inconsistency — if someone promotes a domain page (which has tags) to a master proposal, the tags field would be lost in the transition.

**Fix**: Add `tags: []` to all 4 master templates for consistency with domain templates.

---

#### L4 — LLM-Wiki-Federation-Protocol.md promotion threshold is inconsistent

**Location**: `protocol/LLM-Wiki-Federation-Protocol.md` line 79 vs `protocol/knowledge-lifecycle.md` line 123, `.claude/commands/promote.md` line 29

**Detail**: The federation protocol says a pattern needs "3+ Domain pages" to trigger promotion. But knowledge-lifecycle.md and `/promote` both say "3+ source pages from 2+ domains." The federation protocol's wording is looser — "3+ Domain pages" could be from a single domain, which would bypass the cross-domain requirement.

**Fix**: Change the federation protocol's "3+ Domain pages" to "3+ pages from 2+ domains" for consistency.

---

## 4. Severity Summary

| Severity | Count | IDs |
|----------|-------|-----|
| Critical | 2 | C1 (raw template field name), C2 (FEDERATION authority table) |
| High | 2 | H1 (lifecycle terminology conflict), H2 (promote template hardcode) |
| Medium | 4 | M1 (missing template fields), M2 (missing sources field), M3 (update no status logic), M4 (orphan domain dirs) |
| Low | 4 | L1 (lint missing ref), L2 (no archive/domains/), L3 (master tags missing), L4 (promotion threshold wording) |
| **Total** | **12** | |

---

## 5. Knowledge Flow Trace

Complete end-to-end trace of a capture through the system:

```
1. Human creates capture
   Template: templates/raw-template.md
   Fields: created, source, source_type, tags, status: raw        ← C1: template uses "type" not "source_type"
   Saved to: capture/inbox/<name>.md
   Status: raw

2. Agent runs /ingest
   Reads capture → finds source_type (but it's "type" in template) ← blocked by C1
   Routes domain via domain-routing.md → FEDERATION.md scopes
   Selects template via domain-routing §6 matrix
   Searches existing pages (CLAUDE.md Domain Wiki Rules)
   Creates page at spaces/<domain>/wiki/<type>/<name>.md
   Frontmatter needs: captured_from, ingested_by, sources         ← M1: templates miss these
   Updates domain index.md
   Updates domain log.md
   Marks capture: status: processed, ingested_to: [[...]]         ← C2: FEDERATION forbids this

3. Domain page growth (/update)
   Agent reviews, expands content
   Should advance: seedling → growing when criteria met           ← M3: /update doesn't define this
   Updates log.md

4. Curation (/lint)
   Checks: orphans, duplicates, stale, metadata, broken links
   Should validate against metadata-schema.md                     ← L1: /lint doesn't reference it
   Reports only, no auto-fix

5. Cross-domain promotion (/promote)
   Pattern found: 3+ pages from 2+ domains                        ← L4: federation protocol wording looser
   Creates Master proposal at spaces/master/wiki/<category>/
   Must use correct template (not always model)                   ← H2: command hardcodes model template
   status: proposed
   Updates master log.md
   Human decides: accepted / rejected

6. Master activation
   Human sets: status: accepted → active
   Page linked from master index.md
   Traceability: master.sources → domain page → capture.inbox
```

**Flow verdict**: Conceptually sound but has 2 blocking critical issues (C1, C2) that would cause errors on the very first `/ingest` execution.

---

## 6. Recommended Fix Priority

### Must fix before first `/ingest` (blockers):

| Priority | ID | Fix | Effort |
|----------|----|-----|--------|
| 1 | C1 | Rename `type:` to `source_type:` in raw-template.md | 1 line |
| 2 | C2 | Split FEDERATION.md capture row into raw content vs frontmatter | 2 rows |
| 3 | H2 | Add template selection logic to `/promote` step 3a | ~10 lines |

### Should fix before active use:

| Priority | ID | Fix | Effort |
|----------|----|-----|--------|
| 4 | M1 | Add `captured_from`, `ingested_by` to 5 domain templates | 10 lines |
| 5 | M2 | Add `sources: []` to reference and entity templates | 2 lines |
| 6 | M3 | Add status transition step to `/update` | ~5 lines |
| 7 | H1 | Align federation protocol lifecycle terminology | ~5 lines |

### Nice to fix:

| Priority | ID | Fix | Effort |
|----------|----|-----|--------|
| 8 | L1 | Add metadata-schema.md reference to `/lint` | 1 line |
| 9 | L2 | Create `archive/domains/.gitkeep` | 1 file |
| 10 | L3 | Add `tags: []` to 4 master templates | 4 lines |
| 11 | L4 | Tighten federation protocol promotion wording | 1 line |
| 12 | M4 | Document or remove domain `raw/` and `sources/` dirs | Decision needed |

---

## 7. Scalability Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Domain proliferation | Medium | Medium | 2 domains now; 5+ would make domain-routing.md boundary zones table grow O(n²). Mitigated by Primary Intent Rule but routing complexity increases with each new domain. |
| Master overgrowth | Low | High | Master has 4 categories, no content yet. Risk of treating Master as "interesting ideas" rather than "cross-domain abstractions." Mitigated by strict 3-sources-2-domains rule and human-only acceptance. |
| Protocol version drift | Medium | High | 5 protocol docs, 5 commands, 10 templates — all hand-aligned. One update can silently break consistency. Mitigated by periodic system integration audits (this document's purpose). |
| Template-schema divergence | Medium | Medium | Templates and metadata-schema.md define fields independently. Already diverged (C1). Will diverge again as schema evolves. Mitigated by adding schema field references to template comments. |
| Capture inbox backlog | Low | Low | Inbox is empty now but has no size limit or SLA documented. A backlog of 50+ unprocessed captures would block knowledge flow. Mitigated by daily maintenance workflow. |

---

## 8. Recommended Next Phase

The system passes structural validation (all directories, files, and documents exist) but fails operational readiness on 2 critical issues that block the first `/ingest` execution.

**Recommended phase**: **Pre-Flight Corrections**

Before beginning the Knowledge Activation phase (first capture → first ingest):

1. Fix the 2 critical issues (C1, C2) — these block `/ingest`
2. Fix the 1 high issue (H2) — this blocks `/promote`
3. Optionally fix H1 and M1-M3 — these would cause operational friction but aren't blockers

After corrections, the system is ready for:
1. Create first capture in `capture/inbox/`
2. Run `/ingest` end-to-end
3. Populate domains with initial wiki pages
4. Run `/lint` on real content
5. Detect cross-domain pattern → `/promote`

---

*Audit complete. 24 checks passed, 12 problems found. No files modified.*

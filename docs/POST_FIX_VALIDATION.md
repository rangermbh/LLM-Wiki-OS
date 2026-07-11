---
created: 2026-07-11
updated: 2026-07-11
status: complete
validation_type: post-fix
scope: C1, C2, H1, H2, M1, M2, M3
---

# Post-Fix Validation

Verification that all 7 pre-flight corrections were correctly applied and the system is consistent end-to-end.

**Date**: 2026-07-11
**Validator**: Claude Code (Wiki Maintainer Agent)
**Method**: Read every modified file, trace every reference, verify field names, lifecycle terms, and workflow completeness.

---

## 1. Fix Verification

### C1 — raw-template.md `type` → `source_type`

| Check | Result |
|-------|--------|
| Field renamed from `type` to `source_type` | PASS |
| Allowed-values comment present (`web-clip, manual-note, pdf-export, ai-conversation, other`) | PASS |
| Allowed values match metadata-schema.md §1 enum | PASS |
| Template still has all other required fields (created, source, tags, status) | PASS |

**File**: `templates/raw-template.md` line 4

---

### C2 — FEDERATION.md authority table split

| Check | Result |
|-------|--------|
| "Modify capture files" row removed | PASS |
| New row: "Modify capture raw content" — Forbidden / Forbidden | PASS |
| New row: "Modify capture frontmatter" — Approve / Execute (status, dates, links only) | PASS |
| Agent "Execute" scope matches `/ingest` step 3 actions (status, processed_date, ingested_to, agent_version) | PASS |
| Aligned with CLAUDE.md Raw Rules ("add metadata, add processing status, create references") | PASS |
| Aligned with knowledge-lifecycle.md raw→processed transition | PASS |

**File**: `FEDERATION.md` lines 43-44

---

### H2 — `/promote` template selection

| Check | Result |
|-------|--------|
| Hardcoded `templates/master-model-template.md` removed | PASS |
| All 4 Master templates listed with category descriptions | PASS |
| Categories match master schema.md (models, principles, concepts, frameworks) | PASS |
| Categories match master index.md sections | PASS |
| Step numbering preserved (3a, 3b, 3c, 3d intact) | PASS |

**File**: `.claude/commands/promote.md` lines 19-23

---

### H1 — Federation protocol lifecycle terminology

| Check | Result |
|-------|--------|
| `PROCESSED` → `INGESTED` in lifecycle diagram | PASS |
| "Processing (Domain raw → wiki)" → "Ingestion (Capture → Domain wiki)" | PASS |
| Macro-to-micro mapping note added (§4) | PASS |
| No collision: "processed" (capture status) vs "INGESTED" (macro-stage) are now distinct | PASS |
| Note explicitly references `protocol/knowledge-lifecycle.md` for detailed stages | PASS |

**File**: `protocol/LLM-Wiki-Federation-Protocol.md` lines 52-86

---

### M1 — `captured_from` and `ingested_by` in domain templates

| Template | `captured_from` | `ingested_by` | Result |
|----------|-----------------|---------------|--------|
| domain-concept-template.md | line 8 | line 9 | PASS |
| domain-method-template.md | line 8 | line 9 | PASS |
| domain-technology-template.md | line 8 | line 9 | PASS |
| domain-reference-template.md | line 13 | line 14 | PASS |
| domain-entity-template.md | line 10 | line 11 | PASS |

All 5 templates have both fields placed after `sources` in the frontmatter. ✓

---

### M2 — `sources` field in reference and entity templates

| Template | `sources: []` | Result |
|----------|---------------|--------|
| domain-reference-template.md | line 12 | PASS |
| domain-entity-template.md | line 9 | PASS |

Both now consistent with concept, method, and technology templates. ✓

---

### M3 — `/update` lifecycle maturity evaluation

| Check | Result |
|-------|--------|
| Step 2d added with `protocol/knowledge-lifecycle.md` reference | PASS |
| seedling → growing criteria listed (all sections filled, ≥1 incoming link, source capture referenced) | PASS |
| Criteria match knowledge-lifecycle.md seedling definition exactly | PASS |
| growing → evergreen: human approval required (do not auto-transition) | PASS |
| Matches knowledge-lifecycle.md authority table (growing→evergreen: Human only) | PASS |
| Step 3 updated to "including any status transitions" | PASS |

**File**: `.claude/commands/update.md` lines 18-24

---

## 2. Integration Stack Trace

Top-down consistency check through all 5 layers.

### Policy Layer

| Document | Status | Notes |
|----------|--------|-------|
| CLAUDE.md | OK | Constitution unchanged, all rules intact |
| FEDERATION.md | FIXED (C2) | Authority table now distinguishes raw content vs frontmatter |

### Protocol Layer

| Document | Status | Notes |
|----------|--------|-------|
| metadata-schema.md | OK | No changes needed |
| domain-routing.md | OK | No changes needed |
| knowledge-lifecycle.md | OK | No changes needed |
| LLM-Wiki-Federation-Protocol.md | FIXED (H1) | Lifecycle terms aligned, macro→micro mapping added |
| Git-Commit-Convention.md | OK | No changes needed |

### Template Layer

| Template | Status | Notes |
|----------|--------|-------|
| raw-template.md | FIXED (C1) | `source_type` field correct |
| domain-concept-template.md | FIXED (M1) | `captured_from`, `ingested_by` added |
| domain-method-template.md | FIXED (M1) | `captured_from`, `ingested_by` added |
| domain-technology-template.md | FIXED (M1) | `captured_from`, `ingested_by` added |
| domain-reference-template.md | FIXED (M1, M2) | `captured_from`, `ingested_by`, `sources` added |
| domain-entity-template.md | FIXED (M1, M2) | `captured_from`, `ingested_by`, `sources` added |
| master-model-template.md | OK | No changes needed |
| master-principle-template.md | OK | No changes needed |
| master-concept-template.md | OK | No changes needed |
| master-framework-template.md | OK | No changes needed |

### Command Layer

| Command | Status | Notes |
|---------|--------|-------|
| `/ingest` | OK | References all correct protocol docs and templates |
| `/update` | FIXED (M3) | Now includes lifecycle maturity evaluation |
| `/promote` | FIXED (H2) | Now selects from all 4 Master templates |
| `/lint` | OK | No changes needed (L1 not in scope) |
| `/reflect` | OK | No changes needed |

### Filesystem Layer

| Layer | Status | Notes |
|-------|--------|-------|
| Capture (`capture/`) | OK | inbox + attachments, .gitkeep intact |
| Domain AI (`spaces/ai/`) | OK | schema, index, log, 5 wiki dirs, raw, sources |
| Domain KM (`spaces/knowledge-management/`) | OK | schema, index, log, 5 wiki dirs, raw, sources |
| Master (`spaces/master/`) | OK | schema, index, log, 4 wiki dirs |
| Archive (`archive/`) | OK | .gitkeep present (L2 not in scope) |

---

## 3. Metadata Field Consistency Matrix

### Capture Layer

| Field | metadata-schema | raw-template | /ingest writes |
|-------|-----------------|--------------|----------------|
| `created` | Required | Present | — |
| `source` | Required | Present | — |
| `source_type` | Required | **Present (was broken)** | — |
| `tags` | Optional | Present | — |
| `status` | Required | Present (`raw`) | → `processed` |
| `processed_date` | Optional | — | Sets |
| `ingested_to` | Optional | — | Sets |
| `agent_version` | Optional | — | Sets |

**Result**: All fields consistent. ✓

### Domain Wiki Layer

| Field | metadata-schema | All 5 templates | /ingest writes |
|-------|-----------------|-----------------|----------------|
| `created` | Required | Present | Sets |
| `updated` | Required | Present | Sets |
| `domain` | Required | Present | Sets |
| `tags` | Optional | Present | Sets |
| `status` | Required | Present (`seedling`) | Sets |
| `sources` | Optional | **Present in all 5 (was missing in 2)** | Sets |
| `captured_from` | Optional | **Present in all 5 (was missing in all)** | Sets |
| `ingested_by` | Optional | **Present in all 5 (was missing in all)** | Sets |

**Result**: All fields consistent across schema, templates, and command. ✓

---

## 4. Lifecycle Terminology Consistency

### Macro → Micro Mapping

| Federation Protocol | knowledge-lifecycle.md | metadata-schema.md status values | Layer |
|---------------------|------------------------|----------------------------------|-------|
| RAW INPUT | raw | `raw` | capture |
| INGESTED | processed (capture) + seedling (domain) | `processed`, `seedling` | capture → domain |
| CURATED | growing + evergreen | `growing`, `evergreen` | domain |
| ABSTRACTED | proposed + accepted + active | `proposed`, `accepted`, `active` | master |
| — | superseded, rejected | `superseded`, `rejected` | terminal |

**Previously**: "PROCESSED" in the federation protocol collided with "processed" (capture status).
**Now**: "INGESTED" is distinct. The mapping note in federation protocol §4 explicitly documents the decomposition.

**Result**: No terminology collisions. ✓

---

## 5. Workflow Completeness

### `/ingest` End-to-End

| Step | Description | Status |
|------|-------------|--------|
| 1 | List captures with `status: raw` | OK |
| 2a | Read raw content | OK |
| 2b | Route domain via domain-routing.md → FEDERATION.md | OK |
| 2c | Determine content type via matrix | OK |
| 2d | Search existing pages (update over create) | OK |
| 2e | Create wiki page — template has all needed fields | **WAS BROKEN (M1), NOW OK** |
| 2e | Fill frontmatter — `source_type` field exists in capture | **WAS BROKEN (C1), NOW OK** |
| 2f | Update domain index | OK |
| 2g | Update domain log | OK |
| 3 | Mark capture processed — authority model allows this | **WAS BROKEN (C2), NOW OK** |
| 4 | Report summary | OK |

**Result**: All blocking issues resolved. Ingest workflow is complete. ✓

### `/promote` End-to-End

| Step | Description | Status |
|------|-------------|--------|
| 1 | Scan 2+ domains | OK |
| 2 | Identify 3+ page patterns | OK |
| 3a | Write Master page with appropriate template | **WAS HARDCODED (H2), NOW CORRECT** |
| 3b | Place in correct category dir | OK |
| 3c | Set `status: proposed` | OK |
| 3d | Link to source pages | OK |
| 4 | Update master log | OK |
| 5 | Present to human | OK |

**Result**: Promote now supports all 4 Master categories. ✓

### `/update` End-to-End

| Step | Description | Status |
|------|-------------|--------|
| 1 | Identify target pages | OK |
| 2a | Read content | OK |
| 2b | Check sources | OK |
| 2c | Check stale content | OK |
| 2d | Evaluate lifecycle maturity | **WAS MISSING (M3), NOW PRESENT** |
| 2e | Update `updated` date | OK |
| 2f | Add to domain log | OK |
| 3 | Report changes + status transitions | OK |

**Result**: Update now drives knowledge maturity. ✓

---

## 6. Remaining Risks

Issues from the original audit that were not in scope for this fix round:

| ID | Risk | Severity | Impact on first `/ingest` |
|----|------|----------|---------------------------|
| L1 | `/lint` doesn't reference metadata-schema.md | Low | None — lint is a separate command |
| L2 | No `archive/domains/` directory | Low | None — no domains to archive yet |
| L3 | Master templates missing `tags` field | Low | None — tags is optional per schema |
| L4 | Federation protocol says "3+ Domain pages" (should say "2+ domains") | Low | Minor wording, `/promote` has the correct criteria |
| M4 | Domain `raw/` and `sources/` directories unused | Low | None — no operational impact |

None of these block the first `/ingest`. They are minor polish items.

### No Regressions Detected

| Check | Result |
|-------|--------|
| All modified files still have valid YAML frontmatter | PASS |
| No broken wikilinks introduced | PASS |
| No step numbering broken in commands | PASS |
| No field name conflicts introduced | PASS |
| No file renames (all edits were in-place) | PASS |

---

## 7. Ready / Not Ready Decision

### First `/ingest` Readiness

The 2 critical issues (C1, C2) and the high issue (H2) that blocked the first `/ingest` are fixed and verified.

**Verdict**: **READY** for first `/ingest` execution.

Preconditions for first `/ingest`:
1. A capture file exists in `capture/inbox/` with valid frontmatter (minimum: `created`, `source`, `source_type`, `status: raw`) — **not yet created, requires human action**
2. Agent has read access to all protocol docs and templates — **verified**
3. Authority model permits frontmatter updates — **verified (C2)**
4. Templates have all fields `/ingest` needs to set — **verified (M1, M2)**
5. Domain routing rules can match capture content to a domain — **verified (AI + KM domains active)**

### System State

| Metric | Value |
|--------|-------|
| Files modified in this fix round | 10 |
| Critical issues resolved | 2 (C1, C2) |
| High issues resolved | 2 (H1, H2) |
| Medium issues resolved | 3 (M1, M2, M3) |
| Issues remaining (not in scope) | 5 (L1-L4, M4) |
| System ready for Knowledge Activation | **YES** |

---

*Validation complete. No files modified during validation.*

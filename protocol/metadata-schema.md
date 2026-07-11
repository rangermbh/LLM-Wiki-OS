# Metadata Schema

This document defines all metadata fields, their types, allowed values, and layer-specific requirements.

Version: 1.0.0

---

## 1. Raw Capture Metadata

Frontmatter for files in `capture/inbox/`.

| Field | Type | Required | Allowed Values | Description |
|-------|------|----------|---------------|-------------|
| `created` | date | YES | `YYYY-MM-DD` | Date the capture was created |
| `source` | string | YES | Free text or URL | Where this came from |
| `source_type` | enum | YES | `web-clip`, `manual-note`, `pdf-export`, `ai-conversation`, `other` | Category of source |
| `tags` | array | NO | `[tag1, tag2]` | Free-form tags |
| `status` | enum | YES | `raw`, `processed` | Processing state |
| `processed_date` | date | NO | `YYYY-MM-DD` | When ingestion occurred |
| `ingested_to` | array | NO | `[[domain/page]]` | Wikilinks to resulting wiki pages |
| `agent_version` | string | NO | e.g., `claude-opus-4.7` | Which agent processed this |

### Status Transitions

```
raw  →  processed
```

- `raw`: Not yet ingested. Default for new captures.
- `processed`: Has been ingested into at least one Domain Wiki page. Set by agent after `/ingest`.

---

## 2. Domain Wiki Metadata

Common frontmatter for all pages under `spaces/<domain>/wiki/`.

### Common Fields (All Domain Pages)

| Field | Type | Required | Allowed Values | Description |
|-------|------|----------|---------------|-------------|
| `created` | date | YES | `YYYY-MM-DD` | Date page was created |
| `updated` | date | YES | `YYYY-MM-DD` | Date of last substantive edit |
| `domain` | string | YES | Registered domain name | Which domain this belongs to |
| `tags` | array | NO | `[tag1, tag2]` | Free-form tags, prefer lowercase-kebab |
| `status` | enum | YES | `seedling`, `growing`, `evergreen`, `superseded` | Knowledge maturity |
| `sources` | array | NO | `[[capture/file]]` or URLs | Source captures or external references |
| `captured_from` | string | NO | `capture/inbox/filename.md` | Trace back to original capture |
| `ingested_by` | string | NO | e.g., `claude-opus-4.7` | Which agent created this page |

### Type-Specific Fields

#### Concept (`domain-concept-template.md`)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| (no additional fields) | — | — | Common fields sufficient |

#### Method (`domain-method-template.md`)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| (no additional fields) | — | — | Common fields sufficient |

#### Technology (`domain-technology-template.md`)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| (no additional fields) | — | — | Common fields sufficient |

#### Reference (`domain-reference-template.md`)

| Field | Type | Required | Allowed Values | Description |
|-------|------|----------|---------------|-------------|
| `type` | enum | YES | `reference` | Fixed value |
| `reference_type` | enum | YES | `paper`, `book`, `article`, `talk`, `course` | Category |
| `authors` | array | NO | `[name1, name2]` | Author names |
| `year` | string | NO | e.g., `2024` | Publication year |
| `url` | string | NO | Valid URL | Link to source |

#### Entity (`domain-entity-template.md`)

| Field | Type | Required | Allowed Values | Description |
|-------|------|----------|---------------|-------------|
| `type` | enum | YES | `entity` | Fixed value |
| `entity_type` | enum | YES | `person`, `organization`, `project`, `event` | Category |

---

## 3. Master Wiki Metadata

Common frontmatter for all pages under `spaces/master/wiki/`.

### Common Fields (All Master Pages)

| Field | Type | Required | Allowed Values | Description |
|-------|------|----------|---------------|-------------|
| `created` | date | YES | `YYYY-MM-DD` | Date proposed |
| `updated` | date | YES | `YYYY-MM-DD` | Date last modified |
| `type` | enum | YES | `model`, `principle`, `concept`, `framework` | Master category |
| `sources` | array | YES | `[[domain/page]]` | Domain wiki pages that support this |
| `domains` | array | YES | `[domain-a, domain-b]` | Which domains contributed evidence |
| `status` | enum | YES | `proposed`, `accepted`, `active`, `superseded`, `rejected` | Approval state |
| `tags` | array | NO | `[tag1, tag2]` | Free-form tags |

### Type-Specific Fields

No additional fields. Master pages are distinguished by `type` and directory placement.

### Status Transitions

```
proposed  →  accepted  →  active  →  superseded
                ↓
             rejected
```

- `proposed`: Agent suggestion, awaiting human review. Agent sets this.
- `accepted`: Human approved, not yet active. Human sets this.
- `active`: Currently part of the personal world model. Human sets this.
- `superseded`: Replaced by a newer version. Moved to `archive/`. Human sets this.
- `rejected`: Human declined. Human sets this.

---

## 4. Tag Naming Convention

Recommended format: `lowercase-kebab-case`

| Good | Avoid |
|------|-------|
| `large-language-models` | `Large Language Models`, `LLMs` (ambiguous) |
| `prompt-engineering` | `PromptEngineering`, `prompt_engineering` |
| `transformer-architecture` | `Transformer`, `transformer` (too generic) |

Tags should be:
- Specific enough to be useful for filtering
- General enough to group related pages
- Consistent across all domains (same concept = same tag)

---

## 5. Cross-Layer Traceability

```
capture/inbox/article.md
  │  captured_from → spaces/ai/wiki/concepts/transformer-architecture.md
  │
  └── ingested_to → spaces/ai/wiki/concepts/transformer-architecture.md
                    │  sources → capture/inbox/article.md
                    │
                    └── master sources → spaces/ai/wiki/concepts/transformer-architecture.md
```

Every Domain page should trace back to its capture.
Every Master page should trace back to its Domain sources.

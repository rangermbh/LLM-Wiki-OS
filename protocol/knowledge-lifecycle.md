# Knowledge Lifecycle

This document defines the complete lifecycle of knowledge as it moves through the LLM Wiki Federation, from raw capture to Master Wiki entry.

Version: 1.0.0

---

## 1. Lifecycle Stages

```
                           CAPTURE LAYER
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
                 capture               capture
               (inbox/raw)         (inbox/processed)
                    │                     │
                    └──────────┬──────────┘
                               │ /ingest
                               ▼
                         DOMAIN LAYER
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
                seedling              growing
                    │                     │
                    └──────────┬──────────┘
                               │ curation
                               ▼
                            evergreen
                               │
                               │ /promote
                               ▼
                          MASTER LAYER
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
                proposed              accepted
                    │                     │
                    └──────────┬──────────┘
                               │ human decision
                               ▼
                             active
```

## 2. Stage Definitions

### Capture Layer

#### raw
- **Location**: `capture/inbox/`
- **Description**: Unprocessed external input. Original content preserved exactly as received.
- **Who sets it**: Human (creates the file).
- **Transition to**: `processed` when `/ingest` has extracted knowledge into Domain Wiki.
- **Rules**: Never modify the raw content section. Metadata may be added.

#### processed
- **Location**: `capture/inbox/` (same file, status changed)
- **Description**: Capture has been ingested. Wiki pages created. Metadata updated with `processed_date` and `ingested_to` links.
- **Who sets it**: Agent (during `/ingest`).
- **Transition to**: None. File remains in inbox as immutable record.
- **Rules**: `status` field changed from `raw` to `processed`. `ingested_to` links added.

---

### Domain Layer

#### seedling
- **Location**: `spaces/<domain>/wiki/<type>/`
- **Description**: Newly created page from ingestion. Basic structure filled. May be incomplete.
- **Frontmatter**: `status: seedling`
- **Who sets it**: Agent (during `/ingest`).
- **Transition to**: `growing` when the page has been reviewed, expanded, and linked by the human or agent.
- **Criteria**:
  - All template sections filled (no `<!-- comment -->` placeholders)
  - At least one incoming link from index or another page
  - Source capture referenced

#### growing
- **Location**: Same as seedling.
- **Description**: Actively developed page. Content is being refined, linked, and tested against other knowledge.
- **Frontmatter**: `status: growing`
- **Who sets it**: Human or Agent (during `/update`).
- **Transition to**: `evergreen` when content has stabilized and needs only occasional updates.
- **Criteria**:
  - Content is comprehensive for its scope
  - Multiple incoming and outgoing wikilinks
  - Has been reviewed at least once
  - No known contradictions with other pages

#### evergreen
- **Location**: Same as seedling.
- **Description**: Mature, stable knowledge. Rarely needs substantive changes. The gold standard for Domain Wiki pages.
- **Frontmatter**: `status: evergreen`
- **Who sets it**: Human (approval required).
- **Transition to**: `superseded` if a newer page replaces it.
- **Criteria**:
  - Content has been stable for 3+ months
  - Linked from index and multiple related pages
  - No open questions or TODOs
  - Validated against real use

#### superseded
- **Location**: `archive/domains/<domain>/`
- **Description**: Replaced by newer knowledge. Moved to archive, not deleted.
- **Frontmatter**: `status: superseded`
- **Who sets it**: Human (approval required).
- **Transition to**: None. Terminal state.
- **Rules**: File moved to `archive/`. Index updated. Original location gets a redirect note.

---

### Master Layer

#### proposed
- **Location**: `spaces/master/wiki/<category>/`
- **Description**: Agent has detected a cross-domain pattern and proposed a Master entry.
- **Frontmatter**: `status: proposed`
- **Who sets it**: Agent (via `/promote`).
- **Transition to**: `accepted` or `rejected` by human decision.
- **Criteria**:
  - 3+ source pages from 2+ different domains
  - Cross-domain pattern clearly articulated
  - Links back to all source pages

#### accepted
- **Location**: Same as proposed.
- **Description**: Human has reviewed and approved the proposal. Not yet fully integrated into world model.
- **Frontmatter**: `status: accepted`
- **Who sets it**: Human.
- **Transition to**: `active` when fully integrated into thinking/practice.

#### active
- **Location**: Same as proposed.
- **Description**: Fully integrated into personal world model. Actively used in thinking and decision-making.
- **Frontmatter**: `status: active`
- **Who sets it**: Human.
- **Transition to**: `superseded` if replaced by a refined version.

#### rejected
- **Location**: Same as proposed (kept for record).
- **Description**: Human declined the proposal. Kept as a record of what was considered.
- **Frontmatter**: `status: rejected`
- **Who sets it**: Human.
- **Transition to**: None. Terminal state.

---

## 3. Lifecycle Events and Commands

| Event | Trigger | Command | Agent Action |
|-------|---------|---------|--------------|
| New capture arrives | Human drops file in inbox | — | Wait for human to invoke `/ingest` |
| Capture processed | Human runs `/ingest` | `/ingest` | Read capture → Route to domain → Select template → Create page → Update index + log → Mark capture processed |
| Page needs update | Scheduled or human request | `/update` | Read page → Check sources → Refresh content → Update `updated` date → Log change |
| Quality review | Scheduled or human request | `/lint` | Scan all pages → Check structural/knowledge/federation → Generate report |
| Pattern detected | During `/reflect` or `/update` | `/promote` | Identify cross-domain pattern → Write Master proposal → Set `status: proposed` → Present to human |
| System health review | Weekly schedule | `/reflect` | Review metrics → Check integrity → Identify improvements → Generate report |

---

## 4. Status Change Authority

| Transition | Authority |
|------------|-----------|
| `raw` → `processed` | Agent (automatic during `/ingest`) |
| `seedling` → `growing` | Agent or Human |
| `growing` → `evergreen` | Human only |
| `evergreen` → `superseded` | Human only |
| `proposed` → `accepted` | Human only |
| `accepted` → `active` | Human only |
| Any → `superseded` (Master) | Human only |
| `proposed` → `rejected` | Human only |

---

## 5. Knowledge Maturity Indicators

When evaluating a page's maturity, consider:

| Dimension | seedling | growing | evergreen |
|-----------|----------|---------|-----------|
| Completeness | Template sections may be empty | All sections filled | Comprehensive |
| Connectivity | May be orphaned | 1-3 links | Well-linked (>3) |
| Stability | May change significantly | Under active refinement | Stable for months |
| Validation | Untested | Partially validated | Battle-tested |
| Confidence | Tentative | Moderate | High |

---

*Protocol version 1.0.0, aligned with CLAUDE.md Knowledge Lifecycle (lines 173-202)*

---
created: 2026-07-11
type: readiness-assessment
scope: capture-to-ingestion pipeline
---

# Knowledge Ingestion Readiness Report

## Executive Summary

| Checkpoint | Status |
|------------|--------|
| Capture directories | READY |
| Raw template | READY |
| Domain templates | READY (5) |
| Ingest command | NEEDS UPDATE |
| Domain routing rules | MISSING |
| Processing status tracking | MISSING |
| Metadata schema | IMPLICIT ONLY |
| **Overall** | **CONDITIONALLY READY** |

The pipeline infrastructure is in place. Real captures can be ingested. However, the agent must handle 4 gaps during execution: template selection logic, domain routing, processing status, and metadata completeness.

---

## 1. Capture Directory Structure

### Current State

```
capture/
├── inbox/           # Raw markdown captures
│   └── .gitkeep     # (empty, waiting for first capture)
└── attachments/     # Binary files (PDFs, images, etc.)
    └── .gitkeep     # (empty, waiting for first attachment)
```

### Assessment

| Check | Result |
|-------|--------|
| Inbox exists and writable | PASS |
| Attachments directory exists | PASS |
| Separated concerns (text vs binary) | PASS |
| .gitkeep protection | PASS |
| Example capture file present | FAIL — empty, no guidance for first-time user |
| README or instruction file | FAIL — no documentation on what format to use |

### Recommendation
Create `capture/inbox/README.md` with a 3-sentence guide: what goes here, what format to use, what happens next. This is a UX gap for the human.

---

## 2. Ingest Command Analysis

### Current Definition

File: `.claude/commands/ingest.md` (29 lines)

**Execution Steps:**
1. List all files in `capture/inbox/`.
2. For each file:
   a. Read the raw content.
   b. Identify which Domain(s) it belongs to.
   c. Create a new wiki page using "the domain-concept template."
   d. If no suitable Domain exists, propose creating one.
   e. Update the Domain's `index.md`.
   f. Update the Domain's `log.md`.
3. Report what was created and where.
4. Do NOT modify or delete the original capture file.

### Gaps Identified

| # | Gap | Impact |
|---|-----|--------|
| I1 | Step 2c says "domain-concept template" (singular) | There are now 5 domain templates: concept, method, technology, reference, entity. The agent must choose the right one per capture. No selection logic is specified. |
| I2 | Step 2b ("Identify which Domain") has no criteria | The agent must decide AI vs Knowledge-Management with no routing rules. Relies entirely on agent judgment call per capture. |
| I3 | Step 4 says "Do NOT modify the original capture file" | Correct by CLAUDE.md Raw Rules. But with no processing-status mechanism, the human cannot visually distinguish processed from unprocessed captures in inbox. |
| I4 | No multi-domain handling | A capture about "LLM-assisted knowledge management" spans both AI and Knowledge-Management domains. The command has no logic for this case. |
| I5 | No content type detection | A capture might contain a method description, a technology review, or a concept definition. The command doesn't guide the agent on how to classify content into the 5 knowledge types. |

### What Works
- Core flow (read → classify → create → index → log → report) is sound
- Immutability protection is correct
- Index/log update requirement ensures audit trail
- "Propose new domain" escape hatch exists for unknown topics

---

## 3. Template Analysis

### Raw Template (`templates/raw-template.md`)

```yaml
frontmatter:
  created: YYYY-MM-DD       # OK
  source: ""                # OK — URL, book, conversation
  type: ""                  # AMBIGUOUS — what values? article, webclip, pdf, conversation?
  tags: []                  # OK
  status: raw               # OK — but no transition to "processed"
```

Sections: Title, Source, Raw Content, Initial Impressions

**Gaps:**
- `type` field has no defined vocabulary
- No `processed_date` or `processed_by` field
- No link field to the resulting wiki page(s) after ingestion

### Domain Templates (5 files)

| Template | Frontmatter | Sections |
|----------|-------------|----------|
| `domain-concept-template.md` | created, updated, domain, tags, status: seedling, sources | Definition, Key Points, Relationships, Sources, Notes |
| `domain-method-template.md` | created, updated, domain, tags, status: seedling, sources | Purpose, Steps, When to Use, When NOT to Use, Related Concepts, Sources |
| `domain-technology-template.md` | created, updated, domain, tags, status: seedling, sources | What It Is, Purpose, Key Features, Ecosystem, When to Use, When NOT to Use, Source |
| `domain-reference-template.md` | created, updated, domain, type, reference_type, authors, year, url, tags, status: seedling | Citation, Summary, Key Insights, Relevance, Related Concepts |
| `domain-entity-template.md` | created, updated, domain, type, entity_type, tags, status: seedling | Type, Significance, Key Contributions/Role, Relationships, External Links |

**Common frontmatter across all 5:** `created`, `updated`, `domain`, `tags`, `status`
**Status vocabulary:** `seedling` (all templates default to this)

**Gaps across all domain templates:**
- No `captured_from` field to trace back to the source capture file
- No `ingested_by` field to record which agent version processed it
- Status vocabulary is undefined — seedling → what? budding? evergreen? mature?

### Content Type Selection Matrix (Implicit)

When the agent reads a capture, it must decide which template to use:

| Capture contains... | Use template |
|---------------------|--------------|
| A definition, idea, or theory | `domain-concept-template.md` |
| A procedure, technique, or algorithm | `domain-method-template.md` |
| A tool, framework, or platform review | `domain-technology-template.md` |
| A paper, book, or article summary | `domain-reference-template.md` |
| Information about a person, org, or project | `domain-entity-template.md` |

This matrix exists only by implication from template names. It is not documented anywhere.

---

## 4. Metadata Requirements

### Required vs Optional (Current State)

No metadata schema document exists. The following is derived by reading all templates:

| Field | Raw | Domain | Notes |
|-------|-----|--------|-------|
| `created` | Required | Required | Date format: YYYY-MM-DD |
| `updated` | — | Required | Same format |
| `source` | Optional | — | Free text, no format |
| `type` | Optional | — | No defined vocabulary |
| `tags` | Optional | Optional | Array, no naming convention |
| `status` | Required (raw) | Required (seedling) | No lifecycle definition |
| `domain` | — | Required | Must match a registered domain |
| `sources` | — | Optional | Array, no format |
| `reference_type` | — | Optional (reference only) | paper, book, article, talk, course |
| `entity_type` | — | Optional (entity only) | person, organization, project, event |
| `authors` | — | Optional (reference only) | Array |
| `year` | — | Optional (reference only) | String |
| `url` | — | Optional (reference only) | String |

### Gap

No document defines which fields are mandatory, which are optional, or what vocabulary each field accepts. The agent must infer this from template examples.

---

## 5. Domain Routing Rules

### Current State

No routing rules document exists. The agent must decide domain assignment during `/ingest` step 2b based on:

1. Reading the capture content
2. Matching against domain schemas in `FEDERATION.md`
3. Agent judgment

### Registered Domains

| Domain | Scope Keywords |
|--------|---------------|
| AI | ML, DL, NLP, CV, RL, LLMs, AI alignment, AI safety, AI tools |
| Knowledge Management | PKM, Obsidian, LLM-assisted knowledge, information organization, note-taking, knowledge graph, digital garden |

### Routing Scenarios

| Capture Topic | Route To | Confidence |
|---------------|----------|------------|
| Transformer architecture explained | AI → concepts | Clear |
| How to set up Obsidian Dataview | Knowledge-Management → technologies | Clear |
| LLM-assisted note summarization | BOTH — AI + Knowledge-Management | Ambiguous |
| A book review of "Thinking, Fast and Slow" | Neither domain fits | Orphan — propose new domain? |
| Personal reflection on learning systems | Unclear | Ask human |

### Gap

No routing rules for:
1. **Multi-domain captures** — create pages in both domains? Create in one and cross-link?
2. **No-match captures** — at what threshold does the agent propose a new domain vs. ask the human?
3. **Domain overlap areas** — "LLM-assisted knowledge" appears in BOTH AI and Knowledge-Management scopes. Which domain owns it?

---

## 6. Processing Status Tracking

### Current State

- Raw template has `status: raw`
- `/ingest` step 4: "Do NOT modify or delete the original capture file"
- CLAUDE.md Raw Rules: "Allowed: add processing status"

There is a contradiction: CLAUDE.md allows adding processing status to raw files, but `/ingest` says "Do NOT modify the original capture file." These need reconciliation.

### Options

| Option | Description | Trade-off |
|--------|-------------|-----------|
| A. Add `processed: YYYY-MM-DD` to raw frontmatter | Update raw file's YAML after ingestion | Simple, traceable. Contradicts `/ingest` step 4 but allowed by CLAUDE.md Raw Rules. |
| B. Create `.ingested` marker alongside raw file | Empty marker file `capture.md.ingested` | No modification to raw file. Less elegant. |
| C. Record in domain log only | The log.md entry IS the processing record | No visible marker in inbox. Human must check logs. |
| D. Agent moves processed capture to `spaces/<domain>/raw/` | Relocate file after processing | Violates "delete source material" rule. Inbox loses original copy. |

**Recommended: Option A.** It is explicitly permitted by CLAUDE.md line 215-217 ("add processing status") and the `/ingest` command should be updated to reflect this.

---

## 7. Summary of Gaps Before First Ingestion

| # | Gap | Blocking? | Fix |
|---|-----|-----------|-----|
| G1 | `/ingest` references single template | SOFT | Agent uses implicit content-type matrix during execution; command text can be updated later |
| G2 | No domain routing rules | SOFT | Agent judges per capture; first few captures will establish precedent |
| G3 | No processing status mechanism | SOFT | Agent adds `processed` field to raw frontmatter (allowed by CLAUDE.md) |
| G4 | No metadata schema document | NO | Templates provide sufficient implicit guidance |
| G5 | No multi-domain capture handling | SOFT | First occurrence will establish the pattern |
| G6 | `capture/inbox/` has no README | NO | UX gap, not functional gap |
| G7 | `/ingest` step 4 vs CLAUDE.md Raw Rules conflict | SOFT | Agent follows CLAUDE.md (higher authority): adding processing status IS allowed |
| G8 | No content type vocabulary for `raw-template` `type` field | NO | Can be standardized later |

**Verdict**: The system can begin ingesting real captures immediately. The agent can compensate for all gaps during execution. No fix blocks the first `/ingest` run.

---

## 8. Recommended Pre-Flight Checklist

Before first ingestion, the human should:

1. **Drop a capture file** into `capture/inbox/` — any format: web clip, manual note, AI conversation export
2. **Name it descriptively** — e.g., `webclip-transformer-paper.md`, `manual-pkm-workflow-notes.md`
3. **Add basic frontmatter** — at minimum: `created`, `source`
4. **Run `/ingest`** — agent will read, route, template, create, and report
5. **Review the result** — check the created wiki page, index update, and log entry

---

*Report generated by Wiki Maintainer Agent, 2026-07-11*

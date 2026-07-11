# /ingest — Process Capture Inbox

## Purpose

Process raw items in `capture/inbox/` and extract structured knowledge into the appropriate Domain Wiki.

## Input

- One or more files in `capture/inbox/` with `status: raw`

## Prerequisites

- At least one capture file in `capture/inbox/` with YAML frontmatter (minimum: `created`, `source`, `source_type`, `status: raw`)
- See `protocol/metadata-schema.md` for full frontmatter specification
- See `templates/raw-template.md` for the capture template

## Execution Steps

### Flow

```
Capture → Routing → Distillation → Concept Creation
```

### Step 1: List captures
List all files in `capture/inbox/` with `status: raw`. Skip files already marked `status: processed`.

### Step 2: For each unprocessed capture

#### a. Read raw content
Read the full file. Identify the capture's frontmatter and raw content section.

#### b. Determine domain
Route to the correct Domain(s) following `protocol/domain-routing.md`:

1. Check capture content against registered Domain scopes in `FEDERATION.md`.
2. If the topic matches one domain → route there.
3. If the topic falls in a boundary zone → apply the Primary Intent Rule (domain-routing.md §4).
4. If the topic spans 2+ domains → apply Multi-Domain Handling (domain-routing.md §3).
5. If no domain matches → apply Unknown Domain Handling (domain-routing.md §5).

#### c. Determine content type
Select the appropriate template based on content characteristics (domain-routing.md §6):

| Content Characteristic | Template |
|------------------------|----------|
| Defines or explains an idea or theory | `templates/domain-concept-template.md` |
| Describes a procedure, technique, or workflow | `templates/domain-method-template.md` |
| Reviews or documents a tool, framework, or platform | `templates/domain-technology-template.md` |
| Summarizes a paper, book, article, or talk | `templates/domain-reference-template.md` |
| Describes a person, organization, project, or event | `templates/domain-entity-template.md` |

If a capture contains multiple content types, use the dominant type and note secondary aspects in the page's Notes section.

#### d. Search for existing knowledge
Before creating a new page, search the target Domain's `wiki/` directory for:
1. A page that already covers this concept (→ update it instead)
2. A page that partially overlaps (→ create new page, cross-link both)

Prefer updating existing pages over creating new ones (CLAUDE.md Domain Wiki Rules).

#### e. Distill knowledge
Apply Knowledge Distillation following `protocol/knowledge-distillation.md`:

1. **Identify the concept**: What is the ONE concept this capture is about?
2. **Extract core mechanism**: Internal logic, architecture, process. How does it work?
3. **Identify relationships**: What existing wiki concepts does this connect to?
4. **Identify boundaries**: What does this concept NOT explain? Where does it break down?
5. **Synthesize**: Write the Concept Page as the knowledge system's **understanding** of the concept — not as a summary of the source article.

Before proceeding to create the page, verify the quality checklist:
- [ ] Definition situates the concept in the domain (one sentence)
- [ ] Core Idea can be understood without reading the source
- [ ] Mechanism is specific enough to distinguish from adjacent concepts
- [ ] Relationships explain connection types, not just list names
- [ ] Limitations section will be present and substantive
- [ ] No marketing language or vendor enthusiasm
- [ ] Page structure does not follow source article structure

#### f. Resolve wikilinks
Before writing the wiki page, resolve all wikilinks following `protocol/knowledge-linking.md`:

1. For each concept referenced in the page content, search existing wiki pages across all domains.
2. Use explicit path links (e.g., `[[spaces/ai/wiki/concepts/rag|RAG]]`) instead of bare links (e.g., `[[RAG]]`).
3. For same-domain same-type links, use relative paths (e.g., `[[target|Display]]`).
4. For cross-domain links, use full paths from the vault root.
5. If a target concept does not exist yet, do NOT create a bare link. Instead:
   - When creating the page in step g, add a TODO note in its Notes section (e.g., `TODO: future link to [[spaces/ai/wiki/concepts/rag|RAG]]`).
   - Optionally add a `<!-- planned: [[path|Concept]] -->` comment in the domain index.
6. Verify every `[[link]]` resolves to an existing file or is intentionally documented as planned.

Bare wikilinks (e.g., `[[RAG]]`) create orphan files in the Vault root and are forbidden.

#### g. Create wiki page
1. Copy the selected template.
2. Fill in frontmatter:
   - `created`: today's date
   - `updated`: today's date
   - `domain`: the routed domain name
   - `tags`: 2-5 relevant tags in `lowercase-kebab-case` format
   - `status`: `seedling`
   - `sources`: wikilink to the capture file
   - `captured_from`: relative path to `capture/inbox/<filename>.md`
   - `ingested_by`: `claude-opus-4.7`
3. Fill in content sections based on the distilled understanding from step 2e. The raw capture is the evidence source; the distillation results are the knowledge structure source. Do not copy the source article's narrative structure or phrasing.
4. Replace all `<!-- comment -->` placeholders with actual content or remove them.
5. Save to `spaces/<domain>/wiki/<type>/<page-name>.md`.
6. Use `kebab-case` for page filenames.

#### h. Update Domain index
Add the new page to the appropriate section in `<domain>/index.md`:
- Concepts → Concepts section
- Methods → Methods section
- Technologies → Technologies section
- References → References section
- Entities → Entities section

Format: `- [[wiki/<type>/<page-name>|Page Title]] — Brief description.`

#### i. Update Domain log
Add an entry to `<domain>/log.md`:
```
| YYYY-MM-DD | ingest | Routed "capture-filename" → wiki/<type>/<page-name>. Domain: <domain>. Type: <content-type>. |
```

### Step 3: Mark capture as processed
Update the capture file's frontmatter:

```yaml
status: processed
processed_date: YYYY-MM-DD
ingested_to:
  - "[[spaces/<domain>/wiki/<type>/<page-name>]]"
agent_version: claude-opus-4.7
```

This is permitted by CLAUDE.md Raw Rules lines 215-217: "add processing status."

Do NOT modify the raw content section of the capture file.

### Step 4: Report
Output a summary:
- Which captures were processed
- Which pages were created (with full paths)
- Which indexes and logs were updated
- Any routing decisions made and the reasoning
- Any captures that were skipped (with reasons)

## Limits

- Never modify the **raw content section** of capture files. Only update frontmatter metadata.
- Never delete capture files.
- Never skip index and log updates.
- If domain is unclear after applying routing rules, ask the human before creating pages.
- Each capture file should produce 1-3 wiki pages maximum.
- If a capture would produce 4+ pages, ask the human whether to split or consolidate.
- Do not create Master Wiki pages during ingestion (use `/promote` for that).

## References

- `protocol/metadata-schema.md` — Full metadata field definitions
- `protocol/domain-routing.md` — Domain selection and content type rules
- `protocol/knowledge-lifecycle.md` — Status definitions and transitions
- `protocol/knowledge-linking.md` — Wiki link format and resolution rules
- `protocol/knowledge-distillation.md` — Article summary vs knowledge concept distinction, distillation rules, quality checklist
- `FEDERATION.md` — Registered domains and their scopes
- `CLAUDE.md` — Operating Constitution (Raw Rules, Domain Wiki Rules)

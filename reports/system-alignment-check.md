---
created: 2026-07-11
type: alignment-check
scope: full-system
---

# System Alignment Check

## Summary

| Category | Count |
|----------|-------|
| Inconsistencies | 4 |
| Missing components | 5 |
| Unnecessary components | 0 |
| Migration required | 5 items |

Overall status: **ACTION REQUIRED** — CLAUDE.md was rewritten; the rest of the repository lags behind.

---

## 1. Inconsistencies

### 1.1 Master Wiki category mismatch

| Source | Categories |
|--------|-----------|
| `CLAUDE.md` (new) | principles, mental models, cross-domain concepts, **personal frameworks** |
| `spaces/master/schema.md` | models, principles, concepts |

**Issue**: CLAUDE.md adds "personal frameworks" as a fourth Master category. Schema and directory structure only account for three (`wiki/models/`, `wiki/principles/`, `wiki/concepts/`).

**Fix**: Add `wiki/frameworks/` directory and update `spaces/master/schema.md`.

### 1.2 Domain Wiki knowledge types not reflected in structure

| Source | Domain knowledge types |
|--------|----------------------|
| `CLAUDE.md` (new) | concepts, methods, technologies, references, entities |
| `spaces/ai/wiki/` | flat directory, no subdirectories |
| `spaces/ai/schema.md` | directory map shows bare `wiki/` |

**Issue**: CLAUDE.md prescribes 5 knowledge types. The AI domain has no corresponding wiki subdirectories and the schema doesn't mention them.

**Fix**: Create `spaces/ai/wiki/concepts/`, `methods/`, `technologies/`, `references/`, `entities/` and update `spaces/ai/schema.md`.

### 1.3 /lint health checks are incomplete

| Source | Health check dimensions |
|--------|------------------------|
| `CLAUDE.md` (new) | Structural (links, orphans, duplicates) + Knowledge (outdated, missing refs, inconsistent) + **Federation** (domain pollution, master overgrowth, missing relationships) |
| `.claude/commands/lint.md` | Structural + Knowledge only. Federation dimension is **absent**. |

**Issue**: `/lint` command was written for the old CLAUDE.md that didn't define federation health checks.

**Fix**: Add Federation section to lint execution steps covering: domain pollution (master-level content in domain wikis), master overgrowth (domain-level content in master), missing cross-domain relationships.

### 1.4 Commit convention not documented

**Issue**: CLAUDE.md defines 5 commit types (`capture`, `update`, `reflect`, `promote`, `maintenance`) but no other file documents or enforces this convention.

**Fix**: Add commit convention to `protocol/LLM-Wiki-Federation-Protocol.md` or `README.md` as a reference.

---

## 2. Missing Components

### 2.1 `.gitignore`

Obsidian generated `.obsidian/` with 4 files. `workspace.json` contains personal UI state (open tabs, pane sizes, locale) and should be gitignored. The other 3 files (`app.json`, `appearance.json`, `core-plugins.json`) are shared vault configuration and should be tracked.

**Action**: Create `.gitignore` with:
```
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/hotkeys.json
```

### 2.2 Domain wiki subdirectories

CLAUDE.md specifies Domain Wiki contains: concepts, methods, technologies, references, entities. Currently `spaces/ai/wiki/` is an empty flat directory.

**Action**: Create:
```
spaces/ai/wiki/concepts/
spaces/ai/wiki/methods/
spaces/ai/wiki/technologies/
spaces/ai/wiki/references/
spaces/ai/wiki/entities/
```

### 2.3 Master `wiki/frameworks/` directory

CLAUDE.md adds "personal frameworks" as a Master category but no directory exists.

**Action**: Create `spaces/master/wiki/frameworks/`.

### 2.4 Missing templates for Master categories

Only `master-model-template.md` exists. CLAUDE.md specifies Master stores 4 categories. Missing:

| Category | Template exists? |
|----------|-----------------|
| mental models | Yes (`master-model-template.md`) |
| principles | **No** |
| cross-domain concepts | **No** |
| personal frameworks | **No** |

**Action**: Create `templates/master-principle-template.md` and `templates/master-framework-template.md`. (Concepts can reuse the domain-concept template with Master-specific frontmatter.)

### 2.5 Missing domain knowledge templates

Only `domain-concept-template.md` exists for 5 domain knowledge types.

**Action**: Create at minimum `templates/domain-method-template.md`. The other types (technologies, references, entities) can be lighter-weight variants.

---

## 3. Unnecessary Components

**None.** All existing files and directories serve valid purposes under the new CLAUDE.md.

---

## 4. Migration Requirements

### M-1: Update `spaces/master/schema.md`

Add "personal frameworks" to the What Belongs Here section and Directory Map. Add `wiki/frameworks/` to the tree.

### M-2: Update `spaces/ai/schema.md`

Replace bare `wiki/` in the directory map with the 5 knowledge type subdirectories. Add a section explaining what goes in each.

### M-3: Update `.claude/commands/lint.md`

Add a third scan section for Federation health: domain pollution, master overgrowth, missing cross-domain relationships.

### M-4: Create `.gitignore`

As specified in 2.1 above.

### M-5: Create missing directories and templates

As specified in 2.2–2.5 above.

---

## 5. Appendix: Files That Are Acceptable As-Is

These files were checked and found consistent with the new CLAUDE.md:

| File | Verdict |
|------|---------|
| `README.md` | OK — describes 3-layer model, commands, setup |
| `protocol/LLM-Wiki-Federation-Protocol.md` | OK — lifecycle stages match |
| `.claude/commands/ingest.md` | OK — maps to daily "Process inbox" |
| `.claude/commands/update.md` | OK — maps to daily "Update relevant knowledge" |
| `.claude/commands/promote.md` | OK — maps to "Promotion Proposal" rule |
| `.claude/commands/reflect.md` | OK — maps to weekly review |
| `templates/raw-template.md` | OK — immutable capture rule preserved |
| `templates/domain-concept-template.md` | OK — covers the "concepts" type |
| `templates/master-model-template.md` | OK — covers the "mental models" type |
| `spaces/master/index.md` | OK — empty index, ready |
| `spaces/master/log.md` | OK — genesis entry present |
| `spaces/ai/index.md` | OK — empty index, ready |
| `spaces/ai/log.md` | OK — genesis entry present |
| `.obsidian/app.json` | OK — shared config, should be tracked |
| `.obsidian/appearance.json` | OK — shared config, should be tracked |
| `.obsidian/core-plugins.json` | OK — shared config, should be tracked |
| `.obsidian/workspace.json` | OK but should be gitignored |

---

## Recommended Execution Order

1. Create `.gitignore` (M-4)
2. Create missing directories (2.2, 2.3)
3. Create missing templates (2.4, 2.5)
4. Update schema files (M-1, M-2)
5. Update `/lint` command (M-3)
6. Run `/lint` to validate

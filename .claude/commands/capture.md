# /capture — Prepare Local Attachment as Raw Capture

## Purpose

Read a local file from `capture/attachments/` and create a corresponding Markdown capture file in `capture/inbox/` that satisfies `/ingest` prerequisites.

This command bridges the gap between raw local files (PDF, DOCX) and the `/ingest` pipeline, which requires `.md` files with YAML frontmatter.

## Input

- **`/capture <filename>`** — Process a specific file in `capture/attachments/`
- **`/capture`** — Auto-discover files in `capture/attachments/` that have not yet been captured, and process them one at a time
- Skips `.gitkeep`

## Prerequisites

- At least one file in `capture/attachments/` (excluding `.gitkeep`)
- Supported file types (verified): **PDF** (text-based), **DOCX**
- For PDF: `PyPDF2` library available
- For DOCX: `python-docx` library available
- For image analysis: `Pillow` (PIL) library available

## Execution Steps

### Step 1: Identify target file

**If a filename is provided:**
Use it directly. The filename can be a full path (`capture/attachments/doc.pdf`) or just the basename (`doc.pdf`).

**If no filename is provided:**
Auto-discover un-captured files:

1. List all files in `capture/attachments/` (exclude `.gitkeep`, subdirectories).
2. For each file, check whether a corresponding capture already exists in `capture/inbox/`:
   - Read the frontmatter of each `.md` file in `capture/inbox/`
   - A file is "captured" if any inbox `.md` file has a `source` field pointing to that attachment
3. Skip already-captured files.
4. If multiple un-captured files exist, process them one at a time and report the queue.
5. If all files are already captured, report this and stop.

A file is considered "captured" when `source` in any inbox `.md` frontmatter contains the attachment's path (e.g., `source: "capture/attachments/doc.pdf"`).

### Step 2: Attempt text extraction

Based on file extension, select extraction method:

| Extension | Method | Library |
|-----------|--------|---------|
| `.pdf` | Text extraction | `PyPDF2` |
| `.docx` | Paragraph, table, and image extraction | `python-docx` + `Pillow` |

If the file type is unsupported, report the limitation and stop. Do NOT create a capture.

#### 2a. Paragraph text extraction

**PDF**: Extract text from each page via `PyPDF2.PdfReader`.

**DOCX**: Extract paragraph text via `python-docx`. Note: heading styles (Heading 1/2/3) may not be preserved in all documents. Infer heading hierarchy from text patterns (一、/（一）/1.) when styles are absent.

#### 2b. Table extraction (DOCX)

Check for tables via `doc.tables`. For each table found:

1. Extract all cell text.
2. Convert to Markdown table format.
3. Insert into Raw Content at the table's approximate position in the document.
4. If table extraction fails or produces garbled output, record the table count and failure reason in `extraction_notes`.

**PDF tables**: PyPDF2 does not natively extract tables. If text patterns suggest tabular data, note this in `extraction_notes` but do not attempt to reconstruct. This is a known limitation.

#### 2c. Image extraction (DOCX)

For each embedded image/drawing found in the document:

1. **Extract** the image file from the DOCX zip archive (`word/media/`).
2. **Save** to `capture/attachments/extracted/<document-name>/imageN.png`.
3. **Analyze** basic properties via Pillow:
   - Dimensions (width × height)
   - Color mode (RGBA, RGB, etc.)
   - Unique color ratio (low ratio → diagram/chart; high ratio → photo/screenshot)
   - File size
4. **Classify** content type:
   - `DIAGRAM/FRAMEWORK` — low unique color ratio (< 15%), likely a policy framework or organizational chart
   - `CHART/DATA` — medium unique color ratio (15-40%), likely a chart or data visualization
   - `PHOTO/SCREENSHOT` — high unique color ratio (> 40%), likely a photograph or complex screenshot
5. **Identify context**: Record the paragraph text immediately before and after the image to establish what policy topic it relates to.
6. **Perform OCR** on each extracted image using macOS Vision framework (when available):
   - Load image via Quartz/CoreGraphics (`CGImageSourceCreateWithURL`)
   - Create `VNRecognizeTextRequest` with accurate recognition level, `zh-Hans` + `zh-Hant` + `en` languages
   - Extract text observations sorted by vertical position (top-to-bottom)
   - Record per-observation confidence scores
   - Fallback: if macOS Vision is unavailable (non-macOS), report that OCR is not available and proceed without text extraction
7. **Embed in Raw Content**: When generating `## Raw Content`, interleave OCR text at each image's original DOCX paragraph position:
   - Iterate through `doc.paragraphs` in order
   - When an image paragraph index is encountered, insert a Markdown block containing:
     ```markdown
     <!-- image{PARA_IDX}: {filename} →专栏{N} -->
     > **专栏{N}** — {section}（{filename}，OCR 置信度 {avg_confidence}）
     > {ocr_text_line_1}
     > {ocr_text_line_2}
     <!-- /image{PARA_IDX} -->
     ```
   - Continue with subsequent text paragraphs
   - This preserves DOCX reading order and makes images findable via `<!-- imageN:` search
8. **Generate concise index**: Create `## Extracted Visuals` as a compact table (filename, title, para_idx, OCR count, confidence). Link to Raw Content positions via para index. Do NOT duplicate full OCR text here.
9. **Catalog standalone images**: If the input is a standalone image file (`.png`, `.jpg`), treat it as both attachment and visual — OCR it, generate minimal `## Raw Content` with extracted text, and list in `## Extracted Visuals` index.

**PDF images**: PyPDF2 does not extract embedded images. Note image count (if detectable) in `extraction_notes`. This is a known limitation.

### Step 3: Assess extraction quality

Evaluate the extracted content:

- **Good**: Text content is substantial and coherent. Proceed to Step 4.
- **Partial**: Text extracted but quality concerns exist (e.g., images present but not OCR'd, heading styles lost, tables may be incomplete). Proceed to Step 4 with `extraction_notes` in frontmatter.
- **Failed**: No meaningful text extracted. Report the failure reason and stop. Do NOT create a capture.

Quality metadata to check:
- Character count and CJK character ratio
- Presence of recognizable document structure (chapters, sections)
- Count of detected but un-extractable elements (images without OCR, apparent but unparsed tables)
- For DOCX: count of images extracted, tables extracted, heading styles preserved/lost

### Step 4: Generate Raw Capture

Create a `.md` file in `capture/inbox/` following `templates/raw-template.md` and `protocol/metadata-schema.md`.

#### Frontmatter

Required fields:

```yaml
title: "Document title (from filename or extracted content)"
created: YYYY-MM-DD
source: "capture/attachments/<original-filename>"
source_type: pdf-export | other
tags: []
status: raw
```

If extraction was partial, add:

```yaml
extraction_notes: "<tool and version>. <quality summary>. <known losses>."
```

If images were extracted, add:

```yaml
extracted_images: <count>
extracted_images_path: "capture/attachments/extracted/<document-name>/"
```

If tables were extracted, add:

```yaml
extracted_tables: <count>
```

#### Content sections

- `## Source` — Original file path, file type, extraction method, quality notes
- `## Raw Content` — Extracted text, preserving paragraph structure. Mark section headings using Markdown headers (`###`, `####`) where text patterns indicate hierarchy. Include extracted tables as Markdown tables at their approximate positions.
- `## Extracted Visuals` (if images were extracted) — For each image: filename, classification (DIAGRAM/CHART/PHOTO), dimensions, document context (surrounding paragraphs), and path to extracted PNG file. Note that OCR text extraction was not performed.
- `## Initial Impressions` — Brief analysis: document identity, relevance to existing domains, connection to existing knowledge

### Step 5: Verify /ingest prerequisites

Validate:
- [ ] YAML frontmatter present and parseable
- [ ] `created`, `source`, `source_type`, `status: raw` all present
- [ ] `## Source`, `## Raw Content`, `## Initial Impressions` sections present
- [ ] Raw Content is non-empty and substantive

### Step 6: Report

Output:
- File processed (original path)
- Capture created (inbox path)
- Extraction quality assessment
- Known information losses (images without OCR, tables skipped, styles lost)
- Images extracted: count, paths, classification summary
- Tables extracted: count
- `/ingest` readiness: PASS or FAIL with reasons
- If auto-discovery mode: remaining un-captured files in queue

## Limits

- **Never modify** the original attachment file in `capture/attachments/`
- **Never run `/ingest`** — stop after creating the Raw Capture
- **Never create** Reference, Concept, or other Domain Wiki pages
- **Never create a capture with empty Raw Content** — if extraction fails, report and stop
- **One file per invocation** — auto-discovery processes the first un-captured file and reports the queue
- If file type is unsupported, report and stop — do not guess or use fallback extraction
- If extraction produces content that is clearly not meaningful text, report and stop
- Extracted images are saved to `capture/attachments/extracted/` — this directory is for derived artifacts, not original attachments
- Do NOT modify protocol files, templates, or architecture

## Verified Capabilities

| Format | Capability | Status | Notes |
|--------|-----------|--------|-------|
| PDF (text-based) | Paragraph text | Verified | PyPDF2; 8-page document |
| DOCX | Paragraph text | Verified | python-docx; 142-paragraph document |
| DOCX | Image extraction | Verified | 15 images extracted, classified (Pillow), and OCR'd (macOS Vision); text content extracted at 0.50-1.00 confidence |
| Standalone images | OCR text extraction | Verified | macOS Vision framework; supports .png, .jpg via CGImageSource |
| DOCX | Table extraction | Code-ready | API verified (0 tables in test doc); Markdown table conversion ready |
| Auto-discovery | Un-captured file detection | Verified | Checks `source` field in inbox frontmatter |

## Known Limitations

| Limitation | Impact |
|------------|--------|
| Scanned/image PDF | Cannot extract; will report failure |
| PDF embedded images | Not extracted by PyPDF2 |
| PDF tables | Not reconstructed; noted in extraction_notes |
| DOCX images — OCR via macOS Vision | Text extracted (zh-Hans+zh-Hant+en); visual structure (arrows, layout, color coding) not captured |
| DOCX heading styles | May be lost; hierarchy inferred from text patterns |
| `.txt`, `.md` | Not yet verified as attachment sources (trivial case) |
| `.pptx`, `.epub`, `.xlsx` | Not supported |
| Batch processing | One file per invocation; queue reported |

## References

- `templates/raw-template.md` — Capture file template
- `protocol/metadata-schema.md` — Frontmatter field definitions
- `protocol/knowledge-lifecycle.md` — Status definitions (`raw`)
- `.claude/commands/ingest.md` — Downstream command

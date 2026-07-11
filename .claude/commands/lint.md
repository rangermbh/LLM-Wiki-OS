# /lint — Knowledge Quality Check

## Purpose

Run quality checks on the wiki to detect issues.

## Input

- Optional: a specific domain. Default: all domains.

## Execution Steps

1. Scan all Domain Wiki pages for:
   a. **Orphan pages**: pages with no incoming links from index or other pages.
   b. **Duplicate content**: two pages covering the same concept.
   c. **Stale pages**: `updated` date older than 6 months.
   d. **Missing metadata**: pages without frontmatter, tags, or status.
   e. **Broken wikilinks**: links pointing to non-existent pages.
2. Scan Master Wiki for:
   a. Pages without source domain links.
   b. Pages that duplicate Domain content (too specific).
3. Generate a lint report with findings categorized by severity:
   - **ERROR**: Broken links, duplicate pages.
   - **WARN**: Stale pages, missing metadata.
   - **INFO**: Orphan pages, suggested improvements.
4. Write report to `reports/lint-<date>.md`.

## Limits

- Do NOT auto-fix issues — only report them.
- Do NOT modify any files during linting.
- For duplicates, flag both pages and let the human decide which to keep.

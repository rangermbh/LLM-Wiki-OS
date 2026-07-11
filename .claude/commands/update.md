# /update — Update Existing Wiki Pages

## Purpose

Review and refresh existing Domain Wiki pages to keep knowledge current.

## Input

- A specific page path, domain name, or "all" domains

## Execution Steps

1. Identify target pages (single page, domain, or all domains).
2. For each page:
   a. Read current content and frontmatter.
   b. Check linked sources for new information.
   c. Check for stale content (outdated facts, broken links).
   d. Validate wikilinks following `protocol/knowledge-linking.md`:
      - Verify all `[[links]]` resolve to existing files.
      - Flag any bare links (e.g., `[[Concept]]` without path) for correction.
      - Flag any unresolved links that point to non-existent pages.
      - Fix format issues: bare links → explicit path links.
   e. Evaluate knowledge quality against `protocol/knowledge-distillation.md`:
      - Check whether the page structure follows Article Summary patterns (source article order, marketing language, missing Limitations).
      - Check whether required sections (Definition, Core Idea, Mechanism, Relationships, Applications, Limitations) are present and substantive.
      - If the page predates the distillation standard and shows Article Summary characteristics, note it in the report as a migration candidate.
      - Do NOT auto-rewrite pages. Flag them for human decision.
   f. Evaluate page maturity against `protocol/knowledge-lifecycle.md` criteria:
      - seedling → growing: all sections filled, ≥1 incoming link, source capture referenced
      - growing → evergreen: requires human approval (do not auto-transition)
      - If criteria for next status are met, update `status` and note the transition in the log.
   g. Update `updated` date in frontmatter.
   h. Add changes to domain `log.md`.
3. Report changes made, including any status transitions.

## Limits

- Do NOT update Master Wiki pages — only suggest.
- Do NOT change the core meaning of a page without human approval.
- If a page needs major revision, create a new page and suggest archiving the old one.
- Preserve factual attribution, source traceability, and original ideas. Do NOT preserve source article narrative style, marketing language, promotional wording, or article structure. The Wiki should use the knowledge system's own explanatory language (see `protocol/knowledge-distillation.md`).

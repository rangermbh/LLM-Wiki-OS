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
   d. Evaluate page maturity against `protocol/knowledge-lifecycle.md` criteria:
      - seedling → growing: all sections filled, ≥1 incoming link, source capture referenced
      - growing → evergreen: requires human approval (do not auto-transition)
      - If criteria for next status are met, update `status` and note the transition in the log.
   e. Update `updated` date in frontmatter.
   f. Add changes to domain `log.md`.
3. Report changes made, including any status transitions.

## Limits

- Do NOT update Master Wiki pages — only suggest.
- Do NOT change the core meaning of a page without human approval.
- If a page needs major revision, create a new page and suggest archiving the old one.
- Keep the original author's voice where possible.

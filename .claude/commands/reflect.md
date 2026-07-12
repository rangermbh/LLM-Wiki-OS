# /reflect — Review and Suggest Improvements

## Purpose

Periodically review the overall health of the wiki system and suggest structural improvements.

## Input

- None required. May optionally specify a focus area.

## Execution Steps

1. Review system-level metrics:
   a. Capture inbox size and age.
   b. Domain wiki page counts and growth rate.
   c. Master wiki proposal backlog.
   d. Orphan count from last lint.
2. Check structural integrity:
   a. Do all domains have schema.md, index.md, log.md?
   b. Are templates up to date?
   c. Are commands working as designed?
   d. Document consistency review:
      - Compare document-map.md sections against actual directory structure (protocols, templates, commands, domains, wiki pages)
      - Check project-state.md phase/status against session-snapshot.md current phase
      - Verify session-snapshot.md pending decisions reflect actual resolution state
      - Identify navigation gaps: items that exist on disk but are missing from document-map.md
3. Identify improvement opportunities:
   a. New domains to create?
   b. Domains to split or merge?
   c. Workflow bottlenecks?
   d. Knowledge gaps?
4. Write reflection report to `reports/reflect-<date>.md`.

## Limits

- This is an advisory command only. Do not make changes.
- Focus on system health, not content quality (that's lint's job).
- Suggest, don't prescribe.

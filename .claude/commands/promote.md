# /promote — Propose Master Wiki Content

## Purpose

Analyze Domain Wiki content for cross-domain patterns and propose Master Wiki entries.

## Input

- Optional: specific domains to analyze. Default: all domains.

## Execution Steps

1. Scan Domain Wiki pages across at least 2 domains.
2. Identify patterns that appear in 3+ pages across different domains:
   a. Recurring concepts or principles.
   b. Similar mental models applied in different contexts.
   c. Universal heuristics or rules of thumb.
3. For each pattern:
   a. Write a proposed Master page using `templates/master-model-template.md`.
   b. Place it in `spaces/master/wiki/<appropriate-category>/`.
   c. Set frontmatter `status: proposed`.
   d. Link back to source Domain pages.
4. Update `spaces/master/log.md` with the proposal.
5. Present proposals to the human for review.

## Limits

- NEVER set a Master page status to anything other than `proposed`.
- Only the human may change status to `accepted` or `rejected`.
- Each proposal must cite at least 3 source pages from at least 2 different domains.
- Do not propose domain-specific facts as Master content.

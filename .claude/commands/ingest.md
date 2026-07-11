# /ingest — Process Capture Inbox

## Purpose

Process raw items in `capture/inbox/` and extract structured knowledge into the appropriate Domain Wiki.

## Input

- One or more files in `capture/inbox/`

## Execution Steps

1. List all files in `capture/inbox/`.
2. For each file:
   a. Read the raw content.
   b. Identify which Domain(s) it belongs to.
   c. Create a new wiki page in `spaces/<domain>/wiki/` using the domain-concept template.
   d. If no suitable Domain exists, propose creating one.
   e. Update the Domain's `index.md` to include the new page.
   f. Update the Domain's `log.md` with the ingestion record.
3. Report what was created and where.
4. Do NOT modify or delete the original capture file.

## Limits

- Never modify raw capture files.
- Never skip the index and log updates.
- If domain is unclear, ask the human before creating pages.
- Each capture file may produce 1-3 wiki pages maximum.

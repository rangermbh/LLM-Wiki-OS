# LLM Wiki OS

A personal knowledge operating system powered by Obsidian, Claude Code, and Git.

## What Is This?

LLM Wiki OS is a **federated personal wiki** designed for long-term knowledge management. Unlike traditional wikis that grow into unmaintainable monoliths, this system separates knowledge into three layers:

| Layer | Location | Purpose |
|-------|----------|---------|
| **Capture** | `capture/` | Raw, unprocessed external inputs |
| **Domain** | `spaces/*/` | Curated domain-specific knowledge |
| **Master** | `spaces/master/` | Cross-domain models, principles, concepts |

## How It Works

### For You (Human)

1. **Capture** — Save anything into `capture/inbox/`. Web clips, notes, PDFs, AI conversations. Don't filter.
2. **Review** — The Maintainer Agent processes captures into structured Domain Wiki pages and notifies you.
3. **Curate** — Review, edit, and approve wiki pages in your Domain spaces.
4. **Abstract** — When patterns emerge across domains, the Agent proposes Master Wiki entries. You decide what gets promoted.

### For Claude Code (Maintainer Agent)

Claude Code acts as your **Wiki Maintainer**, handling:

- **Ingestion** (`/ingest`) — Process raw captures into Domain Wiki pages
- **Updates** (`/update`) — Refresh and maintain existing content
- **Quality** (`/lint`) — Check for broken links, orphans, staleness
- **Promotion** (`/promote`) — Propose cross-domain patterns for Master Wiki
- **Reflection** (`/reflect`) — Periodic system health reviews

All agent rules are defined in `CLAUDE.md`.

## Repository Structure

```
.
├── CLAUDE.md              # Agent behavior rules
├── README.md              # This file
├── capture/               # Raw inputs (immutable)
│   ├── inbox/
│   └── attachments/
├── spaces/                # Knowledge domains
│   ├── master/            # Cross-domain models & principles
│   │   └── wiki/
│   │       ├── models/
│   │       ├── principles/
│   │       └── concepts/
│   └── ai/                # Example domain
│       ├── schema.md
│       ├── index.md
│       ├── log.md
│       ├── raw/
│       ├── wiki/
│       └── sources/
├── protocol/              # Federation protocol spec
├── templates/             # Page templates
├── .claude/commands/      # Maintainer command definitions
├── reports/               # Lint and reflection reports
└── archive/               # Superseded content
```

## Getting Started

### Prerequisites

- [Obsidian](https://obsidian.md) — Open this repository as an Obsidian vault
- [Claude Code](https://claude.ai/code) — The Maintainer Agent
- Git — Version history

### Setup

1. Clone this repository
2. Open the folder in Obsidian
3. Use Claude Code in this directory — the agent reads `CLAUDE.md` automatically
4. Start capturing: drop files into `capture/inbox/`
5. Run `/ingest` to process your first captures

## Design Principles

- **Raw is sacred** — Captured content is never modified
- **Master is earned** — Only cross-domain patterns reach Master Wiki
- **Federation over monolith** — Each domain is self-contained
- **Human in the loop** — The agent suggests, the human decides
- **Git as backbone** — Every change is versioned

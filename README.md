# LLM Wiki OS

A personal knowledge operating system powered by Obsidian, Claude Code, and Git.

## What Is This?

LLM Wiki OS is a **personal knowledge operating system** that combines human curation with AI agent maintenance. It organizes knowledge into a federated architecture where information flows from raw capture to structured wiki pages, and eventually to cross-domain mental models.

## Core Philosophy

**Knowledge Federation.** Knowledge lives in semi-autonomous domains, not a single monolithic wiki. Each domain is self-contained. Cross-domain patterns are promoted to a Master Wiki — but only when they prove useful across contexts.

**Human + Agent Collaboration.** You capture and curate. The agent ingests, maintains, and discovers. The human always holds final authority — the agent proposes, never decides unilaterally.

**Knowledge Lifecycle Management.** Every piece of knowledge follows a defined lifecycle: `raw → processed → seedling → growing → evergreen`. Master entries go through `proposed → accepted → active`. Each transition has explicit criteria and authority boundaries.

## Architecture

```
External Inputs (Web, books, conversations...)
        │
        ▼
┌───────────────────┐
│   Capture Layer   │  ← Raw, immutable records
│   capture/inbox/  │
└───────┬───────────┘
        │ /ingest
        ▼
┌───────────────────┐
│   Domain Wiki     │  ← Curated domain knowledge
│   spaces/{domain}/ │    seedling → growing → evergreen
└───────┬───────────┘
        │ /promote
        ▼
┌───────────────────┐
│   Master Wiki     │  ← Cross-domain models & principles
│   spaces/master/  │    Human-controlled
└───────────────────┘
```

- **Capture Layer** — Temporary landing zone for all incoming information. Raw content is never modified.
- **Domain Wiki** — Structured knowledge organized by domain (AI, Knowledge Management, ...). Each domain has 5 knowledge types: concepts, methods, technologies, references, entities.
- **Master Wiki** — Cross-domain abstractions: mental models, principles, concepts, and personal frameworks. Human-edited only.

## Current Features

- **Web Clipper Capture** — Save web content directly into `capture/inbox/` via Obsidian Web Clipper
- **Ingestion Pipeline** — `/ingest` routes captures to the correct domain, selects templates, and creates structured wiki pages
- **Domain Routing** — Automatic domain assignment with boundary zone handling for ambiguous topics
- **Metadata Governance** — Standardized YAML frontmatter across all pages with defined validation rules
- **Knowledge Linking** — Governed wikilinks with explicit paths, preventing Obsidian root-level orphan files
- **Knowledge Flow Management** — Complete traceability from external source to Master Wiki, with quality gates at each transition

## Project Structure

```
.
├── CLAUDE.md                  # Agent Operating Constitution
├── FEDERATION.md              # Federation members, authority model, signals
├── capture/inbox/             # Raw capture files (immutable content)
├── spaces/
│   ├── master/wiki/           # Cross-domain world model
│   ├── ai/wiki/               # AI domain knowledge
│   └── knowledge-management/  # Knowledge Management domain
├── protocol/                  # Federation protocol documents (6)
├── templates/                 # Page templates (raw, domain × 5, master × 4)
├── .claude/commands/          # Agent command definitions
├── docs/                      # Documentation and audit reports
├── reports/                   # Health check reports
└── archive/                   # Superseded content
```

## Quick Start

### Prerequisites

- [Obsidian](https://obsidian.md) with community plugins enabled
- [Claude Code](https://claude.ai/code)
- Git

### Setup

1. **Clone** this repository and open it as an Obsidian vault
2. **Configure plugins** — Enable community plugins in Obsidian settings (Web Clipper template: `.obsidian/templates/llm-wiki-capture.md`)
3. **Start capturing** — Use the Web Clipper or drop files into `capture/inbox/`
4. **Run ingestion** — In Claude Code, run `/ingest` to process captures into structured Domain Wiki pages
5. **Review** — Check the generated pages in Obsidian. Edit, link, and refine

### Maintenance Commands

| Command | Purpose |
|---------|---------|
| `/ingest` | Process raw captures into Domain Wiki pages |
| `/update` | Review and refresh existing wiki pages |
| `/lint` | Run quality checks (broken links, orphans, staleness) |
| `/promote` | Propose cross-domain patterns for Master Wiki |
| `/reflect` | Weekly system health review |

## Design Principles

- **Raw is sacred** — Captured content is never modified
- **Master is earned** — Only cross-domain patterns reach Master Wiki
- **Federation over monolith** — Each domain is self-contained
- **Human in the loop** — The agent suggests, the human decides
- **Git as backbone** — Every knowledge change is versioned

# Git Commit Convention

This document defines the commit message conventions for the LLM Wiki OS repository. All commits must follow one of the five types defined in `CLAUDE.md`.

## Commit Types

### capture
**When**: New raw information is added to `capture/inbox/` or `capture/attachments/`.

```
capture: <brief description of captured content>

Examples:
capture: webclip - Attention is All You Need paper
capture: manual notes from AI safety workshop
capture: PDF - Goodfellow Deep Learning chapter 6
```

### update
**When**: Existing Domain Wiki pages are created, edited, or improved.

```
update: <domain>: <what changed>

Examples:
update: ai - add Transformer Architecture concept
update: ai - expand fine-tuning methods with LoRA details
update: ai - fix broken link in NLP overview
```

### reflect
**When**: Master Wiki models, principles, or frameworks are added or changed. This marks a change to the personal world model.

```
reflect: <what changed in the world model>

Examples:
reflect: accept "Compression as Understanding" model
reflect: add "Prefer Simplicity" principle
reflect: supersede "First Principles" with v2
```

### promote
**When**: A Promotion Proposal is created for Master Wiki (not when accepted — that's `reflect`).

```
promote: propose <concept> from <domain-a>, <domain-b>

Examples:
promote: propose "Emergence" from ai, biology
promote: propose "Feedback Loops" from ai, business, biology
```

### maintenance
**When**: System-level changes that don't modify knowledge content.

```
maintenance: <what changed>

Examples:
maintenance: add research domain
maintenance: update CLAUDE.md to v2
maintenance: fix lint command numbering
maintenance: add .gitkeep files to empty directories
```

## Rules

1. **One commit per logical change.** Don't batch unrelated changes.
2. **Use the type that matches the PRIMARY change.** If you add a concept AND fix a typo, use `update`.
3. **Keep messages under 72 characters** for the first line.
4. **No emoji in commit messages.**
5. **Never use `--no-verify` or skip hooks.**

## Commit Authorship

The Wiki Maintainer Agent (Claude Code) creates commits with:
```
Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
```

Human-authored commits use the human's Git configuration.

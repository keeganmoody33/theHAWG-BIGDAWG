# theHAWG-BIGDAWG

Self-referential GTM engineering workspace for TheHog.ai. This repo is a living Context OS: source docs, atomic knowledge nodes, strategic foundation docs, and session logs that compound as agents research and execute GTM work.

## Context OS

This repo is structured as a **Context OS** — a two-layer knowledge architecture where AI compounds intelligence over time. Agents navigate via wiki-links across a knowledge graph of atomic concepts.

### Quick Start

1. Open in [Obsidian](https://obsidian.md/) to see the full wiki-link graph
2. Read `CLAUDE.md` for the navigation guide and operating protocol
3. Start with `00_foundation/_synthesis/gtm-context-os-synthesis.md` for the executive summary

### First-read order

1. `progress.md`
2. `plans/findings.md`
3. `plans/task_plan.md`
4. `docs/mcp-setup.md`
5. `docs/thehog-api-reference.md`
6. `docs/customer-sessions.md`
7. `docs/30-60-90-playbook.md`
8. `knowledge_base/`
9. `00_foundation/`

### Structure

| Path | Purpose |
| --- | --- |
| `knowledge_base/technical/` | API, enrichment, metering, MCP |
| `knowledge_base/business/` | Company, ICP segments, segmentation, people |
| `knowledge_base/methodology/` | GTM engineering, SOAD loop, architecture |
| `knowledge_base/emergent/` | New concepts not yet validated |
| `knowledge_base/raw_sources/` | Transcripts, notes, raw input |
| `00_foundation/positioning/` | Scoring engine, battlecards, MCP guide, monitors |
| `00_foundation/messaging/` | Outbound playbook |
| `00_foundation/_synthesis/` | Summary documents |
| `docs/` | Source reference docs and MCP setup instructions |
| `plans/` | Current task plan and backlog |
| `plans/findings.md` | Graph health audit, gap analysis, and session findings |
| `progress.md` | Current status and next actions |

### Operating Loop

1. **SENSE:** Read existing docs and nodes before acting.
2. **ORIENT:** Decide whether new facts belong in `knowledge_base/`, `00_foundation/`, `plans/findings.md`, or `progress.md`.
3. **ACT:** Use Hog.ai MCP/API only when fresh data is needed.
4. **DEPOSIT:** Preserve reusable facts with source, timestamp, confidence, and wiki-links.

### For Agents

- `CLAUDE.md` — navigation guide with `context-os` CLI commands
- `.claude/skills/thehawg-bigdawg/SKILL.md` — GTM engineering skill (SENSE → ORIENT → ACT → DEPOSIT)
- `taxonomy.yaml` / `ontology.yaml` — blessed tags and relationship types
- `plans/findings.md` — graph health audit and gap analysis

### MCP and Secrets

Use `.env.example` as the template. Credentials must stay in local environment variables only:

- `THEHOG_ACCESS_KEY`
- `THEHOG_SECRET_KEY`
- `THEHOG_MCP_URL`

Do not commit `.env` or paste credentials into prompts, docs, or config.

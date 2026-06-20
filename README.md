# theHAWG-BIGDAWG

Self-referential GTM engineering workspace for TheHog.ai. This repo is a living Context OS: source docs, atomic knowledge nodes, strategic foundation docs, and session logs that compound as agents research and execute GTM work.

## First-read order

1. `progress.md`
2. `findings.md`
3. `plans/task_plan.md`
4. `docs/mcp-setup.md`
5. `docs/thehog-api-reference.md`
6. `docs/customer-sessions.md`
7. `docs/30-60-90-playbook.md`
8. `knowledge_base/`
9. `00_foundation/`

## Structure

| Path | Purpose |
| --- | --- |
| `docs/` | Source reference docs and MCP setup instructions |
| `knowledge_base/` | Atomic graph nodes for companies, product facts, ICPs, personas, competitors, and signals |
| `00_foundation/` | Strategic synthesis docs that compose graph nodes into operating systems |
| `plans/` | Current task plan and backlog |
| `findings.md` | Session findings and ingestion notes |
| `progress.md` | Current status and next actions |
| `.claude/skills/thehawg-bigdawg/SKILL.md` | Agent operating instructions for this workspace |

## Operating loop

1. SENSE: read existing docs and nodes before acting.
2. ORIENT: decide whether new facts belong in `knowledge_base/`, `00_foundation/`, `findings.md`, or `progress.md`.
3. ACT: use Hog.ai MCP/API only when fresh data is needed.
4. DEPOSIT: preserve reusable facts with source, timestamp, confidence, and wiki-links.

## MCP and secrets

Use `.env.example` as the template. Credentials must stay in local environment variables only:

- `THEHOG_ACCESS_KEY`
- `THEHOG_SECRET_KEY`
- `THEHOG_MCP_URL`

Do not commit `.env` or paste credentials into prompts, docs, or config.

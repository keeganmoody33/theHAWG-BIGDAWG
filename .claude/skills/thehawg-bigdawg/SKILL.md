---
name: thehawg-bigdawg
description: Operate and validate theHAWG-BIGDAWG Context OS vault for TheHog.ai GTM engineering. Use when updating docs, knowledge graph nodes, MCP/API deposits, or vault validation workflows.
---

# theHAWG-BIGDAWG Skill

## Purpose

Operate this repository as a living Context OS for TheHog.ai GTM engineering. Use the Hog.ai MCP as the ingestion layer and deposit reusable intelligence into the graph.

## First-read order

1. `README.md`
2. `progress.md`
3. `findings.md`
4. `plans/task_plan.md`
5. `docs/mcp-setup.md`
6. `docs/thehog-api-reference.md`
7. `docs/customer-sessions.md`
8. `docs/30-60-90-playbook.md`
9. `knowledge_base/`
10. `00_foundation/`

## Operating loop

### SENSE

Read the relevant docs, existing graph nodes, and session logs. Identify whether the task needs fresh MCP data or can be answered from existing nodes.

### ORIENT

Pick the owner document for the fact or synthesis:

- Atomic company/product/persona facts go in `knowledge_base/`.
- Strategy, scoring, playbooks, and reusable systems go in `00_foundation/`.
- Session observations go in `findings.md`.
- Status and next steps go in `progress.md`.

### ACT

Use Hog.ai MCP/API calls only when fresh data is required. Start with low-cost read-only calls. Avoid broad people searches, enrichment, monitors, or deep research until rate limits and credit costs are understood.

### DEPOSIT

Write durable findings back to the graph with source, timestamp, confidence, and wiki-links. Never leave useful context only in chat.

## MCP connection

Use environment variables only:

- `THEHOG_ACCESS_KEY`
- `THEHOG_SECRET_KEY`
- `THEHOG_MCP_URL`

Default hosted MCP URL:

```text
https://mcp.thehog.ai/mcp
```

Hosted MCP uses OAuth when the client supports remote MCP. Local stdio uses `npx -y @thehog/mcp@latest` with `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY` in the process environment.

Do not hardcode credentials. Do not paste credentials into prompts or markdown.

## Rate-limit and async discipline

- Treat 5 requests per second as an unconfirmed ceiling; prefer lower throughput until official limits are known.
- Use lower throughput for first tests.
- Poll async jobs with backoff.
- Handle terminal statuses: `succeeded`, `failed`, `partial_success`, and `cancelled`.
- Deep research polling should be slower than enrichment polling.
- Stop on repeated failures, 402s, 429s, or unclear credit usage.

## Safe first ingestion

1. Run `companies/search` for `TheHog.ai` or `Hog.ai`.
2. Poll until terminal state.
3. Add confirmed facts to `knowledge_base/thehog-ai-company.md`.
4. Log operation metadata and summary in `findings.md`.
5. Update `progress.md`.

## Never do

- Never commit `.env`.
- Never hardcode secrets.
- Never run credit-heavy jobs without explicit intent.
- Never overwrite existing graph facts without preserving provenance.
- Never skip the DEPOSIT step after discovering reusable information.

## Testing and validation

Use shell-only validation for docs/vault PRs. Recording is usually not useful unless the test involves Obsidian or another GUI.

Before execution:

1. Confirm no login is required for markdown-only validation.
2. Check repo-scoped secrets without printing values. Live MCP/API tests need the secrets listed below; markdown graph validation does not.
3. Read the changed files with line numbers and identify the exact graph links or deposited facts being tested.
4. Check PR comments and CI before executing tests.

Recommended markdown graph validation:

```bash
python3 - <<'PY'
import re
from pathlib import Path
root = Path('/home/ubuntu/repos/theHAWG-BIGDAWG')
mds = [p for p in root.rglob('*.md') if '.git' not in p.parts]
slugs = {p.stem for p in mds}
missing = []
for p in mds:
    text = p.read_text()
    for raw in re.findall(r'\[\[([^\]|#]+)', text):
        if raw not in slugs:
            missing.append((p.relative_to(root).as_posix(), raw))
if missing:
    for path, raw in missing:
        print(f'Missing wiki target: {path}: [[{raw}]]')
    raise SystemExit(1)
print(f'Wiki-link validation passed: {len(mds)} markdown files, 0 missing targets')
PY
```

Recommended knowledge-base frontmatter validation:

```bash
python3 - <<'PY'
from pathlib import Path
root = Path('/home/ubuntu/repos/theHAWG-BIGDAWG')
missing = []
for p in sorted((root / 'knowledge_base').glob('*.md')):
    text = p.read_text()
    if not text.startswith('---\n') or '\n---\n' not in text[4:]:
        missing.append(p.relative_to(root).as_posix())
if missing:
    print('\n'.join(missing))
    raise SystemExit(1)
print('Knowledge-base frontmatter validation passed')
PY
```

For a docs-only PR, also verify exact required strings with `grep -F` so a generic link check cannot pass if the core synthesis was omitted.

## Devin Secrets Needed

- `THEHOG_ACCESS_KEY` — only for live Hog.ai API/MCP ingestion tests.
- `THEHOG_SECRET_KEY` — only for live Hog.ai API/MCP ingestion tests.
- `THEHOG_MCP_URL` — only for live Hog.ai MCP connectivity tests.
- No secrets are needed for markdown graph validation, frontmatter checks, or docs-only PR testing.


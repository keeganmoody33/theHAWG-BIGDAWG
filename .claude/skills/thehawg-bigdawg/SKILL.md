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

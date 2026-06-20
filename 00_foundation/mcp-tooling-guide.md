# MCP Tooling Guide

## Role

The Hog.ai MCP is the ingestion engine for this Context OS. It should be used to gather fresh company, people, market, and signal data, then deposit durable facts into the graph.

## Required behavior

- Read local MCP credentials from `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`, and `THEHOG_MCP_URL`.
- Use hosted remote MCP with OAuth when the client supports it.
- Use local stdio via `npx -y @thehog/mcp@latest` when remote MCP is unavailable.
- Never write credentials to files.
- Prefer read-only, low-cost calls before credit-heavy workflows.
- Poll async operations until terminal state.
- Handle `succeeded`, `failed`, `partial_success`, and `cancelled` explicitly.
- Treat `402 Payment Required` and `429 Too Many Requests` as stop conditions.
- Use backoff for polling.
- Deposit facts with source, date, confidence, metering, and linked nodes.

## Safe first calls

1. `POST /api/v1/companies/search` for `TheHog.ai` with a low `limit`.
2. `POST /api/v1/search` with one narrow keyword and low `max_results`.
3. Tight-schema `POST /api/deep-research` only after rate limits and credit costs are known.

## Calls requiring extra care

- People searches at scale.
- Enrichment, because it can consume credits per contact.
- Monitors, because recurring jobs can accumulate volume.
- Broad deep research, because source count and complexity can increase cost.

## Deposit format

Each new fact should land in the smallest durable node possible:

```yaml
source: hog_mcp
retrieved_at: YYYY-MM-DDTHH:MM:SSZ
confidence: low|medium|high
raw_reference: operation_id_or_url
metering:
  creditsCharged:
  estimatedMaxCredits:
  costEstimated:
  costActual:
```

## Links

- [[thehog-ai-api]]
- [[thehog-ai-company]]

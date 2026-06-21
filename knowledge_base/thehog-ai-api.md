---
type: product
name: TheHog.ai API
status: seed
source_docs:
  - docs/thehog-api-reference.md
---

# TheHog.ai API

## Base URL

`https://developer.thehog.ai`

## Authentication

The API uses two headers:

- `X-Access-Key`
- `X-Secret-Key`

Credentials must come from environment variables only. For local MCP, use `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY`. Hosted remote MCP uses OAuth when supported by the client.

## Live capabilities

- Company Search: `POST /api/v1/companies/search`
- People Search: `POST /api/v1/people/search`
- Contact Enrichment: `POST /api/enrichments`, single contact or batch up to 100 identifiers
- Deep Research: `POST /api/deep-research`; docs also reference `POST /api/v1/deep-research`
- Social Listening Monitors: `POST /api/v1/monitors`
- Monitor Events: `GET /api/v1/monitors/:id/events`
- Multi-Platform Search: `POST /api/v1/search`, async by default or brief synchronous attempt with `sync=true`
- MCP Integration: hosted remote MCP with OAuth or local stdio via `npx -y @thehog/mcp@latest`

## Async invariant

Non-trivial endpoints return an operation ID and poll URL. Poll `GET /api/operations/:id` until `status` is `succeeded`, `failed`, `partial_success`, or `cancelled`. Treat `402 Payment Required` and `429 Too Many Requests` as stop conditions.

## Agent-first interface invariant

The API/MCP surface is the primary interaction layer for agentic GTM work. Human users should set objectives, constraints, budgets, and approvals; agents should choose endpoints, manage polling, interpret terminal statuses, preserve provenance, and return action-ready outputs.

## Credit and metering invariant

When present, capture both `metering.creditsCharged` / `metering.estimatedMaxCredits` and `meta.cost.estimated` / `meta.cost.actual`. For deep research, use `maxCredits` or `budget.maxCredits` when spend must be bounded.

## GTM workflow invariant

The canonical workflow is:

1. Search companies.
2. Find people at selected accounts.
3. Enrich selected contacts.
4. Monitor market signals.
5. Use deep research for structured account and market briefs.

## Links

- [[thehog-ai-company]]
- [[mcp-tooling-guide]]
- [[icp-scoring-engine]]
- [[agent-interface-contract]]

---
name: THEHOG_AI_API
description: REST API surface for TheHog.ai — search, enrich, and batch endpoints with credit metering
domain: technical
node_type: concept
status: validated
last_updated: 2026-06-20
tags:
  - technical
  - api
  - data-enrichment
  - rest
topics:
  - api-design
  - enrichment-endpoints
  - batch-processing
  - credit-metering
  - async-workflows
  - rate-limiting
related_concepts:
  - "[[thehog-ai-company]]"
  - "[[hog-api-enrichment]]"
  - "[[mcp-integration]]"
  - "[[hog-api-metering]]"
  - "[[hog-api-search]]"
  - "[[paulo-nascimento]]"
source:
  type: document
  file: "Context7 corrections + session breadcrumbs"
  date: "2026-06-20"
---

# TheHog.ai API

The TheHog.ai API is a REST interface providing company and people data for GTM workflows. It exposes three primary capabilities: search, enrichment, and batch processing. Authentication uses dual-header credentials — `X-Access-Key` and `X-Secret-Key` — sourced from the Credentials page in the dashboard. Canonical env var names: `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY`.

The API follows a metering-first design — responses may include `metering.creditsCharged` and `metering.estimatedMaxCredits` so callers can track spend in real time. These fields are present on enrichment and search responses but may be absent on some operation types — always use safe access with fallback defaults. Rate limiting returns `429 Too Many Requests`; payment issues return `402 Payment Required`. Both are stop conditions for batch workflows.

## Key Points

- **Base URL:** `https://developer.thehog.ai`
- **Auth headers:** `X-Access-Key` + `X-Secret-Key` (env vars: `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`)
- **Companies:** `POST /api/v1/companies/search` — company lookup by domain/name (async, returns `202` + `pollUrl`)
- **People:** `POST /api/v1/people/search` — contact discovery scoped to company
- **Enrichment:** `POST /api/enrichments` — single or batch (up to 100), returns `200` or `202`
- **Search:** `POST /api/v1/search` — multi-platform search, supports `sync=true`
- **Deep Research:** `POST /api/deep-research` — LLM-powered research with JSON Schema output
- **Monitors:** `POST /api/v1/monitors` — recurring social/web monitors
- **Async polling:** `GET /api/operations/:id` — states: `succeeded`, `failed`, `partial_success`, `cancelled`
- **Metering:** `metering.creditsCharged`, `metering.estimatedMaxCredits`, `meta.cost.estimated`, `meta.cost.actual`

## Evidence

> Batch enrichment supports up to 100 identifiers per request.
> [SOURCE: Context7 library data]

> Stop conditions: 402 Payment Required, 429 Too Many Requests.
> [SOURCE: Context7 corrections applied in hardening session]

## How It Relates

- [[thehog-ai-company]] - The company behind this API
- [[hog-api-enrichment]] - Enrichment-specific workflows using this API
- [[hog-api-metering]] - Credit metering model and cost tracking
- [[hog-api-search]] - Search endpoint patterns and query construction
- [[mcp-integration]] - Alternative agent-native access via MCP protocol
- [[paulo-nascimento]] - Co-founder and architect of this API

---

**Status:** Validated — endpoint structure confirmed via API reference docs + live API calls
**API calls confirmed working:** `web_search`, `x_keyword`, `linkedin_keyword`, `people/search`, `companies/search`

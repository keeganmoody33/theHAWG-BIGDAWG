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
source:
  type: document
  file: "Context7 corrections + session breadcrumbs"
  date: "2026-06-20"
---

# TheHog.ai API

The TheHog.ai API is a REST interface providing company and people data for GTM workflows. It exposes three primary capabilities: search, enrichment, and batch processing. Authentication uses access key + secret key pairs via environment variables `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY`.

The API follows a metering-first design — every response includes `metering.creditsCharged` and `metering.estimatedMaxCredits` so callers can track spend in real time. Rate limiting returns `429 Too Many Requests`; payment issues return `402 Payment Required`. Both are stop conditions for batch workflows.

## Key Points

- **Base URL:** `api.thehog.ai`
- **Auth:** `THEHOG_ACCESS_KEY` + `THEHOG_SECRET_KEY` (canonical env var names)
- **Search:** `POST /api/v1/search` — supports `sync=true` for immediate results
- **Companies:** `POST /api/v1/companies/search` — company lookup by domain/name
- **Batch:** Up to 100 identifiers per request, async by default
- **Async states:** `succeeded`, `failed`, `partial_success`, `cancelled`
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

---

**Status:** Validated — endpoint structure confirmed via Context7 library
**Next:** Execute safe test call (`POST /api/v1/companies/search` for "TheHog.ai" limit 3)

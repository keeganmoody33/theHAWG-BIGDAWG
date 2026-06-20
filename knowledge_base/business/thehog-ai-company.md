---
name: THEHOG_AI_COMPANY
description: TheHog.ai — AI-powered data enrichment and prospecting platform for GTM teams
domain: business
node_type: concept
status: validated
last_updated: 2026-06-20
tags:
  - business
  - company-profile
  - data-enrichment
  - prospecting
topics:
  - company-overview
  - value-proposition
  - product-market-fit
  - gtm-infrastructure
  - ai-enrichment
related_concepts:
  - "[[thehog-ai-api]]"
  - "[[gtm-engineering]]"
  - "[[icp-segmentation]]"
  - "[[mcp-integration]]"
  - "[[hog-api-enrichment]]"
  - "[[icp-scoring-engine]]"
source:
  type: document
  file: "README.md + prior session context"
  date: "2026-06-20"
---

# TheHog.ai Company

TheHog.ai is an AI-powered data enrichment and prospecting platform designed for Go-to-Market (GTM) teams. It provides programmatic access to company and people data through a REST API and MCP (Model Context Protocol) endpoint, enabling automated lead discovery, enrichment, and scoring workflows.

The platform differentiates through batch enrichment at scale (up to 100 identifiers per request), async processing with granular status tracking, and metering transparency via `creditsCharged` / `estimatedMaxCredits` fields. It is purpose-built for the [[gtm-engineering]] workflow — engineers who write code to drive pipeline rather than clicking through CRM UIs.

## Key Points

- REST API at `api.thehog.ai` with search, enrich, and batch endpoints
- MCP endpoint at `https://mcp.thehog.ai/mcp` for agent-native access
- Metering model with credit-based pricing and transparent cost fields
- Supports both sync (`sync=true`) and async enrichment workflows
- Stop conditions on `402 Payment Required` and `429 Too Many Requests`

## Evidence

> Batch enrichment supports up to 100 identifiers per request. Async terminal states: succeeded, failed, partial_success, cancelled.
> [SOURCE: Context7 library data, verified in prior session]

> MCP endpoint: `https://mcp.thehog.ai/mcp` — supports OAuth authentication and local stdio via `npx -y @thehog/mcp@latest`
> [SOURCE: Hog documentation + session breadcrumbs]

## How It Relates

- [[thehog-ai-api]] - The technical interface to TheHog's data
- [[gtm-engineering]] - The discipline this product serves
- [[icp-segmentation]] - Core use case: segment and score leads from Hog data
- [[mcp-integration]] - Agent-native access pattern via MCP protocol
- [[hog-api-enrichment]] - The enrichment workflow powered by Hog

---

**Status:** Validated — confirmed through API docs and prior session work
**Next:** Execute first live API call to ground remaining nodes in real data

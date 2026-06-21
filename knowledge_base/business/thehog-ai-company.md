---
name: THEHOG_AI_COMPANY
description: TheHog.ai — AI-powered data enrichment and prospecting platform for GTM teams
domain: business
node_type: concept
status: validated
last_updated: 2026-06-21
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
  - "[[hudson-liao]]"
  - "[[paulo-nascimento]]"
social:
  x: "https://x.com/TheHogAi"
  linkedin: "https://linkedin.com/company/thehogai"
  crunchbase: "https://crunchbase.com/organization/the-hog"
  f6s: "https://f6s.com/company/the-hog"
source:
  type: document
  file: "README.md + Hog API (web_search, x_keyword) + Y Combinator + Crunchbase"
  date: "2026-06-21"
---

# TheHog.ai Company

TheHog.ai is an AI-powered data enrichment and prospecting platform designed for Go-to-Market (GTM) teams. Founded in 2025 by [[hudson-liao]] and [[paulo-nascimento]], The Hog is a Y Combinator (F25) company. It provides programmatic access to company and people data through a REST API and MCP (Model Context Protocol) endpoint, enabling automated lead discovery, enrichment, and scoring workflows.

The platform differentiates through batch enrichment at scale (up to 100 identifiers per request), async processing with granular status tracking, and metering transparency via `creditsCharged` / `estimatedMaxCredits` fields. It is purpose-built for the [[gtm-engineering]] workflow — engineers who write code to drive pipeline rather than clicking through CRM UIs.

## Key Points

- REST API at `https://developer.thehog.ai` with search, enrich, and batch endpoints (auth: `X-Access-Key` + `X-Secret-Key`)
- MCP endpoint at `https://mcp.thehog.ai/mcp` for agent-native access
- Metering model with credit-based pricing and transparent cost fields
- Supports both sync (`sync=true`) and async enrichment workflows
- Stop conditions on `402 Payment Required` and `429 Too Many Requests`

## Evidence

> Batch enrichment supports up to 100 identifiers per request. Async terminal states: succeeded, failed, partial_success, cancelled.
> [SOURCE: Context7 library data, verified in prior session]

> MCP endpoint: `https://mcp.thehog.ai/mcp` — supports OAuth authentication and local stdio via `npx -y @thehog/mcp@latest`
> [SOURCE: Hog documentation + session breadcrumbs]

> "Real-time web intelligence API for AI agents and GTM teams. Founded in 2025 by Hudson Liao and Paulo Nascimento, The Hog has 2 employees."
> [VERIFIED: Hog API web_search → ycombinator.com/companies/the-hog]

> "The Hog gives You GaaS (Growth as a Service)"
> [VERIFIED: Hog API x_keyword → x.com/Fondocom/status/1990804296972071049 (2025-11-18)]

> GStack × GBrain hackathon tweet: 186K views, 143 likes, 86 bookmarks
> [VERIFIED: Hog API x_keyword → x.com/TheHogAi/status/2057538500829094024 (2026-05-21)]

## How It Relates

- [[thehog-ai-api]] - The technical interface to TheHog's data
- [[gtm-engineering]] - The discipline this product serves
- [[icp-segmentation]] - Core use case: segment and score leads from Hog data
- [[mcp-integration]] - Agent-native access pattern via MCP protocol
- [[hog-api-enrichment]] - The enrichment workflow powered by Hog
- [[hudson-liao]] - CEO & Co-founder, GTM vision
- [[paulo-nascimento]] - Co-founder & CTO, AI & API architecture

---

**Status:** Validated — confirmed through API docs + enriched via Hog API live calls
**API status:** Live — successfully executed web_search, x_keyword, linkedin_keyword, and people/search endpoints (2026-06-21)

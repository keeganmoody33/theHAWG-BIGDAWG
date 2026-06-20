---
name: HOG_API_SEARCH
description: Search endpoint patterns for TheHog API — company and people discovery queries
domain: technical
node_type: pattern
status: validated
last_updated: 2026-06-20
tags:
  - technical
  - search
  - api-patterns
  - prospecting
topics:
  - company-search
  - people-search
  - query-construction
  - sync-search
  - lead-discovery
related_concepts:
  - "[[thehog-ai-api]]"
  - "[[hog-api-enrichment]]"
  - "[[icp-segmentation]]"
  - "[[gtm-engineering]]"
  - "[[thehog-ai-company]]"
source:
  type: document
  file: "Context7 library + session breadcrumbs"
  date: "2026-06-20"
---

# Hog API Search

Search is the discovery layer of TheHog — how you find companies and people that match your [[icp-segmentation]] criteria before enriching them. The search endpoint supports both async (default) and sync (`sync=true`) modes.

The primary search flow for [[gtm-engineering]] is: construct an ICP query → `POST /api/v1/search` or `POST /api/v1/companies/search` → score results → enrich top matches → deposit into knowledge graph.

## Key Points

- **Company search:** `POST /api/v1/companies/search` — lookup by domain, name, or criteria
- **General search:** `POST /api/v1/search` — supports `sync=true` for immediate results
- **Sync mode:** Pass `sync=true` in request body for blocking response (useful for small queries)
- **Async mode:** Default — returns job ID, poll for results
- **Query construction:** Natural language ICP descriptions work (e.g., "Series A B2B SaaS companies using Clay Apollo HubSpot")
- **Result limits:** Use `limit` parameter to control result count

## Evidence

> POST /api/v1/search can use sync=true
> [SOURCE: Context7 library data]

> Planned first safe test: POST /api/v1/companies/search for "TheHog.ai" with limit 3
> [SOURCE: Prior session breadcrumbs]

## How It Relates

- [[thehog-ai-api]] - Search is one of the three core API capabilities
- [[hog-api-enrichment]] - Search results feed into enrichment workflows
- [[icp-segmentation]] - ICP criteria define what to search for
- [[gtm-engineering]] - Search is the prospecting phase of the GTM loop
- [[icp-scoring-engine]] - Search results get scored before enrichment

---

**Status:** Validated — endpoint and sync mode confirmed via Context7
**Next:** Execute first search query to validate response shape and credit usage

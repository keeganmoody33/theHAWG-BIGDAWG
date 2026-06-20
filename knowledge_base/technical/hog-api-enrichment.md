---
name: HOG_API_ENRICHMENT
description: Enrichment workflows via TheHog API — single and batch identifier resolution with async state tracking
domain: technical
node_type: pattern
status: validated
last_updated: 2026-06-20
tags:
  - technical
  - enrichment
  - api-patterns
  - batch-processing
topics:
  - data-enrichment
  - batch-workflows
  - async-processing
  - identifier-resolution
  - credit-management
related_concepts:
  - "[[thehog-ai-api]]"
  - "[[hog-api-metering]]"
  - "[[icp-scoring-engine]]"
  - "[[thehog-ai-company]]"
  - "[[gtm-engineering]]"
source:
  type: document
  file: "Context7 library + session breadcrumbs"
  date: "2026-06-20"
---

# Hog API Enrichment

Enrichment is the core value loop of TheHog.ai — given identifiers (email, domain, company name, LinkedIn URL), the API returns structured firmographic and contact data suitable for [[icp-scoring-engine]] processing.

Two modes exist: **single enrichment** (sync, immediate response) and **batch enrichment** (async, up to 100 identifiers per request). Batch is the workhorse for [[gtm-engineering]] workflows — submit a list, poll for completion, deposit results into the knowledge graph or CRM.

## Key Points

- Single enrichment: `POST /api/v1/enrich` with `sync=true` for immediate results
- Batch enrichment: Submit array of identifiers, receive job ID, poll for terminal state
- Terminal states: `succeeded`, `failed`, `partial_success`, `cancelled`
- Batch limit: 100 identifiers per request
- Each response includes metering fields for credit tracking (see [[hog-api-metering]])
- Failed identifiers in a batch return `partial_success` — not a full failure

## Evidence

> Batch enrichment supports up to 100 identifiers per request. Async terminal states: succeeded, failed, partial_success, cancelled.
> [SOURCE: Context7 library data]

## How It Relates

- [[thehog-ai-api]] - The API surface these endpoints live on
- [[hog-api-metering]] - Every enrichment call reports credit usage
- [[icp-scoring-engine]] - Enriched data feeds the scoring pipeline
- [[gtm-engineering]] - Enrichment is the data acquisition layer of GTM engineering
- [[outbound-playbook]] - Enriched contact data powers outbound sequences

---

**Status:** Validated — patterns confirmed via Context7 corrections
**Next:** Run batch enrichment on first ICP search results to validate end-to-end flow

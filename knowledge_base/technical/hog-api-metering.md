---
name: HOG_API_METERING
description: Credit metering model for TheHog API — cost fields, rate limits, and stop conditions
domain: technical
node_type: concept
status: validated
last_updated: 2026-06-20
tags:
  - technical
  - metering
  - pricing
  - api-operations
topics:
  - credit-system
  - cost-tracking
  - rate-limiting
  - budget-management
related_concepts:
  - "[[thehog-ai-api]]"
  - "[[hog-api-enrichment]]"
  - "[[thehog-ai-company]]"
  - "[[gtm-engineering]]"
source:
  type: document
  file: "Context7 corrections"
  date: "2026-06-20"
---

# Hog API Metering

TheHog.ai uses a credit-based metering model with transparency — API responses may include cost fields so callers can track spend in real time and implement budget guardrails in their [[gtm-engineering]] workflows. These fields are present on enrichment and search responses but may be absent on some operation types — always use safe access (`.get()`) with fallback defaults.

The metering system is critical for production usage: without monitoring `creditsCharged`, a batch enrichment loop can exhaust credits silently. The stop conditions (`402`, `429`) are the hard circuit breakers the API enforces.

## Key Points

- **`metering.creditsCharged`** — Actual credits consumed by this request
- **`metering.estimatedMaxCredits`** — Upper bound estimate before execution
- **`meta.cost.estimated`** — Pre-execution cost estimate
- **`meta.cost.actual`** — Post-execution actual cost
- **Stop condition `402 Payment Required`** — Credits exhausted, halt all requests
- **Stop condition `429 Too Many Requests`** — Rate limit hit, implement backoff
- Production workflows should check `creditsCharged` after every batch and halt if budget threshold exceeded

## Evidence

> Metering fields: metering.creditsCharged, metering.estimatedMaxCredits, meta.cost.estimated, meta.cost.actual
> [SOURCE: Context7 library data]

> Stop conditions: 402 Payment Required, 429 Too Many Requests
> [SOURCE: Context7 corrections applied in hardening session]

## How It Relates

- [[thehog-ai-api]] - The API these metering fields appear in
- [[hog-api-enrichment]] - Enrichment calls are the primary credit consumers
- [[gtm-engineering]] - Budget management is a core GTM engineering concern
- [[monitor-library]] - Metering data feeds the monitoring patterns

---

**Status:** Validated — confirmed via Context7 library corrections
**Next:** Build credit monitoring wrapper that halts on threshold breach

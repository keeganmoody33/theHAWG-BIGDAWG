---
name: HOG_API_METERING
description: Credit metering model for TheHog API — cost fields, rate limits, and stop conditions
domain: technical
node_type: concept
status: validated
last_updated: 2026-06-22
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
  file: "Context7 corrections + live 402 observation (POST /api/enrichments)"
  date: "2026-06-22"
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

> Live **synchronous** `402` on `POST /api/enrichments` for a LinkedIn-URN contact:
> `{"statusCode":402,"error":"Payment Required","message":"Insufficient credits. Required: 2736, available: 849."}`
> The request was refused synchronously, before execution, with exact required/available figures — this refused enrichment charged nothing.
> [VERIFIED: 2026-06-22 live API call, requestId b8eb610a-39c9-4378-b78e-b40c29257166]

## Live observations (2026-06-22)

- A single-contact enrichment can cost **~2,740 credits** when the identifier is a LinkedIn member-URN URL that the API must first resolve.
- Reducing requested `fields` from `[contact.email, contact.phone, signals]` to `[contact.email]` changed the required cost only marginally (`2740 → 2736`), so **cost is driven by identifier resolution, not the field set**. Budget guardrails must account for resolution cost, not just field count.
- The **synchronous** `402` (email-only attempt) behaves exactly as the [[agent-interface-contract]] prescribes: typed, explicit, recoverable, and emitted **before** any spend — enough for an autonomous agent to stop without overspending.
- **Searches do consume credits.** The balance fell ~1,007 → ~849 between the two enrichment attempts; the only intervening call was a `companies/search`, so that drop is search spend, not the refused enrichment. The earlier async enrichment attempt (`failed` at `progress: 100`) is a separate path from the clean synchronous refusal — only the synchronous `402` is a guaranteed no-charge. Treat the no-deduction guarantee as specific to the **pre-flight synchronous** `402`.

## How It Relates

- [[thehog-ai-api]] - The API these metering fields appear in
- [[hog-api-enrichment]] - Enrichment calls are the primary credit consumers
- [[gtm-engineering]] - Budget management is a core GTM engineering concern
- [[monitor-library]] - Metering data feeds the monitoring patterns

---

**Status:** Validated — confirmed via Context7 library corrections + live `402` observation (2026-06-22)
**Next:** Build credit monitoring wrapper that halts on threshold breach; add a pre-flight cost-estimate step before enrichment, since resolution cost dominates

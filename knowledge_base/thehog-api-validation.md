---
type: validation_log
source: thehog_api
status: active
---

# TheHog API Validation Log

## Connectivity check

- **Date:** 2026-06-20
- **Endpoint:** `POST /api/v1/companies/search`
- **Query:** `TheHog.ai`
- **Limit:** `3`
- **Operation ID:** `6d04edfd-41a1-4330-8202-83bd5e2d6f43`
- **Final status:** `succeeded`
- **Companies returned:** `0`
- **Notes:** Confirmed authentication and async polling work. TheHog.ai does not appear to be indexed as a target company record.

## Simple query validation

- **Date:** 2026-06-20
- **Endpoint:** `POST /api/v1/companies/search`

### Salesforce query

- **Query:** `Salesforce`
- **Limit:** `3`
- **Operation ID:** `3798bcbd-b443-4c40-8f89-f9efeb63319a`
- **Final status:** `succeeded` on first poll
- **Companies returned:** `3`
- **Sample results:**
  - Salesforce (`salesforce.com`)
  - Salesforce Admins (`admin.salesforce.com`)
  - Salesforce Architects (`architect.salesforce.com`)

### Clay query

- **Query:** `Clay`
- **Limit:** `3`
- **Operation ID:** `d844f8dd-03ee-4449-8916-201dfb36e413`
- **Final status:** `succeeded` on second poll
- **Companies returned:** `3`
- **Sample results:**
  - Clay (`clay.com`)
  - Clay (acquired by Kangarootime) (`carebyclay.com`)
  - CLAY TECH (`null`)

## Observations

- **Metering/cost not present in response bodies** for any of the first live calls. This may be due to response shape variation or because charges are applied at the organization level rather than per-operation response.
- **Async polling is required** even for fast queries; `Salesforce` succeeded on the first poll, `Clay` on the second.
- **Natural-language complex queries** (e.g., long GTM wedge query) can remain in `processing` for an extended period; longer polling is needed.
- **Match scores** are not returned in the simple-query payloads; ranking appears to be implicit in result order.

## GTM wedge query

- **Date:** 2026-06-20
- **Endpoint:** `POST /api/v1/companies/search`
- **Query:** `Series A B2B SaaS companies in the US using Clay or Apollo hiring GTM engineering or RevOps`
- **Limit:** `25`
- **Operation ID:** `27cea29e-e48b-49f0-9eeb-ca850c79d604`
- **Status:** `processing` after extended polling (multiple minutes)
- **Observation:** Complex natural-language queries with multiple filters (funding stage, stack, hiring signals, role types) remain queued/processing far longer than simple company-name queries. This is expected behavior for deep GTM searches.
- **Next action:** Continue polling until terminal state; if it eventually times out or fails, simplify the query into two or three single-signal searches.

## Next tests

- Longer poll for GTM wedge operation `27cea29e-e48b-49f0-9eeb-ca850c79d604`.
- `POST /api/v1/people/search` for a known company domain (e.g., `clay.com`).
- `POST /api/v1/enrichment` on a single company identifier to test credit/metering visibility.
- `POST /api/deep-research` with a tight schema once credit costs are confirmed.

## Links

- [[thehog-ai-api]]
- [[thehog-ai-company]]
- [[accounts-gtm-engineering-wedge]]

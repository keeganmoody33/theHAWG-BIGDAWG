---
type: company
name: TheHog.ai
status: seed
source_docs:
  - docs/30-60-90-playbook.md
  - docs/thehog-api-reference.md
---

# TheHog.ai Company

## Atomic facts

- TheHog.ai is described as an AI-native go-to-market command center and real-time web intelligence API.
- The company thesis is that one person with The Hog can wield the power of an entire sales and marketing team.
- The product combines company intelligence, people discovery, contact enrichment, deep research, social listening, multi-platform search, and MCP integration.
- The GTM strategy should dogfood TheHog.ai as the perception layer for account research, signal capture, ICP discovery, and outbound execution.

## Strategic implications

- The workspace should treat TheHog.ai as both subject and tool: research TheHog.ai using its own MCP/API and deposit results back into the graph.
- The initial motion should be company-first: accounts before contacts, signals before outreach.
- Live data should be added only with source, timestamp, and confidence.

## Open questions

- Confirm current rate limits.
- Confirm credit cost for company search, people search, enrichment, monitors, and deep research.
- Confirm whether the repository should remain public once live GTM data is added.

## Live API verification

- **Date:** 2026-06-20
- **Endpoint:** `POST /api/v1/companies/search`
- **Query:** `TheHog.ai`
- **Limit:** `3`
- **Operation ID:** `6d04edfd-41a1-4330-8202-83bd5e2d6f43`
- **Poll URL:** `/api/operations/6d04edfd-41a1-4330-8202-83bd5e2d6f43`
- **Initial status:** `queued`
- **Final status:** `succeeded` after 5 poll attempts (progress `100`)
- **Companies returned:** `0`
- **Metering:** none provided in response
- **Cost:** none provided in response
- **Confidence:** high (direct API call, but no matching company records found)
- **Interpretation:** TheHog.ai likely does not index itself as a target company record, or the query is too literal/self-referential. A broader search term (`AI GTM platform`, `GTM intelligence API`) should be tested next.
- **Source:** `docs.thehog.ai` API via `developer.thehog.ai` (live call)

## Links

- [[thehog-ai-api]]
- [[thehog-ai-icp-segments]]
- [[outbound-playbook]]
- [[competitive-battlecards]]

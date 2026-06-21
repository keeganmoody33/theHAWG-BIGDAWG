# Findings

## 2026-06-20

- Phase 1 workspace foundation exists in `docs/`, `knowledge_base/`, `00_foundation/`, and `plans/`.
- Source docs are split into API reference, customer sessions, and 30-60-90 playbook.
- Live Hog.ai MCP/API calls are intentionally gated until rate limits and credit costs are confirmed.
- Context7 resolved The Hog docs as `/websites/thehog_ai`.
- Credentials must come from environment variables only: `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`, and `THEHOG_MCP_URL`.
- Hosted remote MCP uses OAuth at `https://mcp.thehog.ai/mcp`; local stdio uses `npx -y @thehog/mcp@latest`.
- Async operations can end as `succeeded`, `failed`, `partial_success`, or `cancelled`; `402` and `429` should stop automation.
- First live `POST /api/v1/companies/search` attempt for `TheHog.ai` returned `401 Unauthorized` with message `Authentication required`; credentials were then rotated and the second attempt succeeded with `202 Accepted` and polled to `succeeded` (operation ID `6d04edfd-41a1-4330-8202-83bd5e2d6f43`).
- The query `TheHog.ai` returned `0` companies; this suggests TheHog.ai does not index itself as a target company record, or a broader query is needed. Metering/cost was absent in the response, possibly because no records were materialized.
- Split-query strategy (`companies using Clay`, `companies using Apollo`, `B2B SaaS companies hiring GTM engineering`) yielded actionable accounts fast. The `companies using Clay` query returned `10` companies including Clay itself and GTM services firms like FullFunnel. The `companies using Apollo` query returned `9` companies including Apollo.io and G2.
- People search for FullFunnel surfaced **Neeraj Kumar, Director of GTM Engineering**, an exact buyer persona. Apollo.io returned an Engineering Manager II and a Solution Partner; Clay returned zero matches.
- Metering/cost fields are still absent from live operation responses; credit tracking likely happens at the organization dashboard level, not per-response.
- Complex multi-constraint natural-language queries can remain `processing` for many minutes; the current operation `27cea29e-e48b-49f0-9eeb-ca850c79d604` is still pending.

## 2026-06-21

- Groundskeep emphasized that TheHog.ai should be designed as an agent-first API service: agents should navigate and hold the service in their tool bag, while users should mostly set intent and avoid API mechanics. Deposited the synthesis in [[agent-interface-contract]] and linked it from [[thehog-ai-api]] and [[mcp-tooling-guide]].

## First ingestion queue

- `companies/search` for `TheHog.ai` or `Hog.ai`.
- `v1/search` for a small keyword such as `GTM engineering`.
- Tight-schema `deep-research` for TheHog.ai positioning only after credit/rate-limit details are known.

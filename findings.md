# Findings

## 2026-06-20

- Phase 1 workspace foundation exists in `docs/`, `knowledge_base/`, `00_foundation/`, and `plans/`.
- Source docs are split into API reference, customer sessions, and 30-60-90 playbook.
- Live Hog.ai MCP/API calls are intentionally gated until rate limits and credit costs are confirmed.
- Context7 resolved The Hog docs as `/websites/thehog_ai`.
- Credentials must come from environment variables only: `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`, and `THEHOG_MCP_URL`.
- Hosted remote MCP uses OAuth at `https://mcp.thehog.ai/mcp`; local stdio uses `npx -y @thehog/mcp@latest`.
- Async operations can end as `succeeded`, `failed`, `partial_success`, or `cancelled`; `402` and `429` should stop automation.
- First live `POST /api/v1/companies/search` attempt for `TheHog.ai` returned `401 Unauthorized` with message `Authentication required`; request shape was verified against Context7 docs, so next step is credential rotation or dashboard-side activation before retry.

## First ingestion queue

- `companies/search` for `TheHog.ai` or `Hog.ai`.
- `v1/search` for a small keyword such as `GTM engineering`.
- Tight-schema `deep-research` for TheHog.ai positioning only after credit/rate-limit details are known.

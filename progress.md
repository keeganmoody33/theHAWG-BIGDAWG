# Progress

## Current status

- Phase 1 foundation implemented.
- Phase 2 configuration scaffolding implemented without live API calls.
- Context7 hardening complete against `/websites/thehog_ai` docs.
- Canonical local MCP env vars are `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`, and `THEHOG_MCP_URL`.
- First live API call succeeded with `202 Accepted` and polled to `succeeded` for `POST /api/v1/companies/search`.
- Query `TheHog.ai` returned `0` companies; operation ID `6d04edfd-41a1-4330-8202-83bd5e2d6f43` was recorded.
- `.env` holds real credentials with `600` permissions and is gitignored; `.env.example` is clean placeholders.
- Split-query GTM wedge search produced actionable accounts: `companies using Clay` → 10 accounts; `companies using Apollo` → 9 accounts.
- People search surfaced Neeraj Kumar, Director of GTM Engineering at FullFunnel.
- Deep-research on FullFunnel produced a personalized outbound angle and identified FullFunnel as a Top 4 global Clay Elite Studio Partner.
- First outbound packet is deposited for FullFunnel → Neeraj Kumar.
- Agent-first API/MCP product direction captured in `00_foundation/agent-interface-contract.md`.
- **2026-06-22 delivery prep:** repo root decluttered (`tmp_*.json` moved to `knowledge_base/raw_sources/api-scratch/`); `DELIVERY.md` front-door added.
- **2026-06-22 live run:** `AI SDR companies` search added 2 net-new ICP-2 accounts (Salesforge.ai, AiSDR). Neeraj Kumar enrichment **blocked on credits** (synchronous `402`, required ~2,740, available ~849) — the synchronous `402` refused before execution and charged nothing; documented as a live metering proof point in `[[hog-api-metering]]`. (Note: the balance fell ~1,007→~849 between attempts because an intervening `companies/search` consumed credits — searches do bill; the refused enrichment did not.)
- **Credential note:** the saved `THEHOG_API_KEY` secret does not authenticate as-is (it omits the access key and contains a single transposed character in the secret key). The correct values were available from the secret's description note and written to a local, gitignored `.env`. The stored secret should be corrected so future sessions authenticate directly. (Exact characters intentionally omitted here.)

## Next actions

1. Fix the saved `THEHOG_API_KEY` secret so future sessions auth without manual recovery.
2. Top up Hog credits (~2,740 needed), then re-run `enrichment` on Neeraj Kumar for a verified email; or resolve a cheaper identifier (clean vanity LinkedIn URL / email) first.
3. Translate the agent-first contract into MCP/API acceptance checks: discoverability, typed errors, budget controls, async polling semantics, and action-packet outputs.
4. Store the FullFunnel outbound packet in a campaign folder if multi-account sequences are built.
5. Continue expanding the AI-SDR / agent-builder wedge (Salesforge.ai, AiSDR seeded) with people search.
6. Continue checking the complex GTM wedge operation `27cea29e-e48b-49f0-9eeb-ca850c79d604` when time permits.
7. Capture metering and cost in each deposit.

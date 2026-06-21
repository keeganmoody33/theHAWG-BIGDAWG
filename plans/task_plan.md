# Task Plan

## Operating loop

1. SENSE: read `README.md`, `plans/findings.md`, `progress.md`, `docs/`, `knowledge_base/`, and `00_foundation/` before acting.
2. ORIENT: identify the exact node or foundation doc that owns the work.
3. ACT: use Hog.ai MCP/API calls only when the task requires fresh data.
4. DEPOSIT: write atomic facts into `knowledge_base/`, synthesis into `00_foundation/`, and session updates into `plans/findings.md` and `progress.md`.

## Immediate backlog

- Confirm Hog.ai rate limits and credit costs.
- Validate hosted MCP OAuth or local stdio connectivity with one read-only company search.
- Populate TheHog.ai company node from live result data, operation metadata, and metering.
- Build competitor nodes for Clay, Apollo, ZoomInfo, and Keyplay.
- Create first monitor configs for GTM engineering, Clay alternative, real-time GTM data, and AI sales agents using documented monitor types and cadence minimums.
- Design a 1,000+ contact enrichment orchestration pattern around documented batches of up to 100 identifiers.

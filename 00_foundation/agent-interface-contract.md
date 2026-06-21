# Agent Interface Contract

## Thesis

TheHog.ai should be treated as an agent-first API service. Human users set goals,
constraints, budgets, and approval rules; agents navigate the API/MCP surface,
execute workflows, poll async jobs, handle errors, and return structured action
packets.

## Source

```yaml
source: groundskeep_session_note
retrieved_at: 2026-06-21T10:23:00Z
confidence: high
raw_reference: https://app.devin.ai/sessions/f360422e2c344c4fb4226df6a80630a5
```

## Product invariant

The API and MCP are not secondary developer layers. They are the primary
interaction surface for agentic GTM execution.

The dashboard should work as a cockpit, receipt layer, and approval surface. It
should not require users to manually understand headers, operation IDs, polling,
endpoint choice, enrichment semantics, monitor cadence, or JSON schemas.

## Agent navigation contract

| Agent need | Product contract |
| --- | --- |
| Discover capabilities | Machine-readable docs, MCP tool descriptions, schemas, examples, and `llms.txt` index stay current. |
| Translate intent | Agents can map natural-language GTM goals into endpoint plans without requiring the user to pick endpoints. |
| Execute safely | Every expensive or recurring action exposes budget controls, credit estimates where available, and approval boundaries. |
| Handle async work | Operation IDs, poll URLs, terminal statuses, partial results, and retry guidance are stable and machine-readable. |
| Preserve context | Responses include enough provenance, confidence, metering, and source references to deposit durable graph nodes. |
| Return next actions | Outputs should be action packets: ranked targets, buyer contacts, evidence, recommended next step, and activation handoff. |

## Human boundary

Humans should decide:

- the GTM objective;
- target market and exclusion rules;
- budget and credit ceilings;
- data sensitivity and compliance boundaries;
- whether to approve outreach, enrichment, monitors, or recurring automation.

Agents should decide:

- which endpoint or MCP tool to call;
- whether a workflow needs company search, people search, enrichment, monitor
  events, multi-platform search, or deep research;
- how to poll, retry, branch on terminal statuses, and preserve provenance;
- how to convert raw API output into reusable nodes, campaign packets, and CRM
  handoffs.

## Design implications

- Treat MCP tool descriptions as product copy for agents.
- Keep API errors explicit, typed, and recoverable.
- Prefer schemas and examples over prose-only docs.
- Make budget and metering fields reliable enough for autonomous agents to stop
  before overspend.
- Return structured summaries alongside raw records so agents can reason without
  scraping UI state.
- Design the command center around approvals, observability, and receipts rather
  than manual API operation.

## GTM implication

The strongest wedge is not only "better data." It is a tool-bag-grade perception
layer for GTM agents: one service agents can discover, call, monitor, and deposit
from without forcing the human operator into API mechanics.

## Links

- [[thehog-ai-api]]
- [[mcp-tooling-guide]]
- [[icp-gtm-engineer]]
- [[competitive-battlecards]]

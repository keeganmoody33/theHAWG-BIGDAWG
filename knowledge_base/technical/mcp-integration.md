---
name: MCP_INTEGRATION
description: Model Context Protocol integration for TheHog.ai — agent-native API access via hosted and local MCP
domain: technical
node_type: pattern
status: validated
last_updated: 2026-06-20
tags:
  - technical
  - mcp
  - agent-tooling
  - protocol
topics:
  - model-context-protocol
  - agent-native-access
  - hosted-mcp
  - local-stdio
  - oauth-authentication
related_concepts:
  - "[[thehog-ai-api]]"
  - "[[thehog-ai-company]]"
  - "[[gtm-engineering]]"
  - "[[sense-orient-act-deposit]]"
  - "[[agent-skill-protocol]]"
source:
  type: document
  file: "Hog documentation + session breadcrumbs"
  date: "2026-06-20"
---

# MCP Integration

TheHog.ai provides a Model Context Protocol (MCP) endpoint enabling AI agents to access the full API surface without REST scaffolding. This is the agent-native access pattern — agents discover capabilities, construct queries, and process results through the MCP protocol rather than hand-coded HTTP calls.

Two connection modes exist: **hosted** (cloud endpoint) and **local stdio** (npm package). The hosted endpoint handles auth via OAuth; the local mode uses the same env var credentials as the REST API.

## Key Points

- **Hosted endpoint:** `https://mcp.thehog.ai/mcp` — OAuth authentication
- **Local stdio:** `npx -y @thehog/mcp@latest` — uses `THEHOG_ACCESS_KEY` + `THEHOG_SECRET_KEY`
- **Environment variable:** `THEHOG_MCP_URL=https://mcp.thehog.ai/mcp`
- MCP exposes the same capabilities as the REST API (search, enrich, batch)
- Agents using MCP can self-discover available tools and parameters
- Preferred for [[gtm-engineering]] automation where agents run the [[sense-orient-act-deposit]] loop autonomously

## Evidence

> MCP Connection Options: Hosted: https://mcp.thehog.ai/mcp with OAuth. Local stdio: npx -y @thehog/mcp@latest
> [SOURCE: Hog documentation, verified in hardening session]

## How It Relates

- [[thehog-ai-api]] - MCP wraps the same REST API surface
- [[thehog-ai-company]] - MCP is TheHog's agent-native offering
- [[gtm-engineering]] - MCP enables fully autonomous GTM loops
- [[sense-orient-act-deposit]] - MCP agents follow this protocol
- [[agent-skill-protocol]] - Skills invoke MCP tools as part of the SENSE phase

---

**Status:** Validated — endpoint and local mode confirmed
**Next:** Configure MCP in agent environment and test tool discovery

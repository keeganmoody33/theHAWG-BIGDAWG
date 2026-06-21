# MCP Tooling Guide

> Operational doc — composes from [[mcp-integration]], [[thehog-ai-api]], [[agent-skill-protocol]]

Configuration and usage guide for TheHog.ai's Model Context Protocol (MCP) endpoint. This is the agent-native access pattern — how autonomous agents interact with Hog data.

---

## Connection Modes

### Hosted MCP (Recommended for Production)

```json
{
  "mcpServers": {
    "thehog": {
      "url": "https://mcp.thehog.ai/mcp",
      "transport": "sse",
      "auth": {
        "type": "oauth"
      }
    }
  }
}
```

- Endpoint: `https://mcp.thehog.ai/mcp`
- Auth: OAuth flow (browser-based initial auth, then token refresh)
- Best for: Production agents, always-on GTM loops

### Local stdio (For Development)

```bash
npx -y @thehog/mcp@latest
```

- Auth: Uses `THEHOG_ACCESS_KEY` + `THEHOG_SECRET_KEY` from environment
- Best for: Development, testing, local agent work

### Environment Variables

```bash
# Canonical env var names (not HOG_ACCESS_KEY)
THEHOG_ACCESS_KEY=your_access_key
THEHOG_SECRET_KEY=your_secret_key
THEHOG_MCP_URL=https://mcp.thehog.ai/mcp
```

---

## Available MCP Tools

Once connected, agents can discover these tools via the MCP protocol:

| Tool | Maps to API | Purpose |
|------|-------------|---------|
| `search_companies` | `POST /api/v1/companies/search` | Find companies by criteria |
| `search_people` | `POST /api/v1/people/search` | Find people by criteria |
| `enrich_contact` | `POST /api/enrichments` | Enrich single or batch (up to 100) |
| `search` | `POST /api/v1/search` | Multi-platform search (web, social) |
| `deep_research` | `POST /api/deep-research` | LLM-powered research with JSON Schema |
| `monitors` | `POST /api/v1/monitors` | Recurring social/web monitors |

---

## Agent Workflow: Autonomous Prospecting

An agent using MCP follows the [[sense-orient-act-deposit]] loop:

```
SENSE:  check_credits → verify budget headroom
ORIENT: search_companies → find targets matching ICP from [[icp-segmentation]]
ACT:    enrich_batch → get detailed data on top matches
DEPOSIT: write scored results to knowledge_base/business/accounts-*.md
```

---

## Integration with Skills

The [[agent-skill-protocol]] defines how skills invoke MCP tools:

1. Skill fires based on intent (e.g., "find new accounts matching ICP")
2. SENSE phase: skill calls `check_credits` via MCP
3. ACT phase: skill calls `search_companies` → `enrich_batch` via MCP
4. DEPOSIT phase: skill writes results as knowledge nodes with [[wiki-links]]

---

## Gaps Identified

- [ ] OAuth flow not tested end-to-end
- [ ] MCP tool discovery not validated against live endpoint
- [ ] Rate limiting behavior via MCP not documented
- [ ] No error handling patterns for MCP connection failures
- [ ] Local stdio mode not tested with current `@thehog/mcp` version

---

**References:** [[mcp-integration]] · [[thehog-ai-api]] · [[sense-orient-act-deposit]] · [[agent-skill-protocol]]

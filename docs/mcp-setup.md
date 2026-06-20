# Hog.ai MCP Setup

## Environment variables

Create a local `.env` file from `.env.example` and set the Context7-confirmed local MCP variables:

```bash
THEHOG_ACCESS_KEY=
THEHOG_SECRET_KEY=
THEHOG_MCP_URL=https://mcp.thehog.ai/mcp
```

Do not commit `.env` or credentials.

## Hosted remote MCP

Use hosted remote MCP when the client supports remote MCP with OAuth.

```text
https://mcp.thehog.ai/mcp
```

Claude custom connector pattern:

```text
Name: The Hog
URL: https://mcp.thehog.ai/mcp
```

Hosted MCP auth is handled by OAuth in the MCP client flow. Do not paste access keys into prompts, markdown files, or committed config.

Revoke hosted MCP connections from:

```text
https://platform.thehog.ai/settings/integrations/mcp
```

## Local stdio MCP

Use local stdio when the client does not support remote MCP or when local process control is preferred.

Package and source:

```text
npm: @thehog/mcp
GitHub: The-Hog/the-hog-mcp
```

Run the local server with env vars:

```bash
THEHOG_ACCESS_KEY=... THEHOG_SECRET_KEY=... npx -y @thehog/mcp@latest
```

The local MCP server communicates with:

```text
https://developer.thehog.ai/api
```

## Client checks

After adding a local MCP server, verify registration with the client CLI when available:

```bash
claude mcp list
codex mcp list
```

## Local stdio fallback rules

The wrapper must:

- Read `THEHOG_ACCESS_KEY`, `THEHOG_SECRET_KEY`, and `THEHOG_MCP_URL` from the environment.
- Never persist secrets.
- Apply conservative request pacing.
- Poll async jobs with backoff.
- Log only non-sensitive request metadata.

## First safe connectivity test

Run exactly one low-cost read-only test:

1. `companies/search` for `TheHog.ai` or `Hog.ai`.
2. Poll the returned operation until completion.
3. Capture operation metadata, metering, and relevant company facts in `findings.md`.
4. Update `knowledge_base/thehog-ai-company.md`.

Do not run broad people searches, enrichment, monitors, or deep research until rate limits and credit costs are confirmed.

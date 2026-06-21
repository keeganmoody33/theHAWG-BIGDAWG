---
name: thehawg-bigdawg
description: >
  GTM engineering operating skill for theHAWG-BIGDAWG. Fires when user asks about
  prospecting, enrichment, ICP scoring, outbound campaigns, or competitive positioning.
  Follows SENSE → ORIENT → ACT → DEPOSIT protocol. Use when user says "find accounts",
  "score leads", "run outbound", "check competitive position", or "update the GTM knowledge graph".
---

# theHAWG-BIGDAWG GTM Engineering Skill

Operate the GTM engineering loop for TheHog.ai using the Context OS knowledge graph.

## First-read order

1. `README.md`
2. `progress.md`
3. `plans/findings.md`
4. `plans/task_plan.md`
5. `docs/mcp-setup.md`
6. `docs/thehog-api-reference.md`
7. `knowledge_base/`
8. `00_foundation/`

## Protocol: SENSE → ORIENT → ACT → DEPOSIT

### SENSE — Read the Environment

Before any action, verify current state:

1. **Check graph health:**
   ```bash
   context-os graph-exec --graph knowledge_base '(() => {
     const r = codemode.graph_query({ filter: {}, limit: 500 });
     const orphans = r.nodes.filter(n => n.link_count.outbound === 0 && n.link_count.inbound === 0);
     return JSON.stringify({ total: r.total, orphans: orphans.length });
   })()'
   ```

2. **Check what exists:** Read `00_foundation/_synthesis/gtm-context-os-synthesis.md` for current state overview.

3. **Check credentials:** Verify `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY` are set.

4. **Check credit budget:** If making API calls, verify budget headroom first.

### ORIENT — Find What Matters

1. **Identify hub nodes:**
   ```bash
   context-os graph-exec --graph knowledge_base '(() => {
     const r = codemode.graph_query({ filter: {}, limit: 500 });
     return JSON.stringify(r.nodes.sort((a,b) =>
       (b.link_count.outbound+b.link_count.inbound)-(a.link_count.outbound+a.link_count.inbound)
     ).slice(0,5).map(n => ({ name: n.name, links: n.link_count.outbound+n.link_count.inbound })));
   })()'
   ```

2. **Read relevant ICP segment:** Match the user's intent to [[icp-founder-solo]], [[icp-gtm-engineer]], or [[icp-revops-head]].

3. **Read scoring criteria:** Check `00_foundation/positioning/icp-scoring-engine.md` for current weights.

4. **Pick the owner document:**
   - Atomic company/product/persona facts → `knowledge_base/`
   - Strategy, scoring, playbooks, reusable systems → `00_foundation/`
   - Session observations → `plans/findings.md`
   - Status and next steps → `progress.md`

### ACT — Execute the Task

**For prospecting:**
- Use [[hog-api-search]] patterns to construct queries
- Execute via [[mcp-integration]] or REST API
- Score results using [[icp-scoring-engine]] weights

**For outbound:**
- Select template track from `00_foundation/messaging/outbound-playbook.md`
- Personalize using enrichment data
- Log campaign to knowledge graph

**For competitive research:**
- Start with `00_foundation/positioning/competitive-battlecards.md`
- Update with new intelligence
- Link findings to relevant ICP segments

### DEPOSIT — Reinforce the Graph

**Every action must produce a deposit:**

1. Create or update knowledge node with proper frontmatter
2. Link to at least 2 existing nodes via wiki-links (e.g. `[[gtm-engineering]]`)
3. Update `plans/findings.md` if gaps are discovered
4. Update synthesis doc if material state changes

## MCP Connection

Use environment variables only:

- `THEHOG_ACCESS_KEY`
- `THEHOG_SECRET_KEY`
- `THEHOG_MCP_URL`

Default hosted MCP URL:

```text
https://mcp.thehog.ai/mcp
```

Hosted MCP uses OAuth when the client supports remote MCP. Local stdio uses `npx -y @thehog/mcp@latest` with `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY` in the process environment.

Do not hardcode credentials. Do not paste credentials into prompts or markdown.

## Rate-limit and Async Discipline

- Treat 5 requests per second as an unconfirmed ceiling; prefer lower throughput until official limits are known.
- Poll async jobs with backoff.
- Handle terminal statuses: `succeeded`, `failed`, `partial_success`, and `cancelled`.
- Deep research polling should be slower than enrichment polling.
- Stop on repeated failures, 402s, 429s, or unclear credit usage.

## Safe First Ingestion

1. Run `companies/search` for `TheHog.ai` or `Hog.ai`.
2. Poll until terminal state.
3. Add confirmed facts to `knowledge_base/business/thehog-ai-company.md`.
4. Log operation metadata and summary in `plans/findings.md`.
5. Update `progress.md`.

## Quality Gates

- [ ] SENSE phase completed (graph health checked, state verified)?
- [ ] ORIENT phase completed (hub nodes identified, relevant segments read)?
- [ ] ACT phase produced verifiable output?
- [ ] DEPOSIT phase linked output to graph (no orphan nodes)?
- [ ] Attribution tags on every claim?

## Intent Matching

| User says | Do this |
|-----------|---------|
| "find accounts" / "prospect" | Search → Enrich → Score → Deposit |
| "score leads" | Read enrichment data → Apply scoring → Deposit ranked list |
| "run outbound" | Read scored accounts → Select templates → Generate outbound |
| "competitive intel" | Read battlecards → Research → Update → Deposit |
| "graph health" | Run graph-exec health queries → Report → Fix orphans |
| "what's missing" | Read plans/findings.md → Run gap analysis → Update |

## Never Do

- Never commit `.env`.
- Never hardcode secrets.
- Never run credit-heavy jobs without explicit intent.
- Never overwrite existing graph facts without preserving provenance.
- Never skip the DEPOSIT step after discovering reusable information.

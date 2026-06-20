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
2. Link to at least 2 existing nodes via `[[wiki-links]]`
3. Update `plans/findings.md` if gaps are discovered
4. Update synthesis doc if material state changes

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
| "what's missing" | Read findings.md → Run gap analysis → Update |

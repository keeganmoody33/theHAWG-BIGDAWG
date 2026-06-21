# theHAWG-BIGDAWG Context OS

GTM engineering experiment — engineering applied to market entry using [[thehog-ai-company]]'s enrichment API. 90-day rollout via [[thirty-sixty-ninety-plan]].

**Philosophy:** Your taste is primary. The system structures and compounds it. Agents coordinate by reading and modifying the shared environment — not by following procedures.

---

## Directory Structure

```
theHAWG-BIGDAWG/
├── CLAUDE.md                          # ← YOU ARE HERE
├── taxonomy.yaml                      # Blessed tags and domains
├── ontology.yaml                      # Relationship types and linking rules
│
├── knowledge_base/                    # Atomic concepts — the graph
│   ├── technical/                     # API, enrichment, metering, MCP
│   │   ├── thehog-ai-api.md          # REST API surface
│   │   ├── hog-api-enrichment.md     # Enrichment workflows
│   │   ├── hog-api-metering.md       # Credit metering model
│   │   ├── hog-api-search.md         # Search endpoint patterns
│   │   └── mcp-integration.md        # MCP agent-native access
│   ├── business/                      # Company, ICP, segmentation, people
│   │   ├── thehog-ai-company.md      # Company profile + value prop
│   │   ├── hudson-liao.md             # CEO & Co-founder
│   │   ├── paulo-nascimento.md        # Co-founder, AI engineering
│   │   ├── icp-segmentation.md       # Segmentation framework
│   │   ├── icp-founder-solo.md       # Solo founder segment
│   │   ├── icp-gtm-engineer.md       # GTM engineer segment
│   │   └── icp-revops-head.md        # RevOps head segment
│   ├── methodology/                   # Frameworks, protocols, architecture
│   │   ├── gtm-engineering.md        # Core thesis: engineering → GTM
│   │   ├── sense-orient-act-deposit.md  # SOAD operating loop
│   │   ├── thirty-sixty-ninety-plan.md  # Phased rollout milestones
│   │   ├── agent-skill-protocol.md   # Skill format and firing
│   │   └── context-os-architecture.md   # Two-layer architecture
│   ├── emergent/                      # New concepts not yet validated
│   └── raw_sources/                   # Transcripts, notes, raw input
│
├── 00_foundation/                     # Operational docs that compose from the graph
│   ├── agent-interface-contract.md   # API/MCP invariants: approval, budget, async, action-packets
│   ├── positioning/                   # How we position TheHog
│   │   ├── icp-scoring-engine.md     # Scoring model + weights (emergent — untested)
│   │   ├── competitive-battlecards.md # vs. ZoomInfo, Apollo, Clay
│   │   ├── mcp-tooling-guide.md      # MCP setup and agent workflows
│   │   └── monitor-library.md        # Credit monitoring patterns
│   ├── messaging/                     # Value props, templates
│   │   └── outbound-playbook.md      # Email templates per ICP segment (draft — untested)
│   └── _synthesis/                    # Summary documents
│       └── gtm-context-os-synthesis.md  # The 5% that answers 95%
│
├── templates/                         # Node and doc templates
│   └── node_template.md              # Knowledge node frontmatter format
│
├── plans/                             # Task tracking
│   └── findings.md                   # Graph health audit + gap analysis
│
└── .claude/skills/                    # Agent skills
    └── thehawg-bigdawg/
        └── SKILL.md                   # SENSE → ORIENT → ACT → DEPOSIT protocol
```

---

## How to Work in This System

Every task follows: **SENSE → ORIENT → ACT → DEPOSIT.**

See [[sense-orient-act-deposit]] for the full protocol.

### Knowledge Graph

**SENSE** — Check what exists before editing:
```bash
# Graph health: total nodes and orphans
context-os graph-exec --graph knowledge_base '(() => {
  const r = codemode.graph_query({ filter: {}, limit: 500 });
  const orphans = r.nodes.filter(n => n.link_count.outbound === 0 && n.link_count.inbound === 0);
  return JSON.stringify({ total: r.total, orphans: orphans.length });
})()'
```

**ORIENT** — Find the hub nodes (most connected concepts):
```bash
context-os graph-exec --graph knowledge_base '(() => {
  const r = codemode.graph_query({ filter: {}, limit: 500 });
  return JSON.stringify(r.nodes.sort((a,b) =>
    (b.link_count.outbound+b.link_count.inbound)-(a.link_count.outbound+a.link_count.inbound)
  ).slice(0,5).map(n => ({ name: n.name, links: n.link_count.outbound+n.link_count.inbound })));
})()'
```

**ACT** — New nodes get `status: emergent`. Promote to `validated` with 2+ real-world citations. That's the full lifecycle.

**DEPOSIT** — Link new nodes to existing ones via wiki-links (e.g. `[[gtm-engineering]]`). If you see orphans, link them.

### Operational Docs

Foundation docs in `00_foundation/` **compose from** knowledge graph concepts. They reference atomic nodes via wiki-links (e.g. `[[thehog-ai-company]]`), they don't redefine them.

### Quick Navigation

| I need to understand... | Start here |
|-------------------------|------------|
| What TheHog.ai is | [[thehog-ai-company]] |
| How the API works | [[thehog-ai-api]] → [[hog-api-enrichment]] → [[hog-api-search]] |
| Who founded TheHog | [[hudson-liao]] (CEO) + [[paulo-nascimento]] (AI/API) |
| Who we sell to | [[icp-segmentation]] → segment nodes |
| How to score accounts | `00_foundation/positioning/icp-scoring-engine.md` |
| What to send prospects | `00_foundation/messaging/outbound-playbook.md` |
| Competitive positioning | `00_foundation/positioning/competitive-battlecards.md` |
| The project timeline | [[thirty-sixty-ninety-plan]] |
| How this system works | [[context-os-architecture]] → [[sense-orient-act-deposit]] |
| What's missing | `plans/findings.md` |
| The executive summary | `00_foundation/_synthesis/gtm-context-os-synthesis.md` |

---

## Ground Rules

### Attribution

**Every claim must have a tag.** No exceptions.

```
[VERIFIED: file:line]    — you read the file and it confirms the claim
[INFERRED: from X + Y]   — you deduced it from multiple sources
[UNVERIFIABLE]            — you cannot confirm it. Say so.
```

### Epistemic Humility

Before claiming system state, verify with a tool. `graph-exec` for graph structure. Don't guess from memory.

### Credentials

```bash
# Canonical env var names (NOT HOG_ACCESS_KEY)
THEHOG_ACCESS_KEY=<from .env>
THEHOG_SECRET_KEY=<from .env>
THEHOG_MCP_URL=https://mcp.thehog.ai/mcp
```

---

## Context-OS CLI

```bash
context-os context "<query>"              # Broad project context
context-os query heat --time <N>d         # What's active
context-os query co-access <file>         # Files that travel together
context-os graph-exec --graph <path> '<js>' # Structural graph queries
```

Graph-exec functions are **synchronous** — wrap in IIFE, use `return`:
`'(() => { const r = codemode.graph_search({ pattern: "X" }); return JSON.stringify(r); })()'`

---

**Created:** 2026-06-20
**Architecture:** [[context-os-architecture]]
**Operating loop:** [[sense-orient-act-deposit]]

---
name: CONTEXT_OS_ARCHITECTURE
description: Two-layer Context OS architecture — atomic knowledge graph + operational docs with stigmergic coordination
domain: methodology
node_type: framework
status: validated
last_updated: 2026-06-20
tags:
  - methodology
  - architecture
  - context-os
  - knowledge-graph
topics:
  - two-layer-architecture
  - atomic-concepts
  - operational-docs
  - wiki-links
  - knowledge-lifecycle
  - stigmergic-coordination
related_concepts:
  - "[[sense-orient-act-deposit]]"
  - "[[agent-skill-protocol]]"
  - "[[gtm-engineering]]"
  - "[[icp-segmentation]]"
  - "[[thehog-ai-company]]"
source:
  type: document
  file: "context-os33 CLAUDE.md + context-os-basics skill"
  date: "2026-06-20"
---

# Context OS Architecture

The Context OS is a two-layer knowledge architecture where AI compounds intelligence over time. Layer 1 (knowledge_base/) holds atomic, reusable concepts linked via wiki-links (e.g. `[[node-name]]`). Layer 2 (00_foundation/) holds operational documents that compose from the graph — they reference atomic concepts, they don't redefine them.

Coordination is stigmergic: agents read and modify the shared environment rather than following rigid procedures. Knowledge compounds through use — the more a node is linked and referenced, the more canonical it becomes.

## Key Points

- **Layer 1: Knowledge Graph** (`knowledge_base/`) — atomic concepts with frontmatter metadata
  - Domains: `technical/`, `business/`, `methodology/`, `emergent/`
  - Lifecycle: `emergent` → `validated` (2+ citations) → `canonical`
  - Linked via wiki-links (e.g. `[[node-name]]`) — the graph has structure, not just files
  
- **Layer 2: Operational Docs** (`00_foundation/`) — strategic artifacts
  - Compose FROM the graph — reference atomic concepts via wiki-links
  - Areas: `positioning/`, `messaging/`, `_synthesis/`
  - Never redefine what a knowledge node already covers

- **Constitutional docs:** `taxonomy.yaml` (blessed tags) + `ontology.yaml` (relationship types)
- **Anti-pattern:** Orphan nodes (no links) — knowledge that isn't linked is lost

## Evidence

> Two-Layer Architecture: Knowledge Graph (knowledge_base/) — Atomic, reusable concepts with frontmatter metadata. Operational Docs (00_foundation/) — Strategic documents that compose from the graph.
> [SOURCE: context-os33 CLAUDE.md]

## How It Relates

- [[sense-orient-act-deposit]] - The runtime behavior of this architecture
- [[agent-skill-protocol]] - Skills interact with both layers
- [[gtm-engineering]] - This architecture structures GTM knowledge
- [[icp-segmentation]] - ICP segments are knowledge nodes; scoring logic is an operational doc

---

**Status:** Validated — architecture proven in context-os33 across multiple builds
**Next:** Verify graph health — orphan count, link density, hub identification

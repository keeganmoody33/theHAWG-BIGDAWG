---
name: SENSE_ORIENT_ACT_DEPOSIT
description: Core operating loop for Context OS and GTM engineering — check state, find hubs, act, reinforce the graph
domain: methodology
node_type: framework
status: validated
last_updated: 2026-06-20
tags:
  - methodology
  - operating-loop
  - context-os
  - workflow
topics:
  - agent-protocol
  - knowledge-compounding
  - stigmergic-coordination
  - graph-reinforcement
  - decision-loop
related_concepts:
  - "[[gtm-engineering]]"
  - "[[agent-skill-protocol]]"
  - "[[context-os-architecture]]"
  - "[[mcp-integration]]"
  - "[[thehog-ai-company]]"
source:
  type: document
  file: ".claude/skills/thehawg-bigdawg/SKILL.md + context-os33 patterns"
  date: "2026-06-20"
---

# SENSE → ORIENT → ACT → DEPOSIT

The SOAD loop is the core operating protocol for every interaction in this Context OS, adapted from the OODA loop for knowledge-graph-native work. Every agent session, every GTM engineering task, every knowledge update follows this cycle.

This is not a process document — it's the fundamental rhythm of how work compounds. Each cycle through the loop reinforces the knowledge graph: SENSE reads existing state, ORIENT identifies what matters, ACT creates or modifies, DEPOSIT links the result back into the graph. Without DEPOSIT, work is ephemeral.

## Key Points

- **SENSE** — Check what exists. Query the graph, read hot files, verify current state before acting. Never assume state from memory.
- **ORIENT** — Find hub nodes (most-connected concepts). Read coordination surfaces. Identify where new work connects to existing knowledge.
- **ACT** — Create or update content. New nodes start as `emergent`. Execute the task.
- **DEPOSIT** — Link new work to existing nodes via wiki-links (e.g. `[[node-name]]`). If you see orphans, link them. Knowledge that isn't linked is lost.

## Evidence

> Every interaction follows this loop: SENSE — Check what exists. ORIENT — Find hub nodes. ACT — Create/update content. DEPOSIT — Link to existing nodes.
> [SOURCE: context-os33 CLAUDE.md and skill protocols]

## How It Relates

- [[gtm-engineering]] - GTM engineering follows SOAD: sense market → orient on ICP → act on outbound → deposit results
- [[agent-skill-protocol]] - Skills are structured as SOAD cycles
- [[context-os-architecture]] - SOAD is the runtime behavior of the architecture
- [[mcp-integration]] - MCP enables autonomous SOAD loops for agents

---

**Status:** Validated — core protocol from context-os33, proven across multiple builds
**Next:** Every subsequent task in this repo must demonstrate SOAD compliance

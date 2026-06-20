---
name: AGENT_SKILL_PROTOCOL
description: Structured skill format for agent-executable GTM workflows — SOAD-compliant, frontmatter-tagged
domain: methodology
node_type: pattern
status: validated
last_updated: 2026-06-20
tags:
  - methodology
  - agent-skills
  - automation
  - protocol
topics:
  - skill-definition
  - agent-automation
  - intent-matching
  - soad-compliance
  - context-os-skills
related_concepts:
  - "[[sense-orient-act-deposit]]"
  - "[[gtm-engineering]]"
  - "[[context-os-architecture]]"
  - "[[mcp-integration]]"
  - "[[thehog-ai-company]]"
source:
  type: document
  file: "context-os33 skill patterns"
  date: "2026-06-20"
---

# Agent Skill Protocol

Skills are the executable units of a Context OS — structured procedures that agents discover and fire based on intent. Each skill follows the [[sense-orient-act-deposit]] loop and produces traceable outputs linked into the knowledge graph.

The skill format originated in context-os33 (Jacob Deedlewood's reference implementation) and has been adapted for theHAWG-BIGDAWG's GTM context. Skills fire based on natural language intent matching — no slash commands needed.

## Key Points

- Skills live in `.claude/skills/[skill-name]/SKILL.md` (or `.agents/skills/` for multi-agent repos)
- Each skill has YAML frontmatter with `name`, `description`, and optional `model` fields
- The description doubles as intent-matching criteria — agents read it to decide when to fire
- Skills reference `references/` subdirectories for supporting material
- Every skill must produce a DEPOSIT step — output that links back into the graph
- Skills should be idempotent: running the same skill twice produces consistent results

## Evidence

> Agent Skill: .claude/skills/thehawg-bigdawg/SKILL.md with SENSE → ORIENT → ACT → DEPOSIT protocol
> [SOURCE: Prior session breadcrumbs]

## How It Relates

- [[sense-orient-act-deposit]] - Every skill follows the SOAD loop
- [[gtm-engineering]] - GTM skills automate prospecting, scoring, and outbound
- [[context-os-architecture]] - Skills are the action layer of the architecture
- [[mcp-integration]] - Skills can invoke MCP tools in the SENSE phase

---

**Status:** Validated — pattern proven in context-os33 across 10+ skills
**Next:** Create theHAWG-BIGDAWG-specific skills (prospect, score, outbound)

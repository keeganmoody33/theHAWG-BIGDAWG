---
title: GTM Context OS Synthesis
type: synthesis-doc
status: validated
source_concepts:
  - "[[gtm-engineering]]"
  - "[[thehog-ai-company]]"
  - "[[context-os-architecture]]"
  - "[[thirty-sixty-ninety-plan]]"
last_updated: 2026-06-21
---

# theHAWG-BIGDAWG Context OS Synthesis

> Synthesis doc — the 5% that answers 95% of questions about this system

---

## What This Is

A GTM engineering Context OS built around [[thehog-ai-company]]'s enrichment API. The system automates a 90-day GTM experiment: discover prospects → enrich data → score accounts → send targeted outbound.

## Architecture

Two layers per [[context-os-architecture]]:

| Layer | Path | Purpose |
|-------|------|---------|
| **Knowledge Graph** | `knowledge_base/` | Atomic concepts — company, API, ICPs, methodology |
| **Operational Docs** | `00_foundation/` | Scoring engine, playbooks, battlecards, monitors |

Operating loop: [[sense-orient-act-deposit]] — every task reads state, orients on hubs, acts, deposits results.

## Key Nodes (Hub Concepts)

| Node | Domain | Links | Role |
|------|--------|-------|------|
| [[gtm-engineering]] | methodology | 8 | Core thesis — engineering applied to GTM |
| [[thehog-ai-company]] | business | 7 | The platform powering the experiment |
| [[icp-segmentation]] | business | 6 | How targets are identified and bucketed |
| [[thehog-ai-api]] | technical | 6 | The technical interface to Hog data |
| [[sense-orient-act-deposit]] | methodology | 5 | The operating protocol |

## Founders

| Person | Role | Owns |
|--------|------|------|
| [[hudson-liao]] | CEO & Co-founder | GTM strategy, product vision, growth ops |
| [[paulo-nascimento]] | Co-founder & CTO | AI engineering, API architecture, MCP |

YC F25 batch. 2 employees as of last check.

## ICP Segments

| Segment | Node | Key Signal | Template Track |
|---------|------|------------|----------------|
| Solo Founder | [[icp-founder-solo]] | Pre-seed/seed, <10 headcount | Founder-Fast |
| GTM Engineer | [[icp-gtm-engineer]] | Uses Clay/Apollo, hires for GTM | Technical Depth |
| RevOps Head | [[icp-revops-head]] | 3+ enrichment vendors, 50+ headcount | ROI / Consolidation |

## Current Status

| Phase | Timeline | Status |
|-------|----------|--------|
| Foundation | Days 1–30 | **IN PROGRESS** — knowledge graph seeded, scoring designed |
| Execution | Days 31–60 | Not started |
| Scale | Days 61–90 | Not started |

Per [[thirty-sixty-ninety-plan]].

## What's Missing (see plans/findings.md)

Progress since initial build:
1. ~~No live API call executed~~ — **RESOLVED.** Live API calls confirmed: `web_search`, `x_keyword`, `linkedin_keyword`, `people/search`, `companies/search`. Founder nodes enriched via Hog API self-discovery.
2. Scoring weights are theoretical — need real enrichment data to calibrate
3. Outbound templates untested — no A/B framework
4. Competitive match rate data is estimated, not measured
5. MCP endpoint not tested end-to-end

---

**Last updated:** 2026-06-20
**References:** [[gtm-engineering]] · [[thirty-sixty-ninety-plan]] · [[context-os-architecture]] · [[thehog-ai-company]] · [[hudson-liao]] · [[paulo-nascimento]]

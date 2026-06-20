---
name: THIRTY_SIXTY_NINETY_PLAN
description: Phased 30-60-90 day milestone framework for GTM engineering rollout execution
domain: methodology
node_type: framework
status: validated
last_updated: 2026-06-20
tags:
  - methodology
  - execution
  - milestones
  - timeline
topics:
  - phased-rollout
  - milestone-framework
  - feedback-loop
  - gtm-timeline
  - execution-cadence
related_concepts:
  - "[[gtm-engineering]]"
  - "[[icp-segmentation]]"
  - "[[icp-scoring-engine]]"
  - "[[outbound-playbook]]"
  - "[[thehog-ai-company]]"
source:
  type: document
  file: "README.md"
  date: "2026-06-20"
---

# 30-60-90 Day Plan

The 30-60-90 day plan is the strategic layer of theHAWG-BIGDAWG — the phased milestone framework defining what gets built and when. Each phase builds on the previous, with explicit success criteria and a feedback loop tracking metrics against milestones.

This is not a waterfall plan — it's a series of progressively larger bets. Day 30 proves the data works. Day 60 proves the outbound works. Day 90 proves it scales.

## Key Points

### Days 1–30: Foundation
- Stand up [[icp-scoring-engine]] with initial criteria
- Execute first [[hog-api-search]] queries against ICP hypothesis
- Validate data quality via [[hog-api-enrichment]] on sample set
- **Success criteria:** 50+ scored accounts, data quality validated, first outbound draft

### Days 31–60: Execution
- Launch first outbound campaign using [[outbound-playbook]] templates
- Iterate [[icp-segmentation]] based on response data
- Build [[competitive-battlecards]] from market research
- **Success criteria:** 100+ outbound touches, measurable response rate, refined ICP

### Days 61–90: Scale
- Automate enrichment pipeline with batch workflows
- Integrate [[mcp-integration]] for agent-autonomous prospecting
- Build feedback loop: outbound response → ICP refinement → enrichment → outbound
- **Success criteria:** Self-sustaining GTM loop, documented methodology, replicable playbook

## Evidence

> Focused on metrics, realistic timelines, and execution-ready outputs for a practical GTM case study.
> [SOURCE: README.md]

## How It Relates

- [[gtm-engineering]] - The 30-60-90 is the strategic layer of GTM engineering
- [[icp-scoring-engine]] - Built in Phase 1 (Days 1–30)
- [[outbound-playbook]] - Launched in Phase 2 (Days 31–60)
- [[mcp-integration]] - Integrated in Phase 3 (Days 61–90)
- [[icp-segmentation]] - Iterated across all three phases

---

**Status:** Validated — defined in README.md as core project structure
**Next:** Track current phase and milestone progress in plans/progress.md

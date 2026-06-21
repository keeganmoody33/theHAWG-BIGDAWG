---
name: ICP_SEGMENTATION
description: Ideal Customer Profile segmentation — bucketing targets by fit score for tailored outbound
domain: business
node_type: framework
status: validated
last_updated: 2026-06-20
tags:
  - business
  - icp
  - segmentation
  - lead-scoring
topics:
  - ideal-customer-profile
  - market-segmentation
  - fit-scoring
  - target-account-selection
  - outbound-targeting
related_concepts:
  - "[[icp-scoring-engine]]"
  - "[[icp-founder-solo]]"
  - "[[icp-gtm-engineer]]"
  - "[[icp-revops-head]]"
  - "[[thehog-ai-company]]"
  - "[[gtm-engineering]]"
  - "[[hog-api-search]]"
source:
  type: document
  file: "README.md + prior session context"
  date: "2026-06-20"
---

# ICP Segmentation

ICP (Ideal Customer Profile) segmentation is the analytical layer of theHAWG-BIGDAWG — programmatic lead and account fit quantification. Rather than manual list building, segmentation uses data from [[thehog-ai-company]] to score and bucket targets for tailored outbound.

The framework defines three primary ICP segments: [[icp-founder-solo]] (solo founders / early-stage), [[icp-gtm-engineer]] (GTM engineering practitioners), and [[icp-revops-head]] (RevOps leadership). Each segment has distinct pain points, decision criteria, and messaging angles processed through the [[icp-scoring-engine]].

## Key Points

- Segmentation is programmatic — scores computed from enriched data, not gut feel
- Three validated ICP segments with distinct messaging tracks
- Scoring inputs: firmographic data (stage, headcount, funding) + technographic signals (tool stack)
- Output: ranked account lists bucketed by fit score for differentiated outbound
- Segments feed directly into [[outbound-playbook]] template selection

## Evidence

> ICP segmentation with scoring logic, sample outbound emails, and a creative working prototype using the Hog API.
> [SOURCE: README.md]

## How It Relates

- [[icp-scoring-engine]] - The engine that computes fit scores per segment
- [[icp-founder-solo]] - Solo founder / early-stage segment definition
- [[icp-gtm-engineer]] - GTM engineer segment definition
- [[icp-revops-head]] - RevOps head segment definition
- [[hog-api-search]] - Search queries constructed from ICP criteria
- [[outbound-playbook]] - Segmented results trigger differentiated outbound

---

**Status:** Validated — three segments defined, scoring logic conceptualized
**Next:** Score first batch of enriched accounts against segment criteria

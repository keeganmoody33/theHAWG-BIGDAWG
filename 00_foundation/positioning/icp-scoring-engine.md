# ICP Scoring Engine

> Operational doc — composes from [[icp-segmentation]], [[icp-founder-solo]], [[icp-gtm-engineer]], [[icp-revops-head]]

The scoring engine is the analytical core of theHAWG-BIGDAWG — it takes enriched account data from [[hog-api-enrichment]] and computes a fit score per [[icp-segmentation]] segment. Scored accounts feed the [[outbound-playbook]] for template selection.

---

## Scoring Model

### Input: Enriched Account Data

Fields consumed from [[thehog-ai-api]] enrichment response:

| Field | Source | Weight |
|-------|--------|--------|
| `company.stage` | Firmographic | HIGH — filters by funding round |
| `company.headcount` | Firmographic | MEDIUM — size proxy |
| `company.industry` | Firmographic | MEDIUM — B2B SaaS filter |
| `company.techStack` | Technographic | HIGH — tool signals (Clay, Apollo, HubSpot) |
| `company.hiring` | Job postings | HIGH — hiring GTM/RevOps = buying signal |
| `company.funding` | Firmographic | LOW — context only |

### Scoring Formula (v0.1 — emergent)

```
score = (stage_match * 0.30)
      + (headcount_fit * 0.15)
      + (industry_match * 0.15)
      + (tech_stack_signals * 0.25)
      + (hiring_signals * 0.15)
```

Each factor returns 0–100. Final score is 0–100.

### Segment-Specific Weights

| Factor | [[icp-founder-solo]] | [[icp-gtm-engineer]] | [[icp-revops-head]] |
|--------|---------------------|---------------------|-------------------|
| Stage match | 0.40 | 0.20 | 0.15 |
| Headcount fit | 0.10 | 0.15 | 0.25 |
| Industry match | 0.15 | 0.15 | 0.15 |
| Tech stack signals | 0.20 | 0.35 | 0.25 |
| Hiring signals | 0.15 | 0.15 | 0.20 |

---

## Score Buckets

| Score Range | Bucket | Action |
|-------------|--------|--------|
| 80–100 | **HOT** | Immediate outbound via [[outbound-playbook]] |
| 60–79 | **WARM** | Queue for enrichment, then outbound |
| 40–59 | **COOL** | Monitor — re-score monthly |
| 0–39 | **COLD** | Archive — does not fit current ICP |

---

## Implementation Status

| Component | Status | Notes |
|-----------|--------|-------|
| Scoring formula | Designed | Needs validation against real data |
| Segment weights | Designed | Needs A/B testing per segment |
| Score bucketing | Designed | Thresholds are v0.1 estimates |
| API integration | Not started | Requires [[hog-api-enrichment]] output mapping |
| Automated pipeline | Not started | Phase 2 of [[thirty-sixty-ninety-plan]] |

---

## Gaps Identified

- [ ] No real enrichment data to validate scoring weights against
- [ ] Tech stack signal list needs specific tool names per segment
- [ ] Hiring signal keywords not yet defined
- [ ] No feedback loop from outbound response → score recalibration
- [ ] Score bucketing thresholds are arbitrary — need empirical validation

---

**References:** [[icp-segmentation]] · [[icp-scoring-engine]] · [[hog-api-enrichment]] · [[thirty-sixty-ninety-plan]]

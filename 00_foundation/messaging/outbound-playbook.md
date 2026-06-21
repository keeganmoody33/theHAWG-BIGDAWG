---
title: Outbound Playbook
type: operational-doc
status: validated
source_concepts:
  - "[[icp-segmentation]]"
  - "[[icp-founder-solo]]"
  - "[[icp-gtm-engineer]]"
  - "[[icp-revops-head]]"
  - "[[competitive-battlecards]]"
last_updated: 2026-06-20
---

# Outbound Playbook

> Operational doc — composes from [[icp-segmentation]], [[icp-founder-solo]], [[icp-gtm-engineer]], [[icp-revops-head]], [[competitive-battlecards]]

The outbound playbook defines email templates, sequencing logic, and personalization rules triggered by [[icp-scoring-engine]] output. Each ICP segment gets a differentiated template track.

---

## Template Tracks

### Track 1: Founder-Fast (→ [[icp-founder-solo]])

**Trigger:** Score ≥ 80, segment = founder-solo
**Tone:** Direct, time-conscious, empathetic to founder grind
**Key message:** "Your first 50 qualified leads in 30 minutes, not 30 hours"

```
Subject: [FirstName], still building your pipeline in spreadsheets?

Body frame:
- Hook: acknowledge the manual prospecting pain
- Bridge: what if enrichment was one API call
- Proof: specific data point (batch 100 leads, enriched in <5 min)
- CTA: "Reply and I'll show you your first 50 leads in your ICP"
```

**Sequence:** 3 touches over 10 days (Day 0, Day 4, Day 10)

### Track 2: Technical Depth (→ [[icp-gtm-engineer]])

**Trigger:** Score ≥ 80, segment = gtm-engineer
**Tone:** Technical, peer-to-peer, API-first
**Key message:** "Enrichment API that a GTM engineer actually wants to use"

```
Subject: batch enrichment without the Clay tax

Body frame:
- Hook: acknowledge API frustration with current tools
- Bridge: Hog API — batch, async, metered, MCP-native
- Proof: code snippet showing batch enrichment call
- CTA: "Here's a working curl — try it on your ICP"
```

**Sequence:** 3 touches over 14 days (Day 0, Day 5, Day 14)

### Track 3: ROI / Vendor Consolidation (→ [[icp-revops-head]])

**Trigger:** Score ≥ 80, segment = revops-head
**Tone:** Business-value, ROI-focused, data-driven
**Key message:** "One enrichment API replacing your three-vendor stack"

```
Subject: cutting your enrichment spend by 40%

Body frame:
- Hook: "You're probably paying for ZoomInfo + Apollo + Clearbit"
- Bridge: transparent credit metering, higher match rates, one API
- Proof: cost comparison table (see [[competitive-battlecards]])
- CTA: "I can model your current spend vs. Hog — want the numbers?"
```

**Sequence:** 4 touches over 21 days (Day 0, Day 5, Day 12, Day 21)

---

## Personalization Variables

| Variable | Source | Required |
|----------|--------|----------|
| `{{FirstName}}` | [[hog-api-enrichment]] contact data | Yes |
| `{{CompanyName}}` | [[hog-api-enrichment]] company data | Yes |
| `{{CurrentTool}}` | Tech stack from enrichment | If available |
| `{{Headcount}}` | Firmographic data | For RevOps track |
| `{{FundingStage}}` | Firmographic data | For Founder track |

---

## Implementation Status

| Component | Status | Notes |
|-----------|--------|-------|
| Template copy | Drafted | Needs A/B testing |
| Sequence timing | Designed | Based on GTM engineering benchmarks |
| Personalization | Designed | Requires [[hog-api-enrichment]] field mapping |
| Send infrastructure | Not started | Evaluate: Resend, SendGrid, or Loops |
| Tracking | Not started | Open/reply tracking, CRM logging |

---

## Gaps Identified

- [ ] No A/B test framework for template variants
- [ ] Send infrastructure not selected
- [ ] Reply handling and routing not designed
- [ ] No negative-reply / opt-out automation
- [ ] Template copy not validated against anti-spam scoring

---

**References:** [[icp-segmentation]] · [[icp-scoring-engine]] · [[competitive-battlecards]] · [[hog-api-enrichment]]

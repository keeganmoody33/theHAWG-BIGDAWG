# Competitive Battlecards

> Operational doc — composes from [[thehog-ai-company]], [[thehog-ai-api]], [[icp-segmentation]]

Competitive positioning for [[thehog-ai-company]] against the enrichment tool landscape. Used by [[outbound-playbook]] templates and sales prep.

---

## Competitor Matrix

| Dimension | TheHog.ai | ZoomInfo | Apollo | Clay | Clearbit |
|-----------|-----------|----------|--------|------|----------|
| **API-first** | Yes — REST + MCP | REST only | REST only | REST + UI | REST only |
| **Batch enrichment** | 100/request | Varies by plan | 25/request | UI-based | 50/request |
| **MCP support** | Native | No | No | No | No |
| **Metering transparency** | Per-request credits | Annual contract | Credit packs | Per-row pricing | Event-based |
| **Async support** | Yes | Limited | No | N/A (UI) | No |
| **Cost visibility** | Real-time (`creditsCharged`) | Annual invoice | Opaque | Per-table | Monthly |
| **Target buyer** | GTM engineers | Enterprise sales | SMB sales | Growth teams | Developers |

---

## Battlecard: TheHog vs. ZoomInfo

**When we win:** Buyer is a [[icp-gtm-engineer]] or [[icp-revops-head]] who wants API-first access, transparent pricing, and no annual lock-in.

**When we lose:** Buyer needs massive data coverage, CRM integration out-of-box, or is mandated by procurement to use an enterprise vendor.

**Talk track:**
> "ZoomInfo gives you the data but locks you into an annual contract with opaque pricing. Hog gives you the same data quality with per-request metering — you pay for what you use, you see the cost in real time, and you can batch 100 records in a single API call."

**Objection handling:**
- "ZoomInfo has more data" → "Match rates are comparable for B2B SaaS. And you see every credit charged per request — no surprise invoices."
- "We're already on ZoomInfo" → "Run a side-by-side: same 50 accounts, compare match rates and cost. Takes 10 minutes with the Hog API."

---

## Battlecard: TheHog vs. Apollo

**When we win:** Buyer is technical and wants batch + async + MCP — not a click-through UI.

**When we lose:** Buyer wants an all-in-one platform (CRM + sequences + enrichment) rather than best-of-breed API.

**Talk track:**
> "Apollo bundles enrichment into a CRM/sequences platform. If you're a [[icp-gtm-engineer]] building your own stack, you want the enrichment unbundled — Hog gives you batch enrichment at 100/request with async processing, not a UI you have to click through."

---

## Battlecard: TheHog vs. Clay

**When we win:** Buyer has outgrown Clay's UI-based enrichment and wants programmatic access.

**When we lose:** Buyer is a non-technical growth marketer who loves Clay's visual workflow builder.

**Talk track:**
> "Clay is great until you need to enrich 10,000 records and your table is crawling. Hog API batches 100 records per request, runs async, and gives you structured JSON — no spreadsheet limits."

---

## Gaps Identified

- [ ] Match rate comparison data needed — actual test results, not estimates
- [ ] Pricing comparison needs real numbers (TheHog credit pricing vs. competitor plans)
- [ ] Clay competitive analysis is thin — needs deeper feature comparison
- [ ] Missing battlecard: TheHog vs. Clearbit (now Breeze by HubSpot)
- [ ] No customer proof points or case studies to strengthen talk tracks
- [ ] Competitor API documentation comparison not done

---

**References:** [[thehog-ai-company]] · [[thehog-ai-api]] · [[icp-gtm-engineer]] · [[icp-revops-head]] · [[outbound-playbook]]

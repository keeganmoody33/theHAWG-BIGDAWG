# For Hudson — GTM Engineering, dog-fooded on The Hog

> A 2-minute read. The thesis: I ran a real GTM loop on The Hog's own market **using The Hog's own
> API**, and compounded every result into a self-referential Context OS an agent can keep building.
> The product is both what I'd sell and what I sold it with.

---

## The proof (one loop, all live API)

I took The Hog from a cold query to a ready-to-send outbound packet — no other data tools.

| Step | Endpoint | Result |
|------|----------|--------|
| **Discover accounts** | `POST /api/v1/companies/search` | `companies using Clay` → 10, `companies using Apollo` → 9, `AI SDR companies` → Salesforge.ai + AiSDR |
| **Find the buyer** | `POST /api/v1/people/search` | **Neeraj Kumar — Director of GTM Engineering at FullFunnel** (a *Top-4 global Clay Elite Studio Partner*) |
| **Research the account** | `POST /api/deep-research` | Stack (Clay, ZoomInfo, Apollo, n8n, HubSpot, 6sense), pain points, live signals — structured |
| **Draft the outbound** | (composed) | Personalized Clay-displacement angle + CTA, grounded entirely in the research above |

**Why this matters:** the product found its own ideal customer and the exact persona who evaluates
tools like it — then armed the rep with a brief that replaces ~45 min of manual research. Every
operation ID is recorded in `knowledge_base/accounts-gtm-engineering-wedge.md`.

The full packet → `knowledge_base/accounts-gtm-engineering-wedge.md` (section "Outbound packet:
FullFunnel → Neeraj Kumar").

---

## What I learned about the product (the part most useful to you)

1. **The Hog already behaves like an agent-first API — lean into it.** When I hit the credit ceiling
   enriching Neeraj, the API returned a typed `402` with *exact* `required: 2736, available: 849` and
   **deducted nothing**. That's textbook budget-control for autonomous agents. I wrote the product
   POV up as `00_foundation/agent-interface-contract.md`: humans set goals/budgets/approvals; agents
   pick endpoints, poll, retry, and return action packets.

2. **Enrichment cost is dominated by identifier *resolution*, not fields.** Dropping
   `[email, phone, signals]` → `[email]` moved the price only `2740 → 2736`. A clean identifier
   (vanity LinkedIn URL / email) vs. a member-URN URL is the real cost lever. A pre-flight cost
   estimate before enrichment would save agents from dead-end `402`s. (`hog-api-metering.md`)

3. **Single-concept queries beat multi-constraint ones.** `companies using Clay` returns fast, named
   accounts; `Series A SaaS using Clay or Apollo hiring GTM/RevOps` ran for minutes and one query is
   *still* processing. Multi-constraint queries also drag in keyword-collision noise (Instagram,
   Reddit, "Global…"). Split-query orchestration is the reliable pattern.

4. **The command center / API split confuses non-technical users.** Simulated onboarding for a solo
   founder, a GTM engineer, and a Head of Revenue (`docs/customer-sessions.md`) surfaced the same
   recurring gaps: async UX feels "frozen," credit consumption is opaque, and deep research is
   under-discovered. These are product-copy and UI-feedback fixes, not missing features.

---

## How I'd run your first 90 days

Full plan in `docs/30-60-90-playbook.md`. In one line each:

- **Days 1–30 (Foundation):** dog-food every endpoint, build the TAM + ICP library entirely on The
  Hog, stand up 5–7 signal monitors.
- **Days 31–60 (Intelligence):** signal-based scoring model, first two campaigns, A/B messaging —
  all created inside the product, feeding a closed loop back into ICP scoring.
- **Days 61–90 (Scale):** ship the **ICP Scoring Engine** on The Hog's API (monitors → weekly
  re-score → deep-research brief for the top 20 → MCP-driven digest) + a repeatable playbook.

---

## What's real vs. what's still open (no overclaiming)

**Real / live:** API auth, company search, people search, deep research, founder-node enrichment via
self-discovery, the FullFunnel→Neeraj packet, an 18+ node knowledge graph (0 orphans, 0 dangling
links), the agent-interface POV, and a live `402` metering observation.

**Open:** Neeraj's *verified email* (gated by credit balance, ~2,740 needed — not a data gap); ICP
scoring weights are still theoretical until calibrated on real accounts; outbound templates are
drafted but unsent; MCP not yet exercised end-to-end; send infrastructure not selected.

---

## How to navigate this repo (30 seconds)

- **Start here:** this file → `00_foundation/_synthesis/gtm-context-os-synthesis.md` (the 5% that
  answers 95%).
- **The proof:** `knowledge_base/accounts-gtm-engineering-wedge.md`.
- **The product POV:** `00_foundation/agent-interface-contract.md`.
- **The plan:** `docs/30-60-90-playbook.md`.
- **Best viewed as an [Obsidian](https://obsidian.md/) vault** (Ctrl+G for the graph) — it's a
  Context OS, so the value is in the links between nodes, not any single file. `CLAUDE.md` is the
  agent/navigation guide.

---

*Built by Keegan (groundskeep). Everything here was produced with The Hog's own API + an agent
operating loop (SENSE → ORIENT → ACT → DEPOSIT).*

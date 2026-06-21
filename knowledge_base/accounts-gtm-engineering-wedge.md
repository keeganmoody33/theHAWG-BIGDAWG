---
type: account_list
icp: gtm_engineering_wedge
source: thehog_api
status: active
---

# GTM Engineering Wedge Account List

## Source queries

- **Query 1:** `B2B SaaS companies hiring GTM engineering` — 1 result, low signal/noise.
- **Query 2:** `companies using Clay` — 10 results.
- **Query 3:** `companies using Apollo` — 9 results.
- **Complex query:** `Series A B2B SaaS companies in the US using Clay or Apollo hiring GTM engineering or RevOps` — still `processing` as operation `27cea29e-e48b-49f0-9eeb-ca850c79d604`.

## Accounts by signal

### Primary competitors / alternatives

- **Clay** (`clay.com`)
  - Industry: Software Development
  - Location: New York, NY
  - Signal: exact competitor to TheHog for data enrichment waterfalls; ideal account to win away from Clay workflows.

- **Apollo.io** (`apollo.io`)
  - Industry: Software Development
  - Location: San Francisco, California
  - Signal: end-to-end GTM platform; adjacent to TheHog, often used alongside or instead of data enrichment layers.

- **Sumble** (`sumble.com`)
  - Industry: Technology, Information and Internet
  - Location: San Francisco, CA
  - Signal: AI-powered account intelligence; direct competitor in the account intelligence space.

### Services / agency GTM/RevOps customers

- **FullFunnel** (`fullfunnel.co`)
  - Industry: Business Consulting and Services
  - Location: Boston, MA
  - Signal: GTM and RevOps services firm; heavy Clay/Apollo workflows, ideal customer for TheHog API-first data layer.

- **FullFunnel** (`gofullfunnel.com`)
  - Industry: Business Consulting and Services
  - Signal: same brand, different domain.

- **FullFunnel.io** (`fullfunnel.io`)
  - Industry: Advertising Services
  - Location: Tallinn, Harjumaa
  - Signal: ABM and full-funnel marketing consulting for B2B tech; high ACV/long sales cycle.

### Product-led / AI GTM motion

- **Pocus (acquired by Apollo.io)** (`pocus.linkedin.com`)
  - Industry: Software Development
  - Location: San Francisco
  - Signal: product-led sales platform; users are GTM engineers who built PLG scoring pipelines.

- **G2** (`g2.com`)
  - Industry: Technology, Information and Internet
  - Location: Chicago, IL
  - Signal: B2B software intelligence; large data buyer, likely has internal GTM data teams.

- **Bloomberry** (`bloomberry.ai`)
  - Industry: Technology, Information and Internet
  - Location: San Jose, CA
  - Signal: AI writing/LinkedIn automation; early GTM tooling buyer.

### Noise / low relevance

- Clay Lacy Aviation, Bloomberry Resorts Corporation, Bloomberry Agency, Grow Pro Ltd, The Reddit Marketing Agency, Reddit for Business, Reddit Inc., g2 Recruitment, G2 Risk Solutions — returned because of keyword collision on “Clay”/“Apollo”/“G2” or unrelated businesses.

## Top 3 priority accounts

1. **FullFunnel** (`fullfunnel.co`) — GTM/RevOps services buyer, likely already paying Clay/Apollo, high intent to consolidate data workflows.
2. **Clay** (`clay.com`) — competitor displacement; TheHog can replace Clay tables/enrichment workflows with a single API.
3. **Apollo.io** (`apollo.io`) — adjacent platform; TheHog can be the data intelligence layer beneath or alongside Apollo.

## Observations

- Natural-language queries with multiple constraints (funding stage + stack + hiring) can run for many minutes; split-query strategy is faster and more reliable.
- Single-concept queries (`companies using Clay`, `companies using Apollo`) return within seconds and produce actionable, named accounts.
- No metering or cost data was returned in any operation response; credit tracking may be at the org level or not exposed in the public API response body.
- Match scores are not present in the response; ranking is implicit in result order.

## People search results

### FullFunnel (`fullfunnel.co`)

- **Operation ID:** `8ebe6718-c5f3-4538-9b40-ddc48a5cd56f`
- **Final status:** `succeeded`
- **People returned:** `1`
- **Top contact:**
  - **Name:** Neeraj Kumar
  - **Title:** Director of GTM Engineering
  - **Company:** FullFunnel
  - **LinkedIn:** `https://www.linkedin.com/in/ACwAAAzQiFAB25PvJc7YGnSAtEvf-gkJJsqGQtE`
  - **Signal:** exact buyer persona; likely owns Clay/Apollo data workflows and can evaluate TheHog as an API-first replacement.

### Apollo.io (`apollo.io`)

- **Operation ID:** `bf05f9ca-bf84-4342-9b99-4f027e1bf39c`
- **Final status:** `succeeded`
- **People returned:** `2`
- **Contacts:**
  - Piotr Dyba — Engineering Manager II, Apollo.io
  - Sachin Jha — Apollo Solution and Implementation Partner, Apollo.io
- **Signal:** engineering manager is a potential data-layer buyer; solution partner is more services-side.

### Clay (`clay.com`)

- **Operation ID:** `615dd51c-5601-4e9a-85ad-becdc39ee1b3`
- **Final status:** `succeeded`
- **People returned:** `0`
- **Interpretation:** people search did not surface public profiles for the queried titles at Clay; the query may have been too narrow or the data layer may not have coverage for this domain/title combination.

## Deep-research result: FullFunnel

- **Operation ID:** `2a53a8a5-145c-4bae-811e-6220131139f7`
- **Final status:** `succeeded`
- **Company summary:** FullFunnel is a B2B GTM and RevOps services firm that provides "Revenue Operations as a Service." They help organizations streamline revenue operations, build GTM systems, run paid digital marketing, and act as a **Top 4 global Clay Elite Studio Partner**.
- **GTM engineering motion:** FullFunnel advocates for a centrally orchestrated operating model where RevOps and GTM engineers codify the organization's playbook into workflow tables (using tools like Clay and n8n) that automatically identify, score, enrich, and route accounts. They build multi-source waterfall enrichment workflows rather than relying on a single database.
- **Tools mentioned:** Clay, ZoomInfo, Apollo, n8n, HubSpot, ActiveCampaign, 6sense, Claude Code.
- **Likely pain points:**
  - Managing complexity and maintenance overhead of long enrichment waterfalls.
  - Dealing with API rate limits and breakages across 150+ providers.
  - Latency issues when executing live web pulls and multi-step enrichments.
  - High cumulative costs of using multiple data vendors simultaneously.
- **Recent signals:**
  - Neeraj Kumar and FullFunnel recently posted about fully automating their content production engine end-to-end using AI and real-time market insights.
  - FullFunnel recently published content comparing Clay vs. ZoomInfo and discussing scalable GTM engines with Clay and n8n.

## Outbound packet: FullFunnel → Neeraj Kumar

- **Account:** FullFunnel (`fullfunnel.co`)
- **Buyer:** Neeraj Kumar, Director of GTM Engineering
- **LinkedIn:** `https://www.linkedin.com/in/ACwAAAzQiFAB25PvJc7YGnSAtEvf-gkJJsqGQtE`
- **Trigger:** FullFunnel is a top Clay Elite Studio Partner; Neeraj posted about automating content production end-to-end with AI and real-time insights.
- **Pain hypothesis:** As a Clay Elite Studio, FullFunnel is deeply invested in multi-provider enrichment waterfalls. The team likely spends significant engineering time managing API rate limits, breakages, latency, and vendor sprawl across 150+ data sources.
- **Why now:** FullFunnel is actively publishing content and building GTM engines for clients; their internal GTM engineering stack is both their product and their bottleneck.
- **Personalized angle:** "Hi Neeraj — as the Director of GTM Engineering at a top Clay Elite Studio, you've built complex Clay tables and n8n engines. You likely spend a lot of time managing the maintenance, latency, and rate limits of multi-provider enrichment waterfalls. TheHog.ai offers an API-first alternative that simplifies this with a single, real-time web intelligence API, reducing the maintenance burden on your GTM engineering team while delivering fresh context across people and companies."
- **Suggested CTA:** "Would you be open to testing TheHog.ai's single API to see if it can simplify your n8n workflows and reduce the need for complex Clay waterfall setups?"
- **Source:** TheHog.ai deep-research API (`POST /api/deep-research`) + TheHog.ai people search (`POST /api/v1/people/search`)
- **Confidence:** high-moderate (live API data, but no direct contact enrichment yet)

## Next actions

- Run `enrichment` on Neeraj Kumar to test contact data quality and credit visibility.
- Store the outbound packet in a campaign folder if multi-account sequences are built.
- Run another `companies/search` for a second wedge (e.g., `AI SDR companies` or `product-led sales platforms`) to expand the account list.

## Links

- [[thehog-api-validation]]
- [[thehog-ai-company]]
- [[outbound-playbook]]
- [[thehog-ai-icp-segments]]

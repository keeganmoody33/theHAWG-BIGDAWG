# TheHog.ai: API Reference & Feature Map

> *Context7-style doc reference pass — pulled directly from the live docs at `docs.thehog.ai`. All feature descriptions, endpoint signatures, and response shapes are sourced from the actual documentation.*

## Related Workspace Docs

- [Customer Onboarding Sessions](./customer-sessions.md) — three realistic first-time user journeys.
- [30-60-90 Day Playbook](./30-60-90-playbook.md) — founding GTM engineer execution plan.
- [MCP Setup Guide](./mcp-setup.md) — connecting Claude / Cursor to the Hog.ai MCP (added in Phase 2).

***

## Doc Architecture Overview

TheHog.ai exposes its full documentation index at `https://docs.thehog.ai/llms.txt` — a deliberate signal that the docs are built to be consumed by AI agents, not just human readers. Every page carries that index as a header, which means a coding agent or Claude instance can discover all available pages before exploring further.[^1]

The base URL for all API requests is:

```text
https://developer.thehog.ai
```

Every endpoint is prefixed with `/api`. Authentication uses dual-header credentials — never `Authorization` — with `X-Access-Key` and `X-Secret-Key` sourced from the Credentials page in the dashboard.[^2][^1]

***

## Canonical Feature Map (What's Actually Built)

As of the live docs, TheHog.ai has **seven production endpoints**, all marked LIVE. One more is listed as "coming shortly". Here is the complete feature reference pulled from the documentation:[^3]

### 1. Company Search — `POST /api/v1/companies/search`

The entry point for the company-first workflow. Accepts a free-text `query` (up to 256 chars) plus optional structured filters:[^1]

| Filter Layer | Parameters | What It Does |
| --- | --- | --- |
| Text & Identity | `query`, `filters.company.domains` | Natural language or exact domain match |
| Firmographics | `filters.industries`, `filters.locations`, `filters.employeeCount.min/max` | Headcount, geo, industry |
| Technographics | Embedded in `query` text | Stack mentions — e.g. "using Salesforce and HubSpot" |
| Buying Signals | `filters.signals` — `"hiring"`, `"funding"` | Active growth signal detection |

**Response shape:** Always `202 Accepted` — returns `operationId` + `pollUrl`. Poll `GET /api/operations/:id` until `status: "succeeded"`. CompanyCard fields include: `tech_stack`, `is_hiring`, `has_recent_funding`, `growth_band` (`"low"/"medium"/"high"`), `match_score` (0–1), and `signal_summary`.[^2]

**Pagination:** Default 25 results, max 100 per call.[^2]

### 2. People Search — `POST /api/v1/people/search`

Natural language contact discovery, optionally scoped to a company from step 1. Returns ICP-ranked contacts.[^2]

Key request fields:

- `query` — plain English, e.g. `"VP of Sales at a Series B SaaS company"`
- `filters.titles` — exact or partial title filters
- `filters.company.domains` — scope to specific accounts (dramatically improves relevance per the docs)[^2]
- `includeSignals` — boolean, adds signal context to results
- `includeContacts` — boolean, includes contact fields when available
- `limit` — 1 to 100[^2]

**Response:** `202` + operation polling. Semantically ranked results.[^2]

### 3. Contact Enrichment — `POST /api/enrichments`

Accepts one identifier for single-contact enrichment or up to 100 identifiers for batch enrichment. Supported identifier fields include `linkedin_url`, `email`, `x_handle`, and `github_username`. Required `fields` values include `contact.email`, `contact.phone`, and optionally `signals`.[^2]

**Two-mode response:** The enrichment endpoint can return either synchronously (`200 OK` — data present immediately) or asynchronously (`202 Accepted` — poll for results). The distinction matters for pipeline design.[^2]

Response includes contact data such as `emailStatus`, `phoneStatus`, `emails[]` with `isVerified`, `phoneNumbers[]` with `phoneType`, and `fromCache`. Credit cost is visible in both `metering.creditsCharged` / `metering.estimatedMaxCredits` and `meta.cost.estimated` / `meta.cost.actual` when present. `402 Payment Required` means insufficient credits and no credits are deducted.[^2]

### 4. Deep Research — `POST /api/deep-research`

LLM-powered research jobs that browse the web, synthesize findings, and return **structured data conforming to any JSON Schema you define**. This is the highest-leverage endpoint for GTM work.[^2]

| Request Field | Type | Notes |
| --- | --- | --- |
| `prompt` | string | Natural language research question |
| `schema` | object | JSON Schema defining the exact output shape |
| `model` | string (optional) | Override model when needed |
| `urls` | string[] (optional) | Seed URLs as starting points |
| `inputAnchors` | object or object[] (optional) | Caller-owned anchor metadata overlaid into matching schema paths |
| `maxCredits` | integer (optional) | Maximum credits to spend; must match `budget.maxCredits` when both are provided |
| `budget` | object (optional) | Budget request for the research job |
| `enrich` | boolean (optional) | Set `false` to disable server-side schema enrichment |
| `Idempotency-Key` | header | Prevents duplicate jobs; use stable keys such as `research-{company}-{month}` |

**Always async.** The docs show `POST /api/deep-research` and also reference `POST /api/v1/deep-research`; prefer the stable documented guide endpoint unless the MCP tool abstracts this. Poll `progress` to track completion. Credits are proportional to prompt complexity and source usage; use `maxCredits` or `budget.maxCredits` when spend must be bounded.[^2]

**Example output schema** can capture: `companyName`, `mainProducts[]`, `targetCustomers`, `recentFunding.amount/round/date`, `keyExecutives[].name/title`, `recentPress[].headline/source/date`.[^2]

### 5. Social Listening Monitors — `POST /api/v1/monitors`

Recurring monitors that track keywords, profiles, and posts across multiple platforms. Monitor types currently documented:[^2]

| Monitor Type | Platform | Use Case |
| --- | --- | --- |
| `x_keyword`, `x_profile` | X | Category conversations, competitor complaints, executive posts |
| `reddit_keyword`, `reddit_subreddit` | Reddit | Community conversations, ICP buying signals |
| `linkedin_keyword`, `linkedin_profile`, `linkedin_company`, `linkedin_post` | LinkedIn | Professional signal tracking |
| `instagram_profile`, `instagram_post` | Instagram | Profile and post monitoring |
| `tiktok_keyword`, `tiktok_hashtag` | TikTok | Brand/category mentions |
| `web_search`, `site_search` | Open web | Broad signal capture and site-specific tracking |

Config fields: `name`, `type`, `config`, `cadence_minutes`, and `max_results`. The docs describe minimum cadence constraints: scraper-based monitor types start at 60 minutes, while web types can start at 15 minutes. Profile/company monitors can also use cache controls such as `force_fresh` and `cache_ttl_days` where supported.[^2]

**Polling events:** `GET /api/v1/monitors/:id/events?since={ISO timestamp}&limit=50` and cursor pagination via `next_cursor` when present. Event fields include `id`, `monitor_id`, `dedup_key`, `event_type`, `event_json`, `canonical_person_id`, and `canonical_company_id`.[^2]

**Production pattern per docs:** One monitor per topic. Poll with the last successful `since` timestamp. Dedupe by `dedup_key` or post URL. Score for actionability (buying intent, competitor pain). Keep human in the loop for engagement decisions.[^2]

### 6. Multi-Platform Search — `POST /api/v1/search`

One-off search across web and social in a single call. Accepts `type`, `query`, optional `max_results`, and optional platform-specific parameters such as Reddit `sort_by`. Documented search types include `web_search`, `site_search`, `linkedin_keyword`, `x_keyword`, `reddit_search`, `tiktok_keyword`, and `tiktok_hashtag`. Returns `202` + `poll_url`, or can return `200 OK` when `sync=true` completes quickly. Use this before creating a recurring monitor to test query wording.[^1][^2]

### 7. MCP Integration — Hosted Remote or Local stdio

TheHog.ai supports two MCP connection methods:[^1]

- **Hosted remote MCP** — connect to `https://mcp.thehog.ai/mcp`; clients that support remote MCP use OAuth.
- **Local stdio MCP package** — run `npx -y @thehog/mcp@latest` with `THEHOG_ACCESS_KEY` and `THEHOG_SECRET_KEY` in the process environment.

The local package is published as `@thehog/mcp` and its source is `The-Hog/the-hog-mcp`. Hosted connections can be revoked from `https://platform.thehog.ai/settings/integrations/mcp`.[^2]

### Async Operation Polling — `GET /api/operations/:id`

All non-trivial endpoints are async. The polling interface is consistent across every operation:[^2]

```json
{
  "id": "op_01hxyz",
  "status": "processing",   // queued → processing → succeeded / failed / partial_success / cancelled
  "progress": 45,           // 0–100 for deep research
  "result": null,
  "error": null
}
```

Recommended poll interval: every 2–5 seconds for enrichment; every 10–30 seconds for deep research, with exponential backoff in production.[^2]

***

## What's Documented vs. What's Hinted

| Capability | Status in Docs |
| --- | --- |
| Company Search | Full guide + examples |
| People Search | Full guide + examples |
| Contact Enrichment | Full guide + response shapes |
| Deep Research | Full guide + schema examples |
| X Social Listening | Full guide with JS routing example |
| Reddit / LinkedIn / TikTok monitors | Referenced in monitor types, less example coverage |
| Multi-platform search | Endpoint reference, minimal guide |
| MCP integration | Guide exists at `/guides/use-mcp` |
| Deanonymized Visitor Intelligence | Listed on homepage as a use case, no public endpoint guide yet |
| SEO / PPC Intelligence | Listed on homepage as a use case, no public endpoint guide yet |
| Trading / Signal Agents | Listed on homepage as a use case, no public endpoint guide yet |
| Command Center UI | Described on YC launch page, separate from API docs |

The three homepage use cases without public endpoint guides (visitor deanonymization, SEO/PPC, trading signals) represent **the "coming shortly" endpoint** referenced in the product header.[^3]

[^1]: <https://docs.thehog.ai/llms.txt>
[^2]: <https://docs.thehog.ai>
[^3]: <https://docs.thehog.ai/api-reference>

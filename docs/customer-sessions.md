# Three Customer Onboarding Sessions

What follows is a realistic simulation of three distinct customers — different environments, different technical comfort levels, different GTM contexts — each going through their **first three sessions** with TheHog.ai. Every action, confusion point, and discovery moment is grounded in the actual documented behavior of the product.

***

## Customer 1: Marcus — Solo Founder, Pre-Revenue, Non-Technical

**Profile:** Marcus runs a two-person fintech compliance SaaS startup. He is the founder and sole GTM person. His stack today: a spreadsheet, LinkedIn Sales Nav (which he barely uses), and Apollo (which he signed up for but never set up properly). He joined TheHog.ai after seeing it on the YC F25 launch page. He is not a developer. He expected a UI-first product.

***

### Session 1 — "Where's the app?"

Marcus clicks the onboarding email and lands on the dashboard. He navigates to the Credentials page, finds his `X-Access-Key` and `X-Secret-Key`, and immediately does not know what to do with them. He is looking for a search bar. The command center UI described on the YC launch page ("AI co-pilots," "stack-ranked game plans") exists, but Marcus has not found it yet — he's been clicking through what looks like a developer portal.[^4][^2]

**What he tries:**

- Copies the `curl` example from the quickstart into his browser address bar (does not work)[^2]
- Pastes the Python example into a Google Doc (confused)
- Discovers there is a dashboard with a search interface after clicking the logo and landing on the main app

**What clicks:**

- The command center UI loads. He types "B2B fintech compliance companies in the US" into what looks like a search bar and gets back a list of companies
- Realizes the API docs are the *developer layer* — not the product he was supposed to use
- Sets up his first company search result list: 47 companies matching "fintech compliance software, Series A, US, hiring"

**What he does not understand:**

- What `operationId` means or why the search didn't return instantly — he refreshed the page twice thinking it was broken before the results loaded[^2]
- The difference between the "command center" (UI) and the "API" (developer access) — the product feels like two separate things
- What "deep research" is or when to use it vs. a regular company search

**Session 1 output:** One company list, zero contacts, no enrichment. Confusion about async behavior. Scheduled a follow-up with himself to "figure out the email part."

***

### Session 2 — "Okay, now what do I do with these companies?"

Marcus comes back with a clearer goal: get email addresses for 10 decision-makers at the companies from his list.

**What he tries:**

- Clicks on a company from his list, looks for a "find contacts" button — finds it, gets prompted to write a query like "Head of Compliance" in natural language[^2]
- Runs the people search scoped to a company domain — results come back in about 45 seconds
- Clicks "Enrich" on three contacts. Two return immediately (`200 OK`), one shows a spinner for two minutes (`202` async polling behind the scenes) — Marcus thinks it's frozen[^2]

**What clicks:**

- The natural language people query. He types "VP of Compliance or Chief Risk Officer" and gets back ranked contacts with titles and LinkedIn profiles — this genuinely impresses him
- That `includeSignals: true` (surfaced as a toggle in the UI) adds context like "recently promoted" or "joined in last 6 months" to the contact cards
- The verified email badge — he's used to Apollo showing "likely valid" guesses; verified emails feel like a real upgrade

**What he does not understand:**

- Why some enrichments are instant and some are not — he does not see the `200 vs 202` logic explained anywhere in the UI (it's in the docs but not surfaced as product copy)[^2]
- The `fromCache: false` field on enrichment results — he wonders if old data is being served
- Credit consumption — `meta.cost.actual: 4` appears in one API response he accidentally opens in the browser; he doesn't know if he's burning through a budget

**Session 2 output:** 10 enriched contacts with verified emails. First real "aha" moment. Starting to trust the data layer. Still has not touched deep research or monitors.

***

### Session 3 — "Can this tell me what they're actually talking about?"

Marcus has heard that The Hog monitors social media. He wants to know what compliance leaders are saying on LinkedIn and Reddit because he wants to show up in those conversations.

**What he tries:**

- Navigates to "Monitors" in the command center — finds it but isn't sure what "cadence minutes" means[^2]
- Creates his first monitor: `reddit_keyword` for `"compliance software" OR "KYC automation"`, cadence 60 minutes
- Creates a second monitor: `x_keyword` for `"AML compliance" OR "fintech regulation"`

**What clicks:**

- The monitor events feed. Within an hour, he sees three Reddit posts from compliance professionals asking for tool recommendations — exactly his ICP
- The `event_json.text` preview is readable in the UI without any code; he can click through to the actual post
- Realizes this is the signal layer that was missing from his Apollo workflow — Apollo found contacts, but The Hog finds *conversations*

**What he does not understand:**

- The `since` timestamp parameter for polling events — he doesn't know if he should be setting this manually or if the UI handles deduplication for him[^2]
- Why LinkedIn monitor coverage feels thinner than the X coverage — the docs note that `event_json` field availability varies by source, but this isn't clearly explained upfront[^2]
- Deep research still untouched. He sees the option but doesn't understand that it can return a structured JSON brief on any company in 1–5 minutes[^2]

**Marcus after three sessions:** Has a working contact list, enriched emails, and live monitors. Has not touched the API directly. Confused by async UX, unclear on credit consumption, and has not discovered deep research. The gap is product copy and UI feedback — not missing features.

***

## Customer 2: Priya — GTM Engineer at a Series A DevTools Company

**Profile:** Priya is the first GTM engineer at a 30-person devtools company (they build infrastructure monitoring tooling). Her stack: Clay + Apollo + Smartlead + HubSpot. She's running automated enrichment pipelines. She signed up for TheHog.ai's API access specifically because her team is building an AI agent that needs a real-time data layer. She is comfortable in Python, knows REST APIs, and has read the entire docs index before her first session.

***

### Session 1 — "Let me benchmark this against my Clay waterfall"

Priya's first move is to run a controlled comparison: same company search she runs weekly in Clay, now through TheHog.ai API.

**What she tries:**

```python
# Session 1 test: replicate her Clay "Series A devtools, US, hiring" list
response = httpx.post(
    "https://developer.thehog.ai/api/v1/companies/search",
    headers={"X-Access-Key": os.environ["THEHOG_ACCESS_KEY"], "X-Secret-Key": os.environ["THEHOG_SECRET_KEY"]},
    json={
        "query": "Developer tools and infrastructure monitoring companies",
        "filters": {
            "industries": ["Software"],
            "locations": ["United States"],
            "employeeCount": {"min": 20, "max": 200},
            "signals": ["hiring", "funding"]
        },
        "limit": 50
    }
)
operation_id = response.json()["operationId"]
```

She builds her polling loop immediately — she's read the docs. Results come back in about 30 seconds with `match_score` values and `tech_stack` arrays.[^2]

**What clicks:**

- `tech_stack` field on company results — directly identifies companies using competitive or complementary tools without a separate enrichment call. Clay required 3 provider calls to get equivalent technographic data
- `growth_band: "high"` as a pre-computed signal — she doesn't have to define growth logic herself
- `signal_summary` arrays with human-readable descriptions like `"Series B funding in Q1 2026"` — structured but also readable without post-processing

**What she does not understand / wants to probe:**

- Whether `tech_stack` is crawled live or from a database — the docs don't specify freshness[^2]
- What signal categories are available beyond `"hiring"` and `"funding"` — the docs show only these two as examples[^2]
- `match_score` ranking logic — she wants to know if it's semantic similarity to the query or something else

**Session 1 output:** Python script that searches + polls + dumps company list to CSV. Benchmark: TheHog returns richer per-company context than her Clay Apollo waterfall in fewer API calls. She writes this up as a Notion note.

***

### Session 2 — "Building the people + enrichment pipeline"

Priya wants to wire up the full company → people → enrich flow into a pipeline that feeds HubSpot.

**What she tries:**

- Company search returns 50 accounts, she loops through the top 20 by `match_score`
- For each company domain, fires `POST /api/v1/people/search` with `filters.company.domains` and queries like `"engineering leader"`, `"DevOps lead"`, `"platform engineer"`[^2]
- Passes returned person IDs into `POST /api/enrichments` — handles both `200` and `202` response codes with a unified polling loop[^2]

**The async `202` pattern for enrichment:**

```python
result = httpx.post("https://developer.thehog.ai/api/enrichments", ...)
if result.status_code == 200:
    data = result.json()["data"]
elif result.status_code == 202:
    operation_id = result.json()["operationId"]
    # poll until succeeded
```

She figures this out cleanly because she read the quickstart. But she notes that Clay's waterfall is synchronous by abstraction even if async underneath — TheHog.ai requires you to manage async state explicitly.[^2]

**What clicks:**

- `includeSignals: true` on people search adds context like recent job changes, promotions, or company growth signals to individual contacts — useful for personalization layer in Smartlead
- `meta.cost.actual` on enrichment responses — she can track credit burn per enrichment and model cost per contact at scale[^2]

**What she does not understand / friction points:**

- Native batch enrichment exists via `identifiers` with up to 100 contacts, but she still needs batching, retry, and polling strategy for 1,000+ contacts.
- Wants clear guidance on choosing single `identifier` versus batch `identifiers`, especially when mixing LinkedIn URLs, emails, X handles, and GitHub usernames.[^2]
- The `fromCache` boolean on enrichment results — she wants cache TTL and freshness guarantees surfaced near results rather than buried in docs.[^2]

**Session 2 output:** End-to-end Python pipeline: company search → people search → enrichment → CSV output → HubSpot import. 3 hours of work. She considers replacing her Clay workflow with this for the real-time signal layer, keeping Clay for specific multi-provider waterfall scenarios.

***

### Session 3 — "Deep research as account intelligence for my agent"

Priya's real goal: feed structured account intelligence into the AI monitoring agent her team is building. Deep research is the key endpoint.[^2]

**What she tries:**

- Fires `POST /api/deep-research` with a structured JSON Schema that her agent can consume directly:

```python
schema = {
    "type": "object",
    "properties": {
        "companyName": {"type": "string"},
        "currentMonitoringStack": {"type": "array", "items": {"type": "string"}},
        "painPoints": {"type": "array", "items": {"type": "string"}},
        "openRoles": {"type": "array", "items": {
            "type": "object",
            "properties": {
                "title": {"type": "string"},
                "signalType": {"type": "string"}
            }
        }},
        "recentTechAnnouncements": {"type": "array", "items": {"type": "string"}},
        "buyingSignalScore": {"type": "number"}
    },
    "required": ["companyName", "currentMonitoringStack", "painPoints"]
}
```

- Uses `Idempotency-Key` header with `f"research-{domain}-{month}"` format per the docs[^2]
- Seeds the prompt with the company URL and recent news from her company search results
- Deep research jobs complete in 2–4 minutes; she builds a `tqdm` progress bar off the `progress` field[^2]

**What clicks:**

- The schema-conformant output is directly injectable into her agent's context window — no post-processing required. This is the core value prop for her use case[^2]
- `model` override field — she can specify `"openai:gpt-4.1"` for research jobs where she needs maximum accuracy[^2]
- The `urls` seed field — she passes in the company's engineering blog and recent GitHub release notes as starting points, dramatically improving result quality[^2]

**What she does not understand / wants:**

- No streaming support for deep research results. She has to poll and wait; there's no webhook or callback option documented[^2]
- Credit cost for deep research is not pre-estimated before job submission — she wants a dry-run or cost estimation endpoint
- MCP integration: she sets up the Claude + TheHog.ai MCP connection via the hosted remote URL, but wants to understand tool scoping — which tools are exposed by default, and can she restrict to specific endpoints per agent

**Priya after three sessions:** Full pipeline running. Deep research feeding her AI agent's account context layer. Has replaced the Apollo + Clearbit enrichment step in her Clay workflow with TheHog.ai's people + enrichment endpoints. Primary open questions: rate limits at scale, 1,000+ contact batch orchestration, webhook support for async jobs, and MCP tool scoping.

***

## Customer 3: Darnell — Head of Revenue at a 50-Person B2B SaaS Company (Non-Technical, Power User)

**Profile:** Darnell leads a 4-person revenue team (2 AEs, 1 SDR, himself). He manages HubSpot, Smartlead, and a Clay workflow he barely understands. He's been handed a TheHog.ai license by his new GTM engineer. He's not coding anything — he's using the command center UI exclusively. His job is to find accounts, build sequences, and measure pipeline. He's used to Salesforce and Apollo.

***

### Session 1 — "Show me the money"

Darnell jumps in with a specific goal: build a list of 50 mid-market SaaS companies that are actively hiring salespeople and have raised in the last 6 months — his clearest buying signal for his ICP.

**What he tries:**

- Command center company search: "B2B SaaS companies in the US, 100–500 employees, hiring sales team, recently funded"
- Gets back 38 results with `growth_band`, `signal_summary`, and `tech_stack` visible in the card view
- Clicks "Find People" on three accounts — surfaces VP Sales, CRO, and Head of RevOps at each

**What clicks:**

- `signal_summary` cards — "Raised $18M Series A in March 2026, currently hiring 4 sales roles" is exactly the context he gives to his SDR before a cold call. This used to take 20 minutes of manual research per account
- `tech_stack` showing "HubSpot + Salesloft + Apollo" on a target account — immediately tells him there's an existing GTM stack and a budget precedent for tooling

**What he does not understand:**

- `match_score: 0.73` — he has no reference for what a "good" score is or how to filter by it
- Why some contact cards show "Enrich" greyed out — unclear if it's a permissions issue, a plan limitation, or the contact simply has no data
- The async wait on company search. He typed his query, hit enter, and a loading indicator ran for 25 seconds. He re-typed the same query a second time, creating a duplicate operation[^2]

**Session 1 output:** 38-account list. 9 enriched contacts with emails. Starting to see the signal layer. His SDR does not use the tool yet — Darnell still copy-pasting results into a Google Sheet manually.

***

### Session 2 — "I want to know what these people are saying"

Darnell's SDR mentioned they've been spending an hour a day manually checking LinkedIn and Reddit for "GTM engineering" conversations. Darnell sets up monitors to automate this.

**What he tries:**

- Creates three monitors in the command center:
  1. `x_keyword` — `"GTM engineering" OR "sales automation" OR "outbound automation"`
  2. `reddit_keyword` — `"B2B outbound" OR "SDR automation" OR "Clay alternative"`
  3. `x_profile` — monitors the CEO handle of a key competitor
- Events start surfacing within the first cadence window (60 minutes)

**What clicks:**

- The competitor CEO monitor — first run surfaces a post where the competitor's CEO is announcing a price increase. Darnell screenshots this, writes a displacement email, and sends it to his SDR within 20 minutes. This is a direct pipeline action from a signal
- Reddit posts with buying intent: "anyone use TheHog vs Clay for enrichment?" appears in the monitor feed — Darnell recognizes his own ICP asking this question publicly and flags it for engagement

**What he does not understand:**

- The `since` timestamp polling mechanic is invisible to him at the UI level, but the underlying behavior (potentially showing the same post twice if polling windows overlap) manifests as what looks like duplicate entries in his monitor feed[^2]
- LinkedIn monitor results are sparser than X results — he expected LinkedIn to be the richest source (it's where his buyers are) but the coverage is thinner[^2]
- No built-in "reply" or "engage" button on monitor events — the workflow stops at surfacing the post; engagement requires leaving the platform. He expected this to be more integrated given the YC launch copy about "auto-draft comments and posts"[^4]

**Session 2 output:** Three live monitors. First signal-to-action loop: competitor price increase post → displacement email drafted in 20 minutes. SDR now checking the monitor feed daily instead of manually scrolling LinkedIn.

***

### Session 3 — "Can I use this for account research without my GTM engineer?"

Darnell wants to generate a one-page account brief for a key target account before his AE's discovery call — without waiting for his GTM engineer to run a script.

**What he tries:**

- Navigates to "Deep Research" in the command center
- Types: "Research [target company]. I need their current tech stack, what GTM tools they use, any recent funding or hiring news, and 3 potential pain points for a B2B SaaS revenue team."
- Hits submit — a loading state runs for 3.5 minutes[^2]
- Output: structured brief with company name, tools identified (`Salesforce, Gong, ZoomInfo`), recent news (`$22M Series B, February 2026`), and three pain points listed as bullet strings

**What clicks:**

- The 3.5-minute wait is worth it. The output replaces ~45 minutes of manual research. His AE reads the brief on the Slack message Darnell pastes it into and says "this is the best pre-call brief I've ever gotten"
- Realizing he can run 5–10 of these per week with zero technical help — the UI abstracts the JSON Schema entirely; he just writes a prompt and reads the output[^2]

**What he does not understand:**

- Why some deep research outputs are much better than others — he doesn't yet understand that specificity of the prompt drives output quality. A vague prompt returns a vague brief; a targeted prompt with seed URLs returns a precise brief[^2]
- No ability to save or templatize prompts — he writes a good prompt on Session 3 but has no way to save it for next time inside the platform
- Credit consumption is still opaque. After three sessions he has no clear idea how many credits he's used or what the cost per deep research job was[^2]

**Darnell after three sessions:** Monitors running, deep research delivering pre-call briefs, contact enrichment feeding into HubSpot. High satisfaction, medium proficiency. The platform is delivering value but the experience has three persistent friction points: async UX feedback, credit transparency, and no save/template layer for prompts and monitor configurations.

***

## Cross-Customer Friction Pattern Summary

| Friction Point | Marcus (Founder) | Priya (GTM Eng) | Darnell (Head of Rev) | Doc Coverage |
| --- | --- | --- | --- | --- |
| Async `202` UX not explained in product | ❌ Confusion | ✅ Handled via code | ❌ Duplicate queries | Documented in quickstart, not surfaced in UI[^2] |
| Credit consumption visibility | ❌ Unknown | ✅ Tracked via `meta.cost` | ❌ Opaque | API fields exist, no UI display |
| Batch enrichment operations | N/A | ⚠️ Needs scaling guidance | N/A | Batch up to 100 identifiers is documented; 1,000+ contact orchestration remains a GTM engineering concern[^2] |
| `match_score` interpretation | ❌ Unclear | ✅ Used as filter | ❌ No reference point | Field present, no explanation of scale |
| LinkedIn monitor coverage gap | ❌ Noticed | ✅ Noted | ❌ Frustrated | Docs note `event_json` varies by source[^2] |
| Deep research prompt quality dependency | N/A (not used) | ✅ Understood | ❌ Inconsistent results | Docs say "narrow prompts produce more accurate results"[^2] |
| Webhook / callback for async jobs | N/A | ❌ Wants it | N/A | Not documented |
| Platform vs. API distinction | ❌ Lost at start | ✅ Clear | ✅ Clear | Not explained in onboarding flow |
| Save/template layer for prompts + monitors | ❌ | ✅ (scripted externally) | ❌ Wants it | Not a current feature |

***

## Changelog Relevance: What to Watch For

The three use cases without public endpoint guides (visitor deanonymization, SEO/PPC intelligence, trading/signal agents) represent the next high-value surface area. When those endpoints ship:[^3]

- **Darnell's use case** unlocks dramatically — deanonymized visitor intelligence would let him see which target companies are already on the pricing page, converting a monitor into a warm outbound trigger
- **Priya's pipeline** gets a new scoring layer — visitor identity + firmographic match + deep research = pre-qualified pipeline without any SDR involvement
- **Marcus's use case** goes product-led — instead of manually monitoring Reddit for his ICP, he monitors who visits his own site and triggers enrichment + outreach automatically

The "one more endpoint coming shortly" flag on the homepage is the most important line in the product for a founding GTM engineer to track.[^3]

***

## References

[^2]: [TheHog.ai docs via Context7 `/websites/thehog_ai`](https://docs.thehog.ai) - API behavior, async polling, enrichment, monitors, MCP, and credit metering reference.
[^3]: [TheHog.ai](https://thehog.ai) - Automated GTM agents. Full-cycle go-to-market automation. Target identification, enrichment, and outbound execution.
[^4]: [Launch YC: The Hog: AI Native Go-To-Market Command Center](https://www.ycombinator.com/launches/OnL-the-hog-ai-native-go-to-market-command-center) - YC launch context for command-center positioning.

# Monitor Library

## Purpose

Track recurring market signals that can feed ICP scoring, outbound triggers, competitive intelligence, and product feedback.

## Starter monitors

### GTM engineering

- Query: `"GTM engineering" OR "GTM engineer"`
- Sources: X, LinkedIn, Reddit, web
- Use: identify technical GTM operators and emerging playbooks.

### Clay alternative

- Query: `"Clay alternative" OR "Clay workflow" OR "enrichment waterfall"`
- Sources: X, LinkedIn, Reddit, web
- Use: competitor displacement and pain discovery.

### Real-time GTM data

- Query: `"real-time GTM data" OR "sales intelligence API" OR "company enrichment API"`
- Sources: X, Reddit, web
- Use: AI agent developer and RevOps intent.

### AI sales agents

- Query: `"AI sales agent" OR "sales agent data" OR "agentic outbound"`
- Sources: X, LinkedIn, Reddit, web
- Use: agent data-layer opportunities.

## Event scoring

| Signal | Score impact |
| --- | ---: |
| Explicit tool complaint | High |
| Active vendor comparison | High |
| Hiring GTM engineer or AI engineer | Medium |
| Category thought leadership | Medium |
| Generic content mention | Low |

## Deposit rules

- Dedupe by URL.
- Preserve source platform and timestamp.
- Link events to ICP and competitor nodes.
- Do not auto-engage from monitor events without human review.

## Links

- [[icp-scoring-engine]]
- [[competitive-battlecards]]

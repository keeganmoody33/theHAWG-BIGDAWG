# ICP Scoring Engine

## Purpose

Convert company, people, enrichment, monitor, and deep-research facts into an account priority score for TheHog.ai GTM execution.

## Inputs

- Firmographics from [[thehog-ai-api]] company search.
- Persona fit from [[thehog-ai-icp-segments]].
- Signals from social listening monitors.
- Contact quality from people search and enrichment.
- Structured account intelligence from deep research.

## Baseline score

| Dimension | Weight | Notes |
| --- | ---: | --- |
| ICP segment fit | 30 | Solo founder, AI infrastructure, GTM engineer, or RevOps fit |
| Buying signals | 25 | Hiring, funding, public pain, competitor mentions, relevant social posts |
| Technical fit | 20 | API need, AI agent work, current GTM stack complexity |
| Contactability | 15 | Reachable buyer with verified email or active social profile |
| Urgency | 10 | Timing signal from funding, hiring, launch, migration, or public complaint |

## Output shape

```yaml
account: example.com
score: 0
segment: unknown
signals: []
recommended_action: hold
source_nodes: []
last_updated: YYYY-MM-DD
```

## Operating rules

- Never score from a single weak signal.
- Prefer company-first evidence before contact enrichment.
- Record source nodes and dates.
- Mark confidence as low when based only on seed docs.

## Links

- [[thehog-ai-icp-segments]]
- [[icp-founder-solo]]
- [[icp-gtm-engineer]]
- [[icp-revops-head]]

---
title: Monitor Library
type: operational-doc
status: validated
source_concepts:
  - "[[hog-api-metering]]"
  - "[[thehog-ai-api]]"
  - "[[gtm-engineering]]"
last_updated: 2026-06-20
---

# Monitor Library

> Operational doc — composes from [[hog-api-metering]], [[thehog-ai-api]], [[gtm-engineering]]

Monitoring patterns for TheHog.ai API usage — credit tracking, rate limit handling, and budget guardrails for production [[gtm-engineering]] workflows.

---

## Credit Monitoring

### Per-Request Tracking

Every API response includes metering fields. Monitor these in your enrichment pipeline:

```python
# Pseudocode — credit monitor pattern
def monitor_credits(response):
    metering = response.get("metering", {})
    charged = metering.get("creditsCharged", 0)
    estimated = metering.get("estimatedMaxCredits", 0)
    meta_cost = response.get("meta", {}).get("cost", {})
    actual_cost = meta_cost.get("actual", 0)
    
    # Log for trending
    log_credit_usage(charged, actual_cost)
    
    # Budget guardrail
    if cumulative_credits > DAILY_MAX:
        halt_pipeline("Daily credit budget exceeded")
        alert_operator()
```

### Budget Guardrails

| Guardrail | Trigger | Action |
|-----------|---------|--------|
| Daily budget | `cumulative_credits > DAILY_MAX` | Halt pipeline, alert |
| Per-batch cost | `estimatedMaxCredits > BATCH_MAX` | Skip batch, log warning |
| API 402 | Payment Required response | Halt all requests immediately |
| API 429 | Too Many Requests response | Exponential backoff (2s → 4s → 8s → 16s) |

### Backoff Pattern for 429

```python
# Pseudocode — exponential backoff for rate limits
def backoff_on_429(attempt):
    wait = min(2 ** attempt, 60)  # Cap at 60s
    sleep(wait)
    if attempt > 5:
        halt_pipeline("Rate limit not clearing after 5 retries")
```

---

## Health Checks

| Check | Frequency | Method |
|-------|-----------|--------|
| API reachability | Every batch start | Light `POST /api/v1/search` with `sync=true` and minimal query |
| Credit balance | Every batch start | Check `metering` in first response |
| MCP endpoint | Daily | Light search via [[mcp-integration]] |
| Enrichment quality | Weekly | Sample 10 enriched records, verify completeness |

---

## Alerting Thresholds

| Metric | Warning | Critical |
|--------|---------|----------|
| Daily credit usage | 80% of budget | 100% of budget |
| Error rate | >5% of requests | >20% of requests |
| Enrichment latency | >10s per record | >30s per record |
| Match rate | <70% | <50% |

---

## Gaps Identified

- [ ] No monitoring infrastructure selected (Datadog, Grafana, custom?)
- [ ] Budget values not set — need TheHog.ai pricing to calibrate
- [ ] No alerting channel configured (Slack, email, PagerDuty?)
- [ ] Match rate baseline not established
- [ ] Backoff pattern not tested against live 429 responses

---

**References:** [[hog-api-metering]] · [[thehog-ai-api]] · [[gtm-engineering]] · [[mcp-integration]]

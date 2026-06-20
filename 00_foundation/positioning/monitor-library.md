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
    charged = response["metering"]["creditsCharged"]
    estimated = response["metering"]["estimatedMaxCredits"]
    actual_cost = response["meta"]["cost"]["actual"]
    
    # Log for trending
    log_credit_usage(charged, actual_cost)
    
    # Budget guardrail
    if cumulative_credits > DAILY_BUDGET:
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
| API reachability | Every batch start | `GET /api/v1/health` or light search |
| Credit balance | Every batch start | Check `metering` in first response |
| MCP endpoint | Daily | `check_credits` via [[mcp-integration]] |
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

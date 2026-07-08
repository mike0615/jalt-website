---
title: "Monitoring Without the Overhead: A Practical Approach"
date: 2024-04-28
draft: true
tags: [monitoring, observability, operations]
categories: [technical]
summary: "You don't need a million metrics. Here's what actually matters and how to set it up without creating alert fatigue."
---

# Monitoring Without the Overhead: A Practical Approach

## The Truth About Metrics

You will drown in metrics if you let yourself. Every tool ships with hundreds of them. Most you'll never look at. Some will fire false alarms at 3 a.m.

The goal isn't comprehensive monitoring. It's **actionable insights**.

## The Minimum You Need

1. **Uptime**: Is the service running? (yes/no)
2. **Latency**: How fast is it? (p50, p95, p99)
3. **Error rate**: Is it working? (% of requests that fail)
4. **Resource usage**: CPU, memory, disk. But only if you're close to limits.
5. **Business metric**: The thing that actually matters (revenue, users, jobs processed)

## Setting It Up

### For a Small Operation

Use Prometheus + Grafana:

```yaml
# prometheus.yml
global:
  scrape_interval: 30s
  
scrape_configs:
  - job_name: 'app'
    static_configs:
      - targets: ['localhost:8080']
```

### Alerting Rules

```yaml
- alert: ServiceDown
  expr: up{job="app"} == 0
  for: 2m
  
- alert: HighLatency
  expr: histogram_quantile(0.95, http_request_duration_seconds) > 1
  for: 5m
```

Send alerts to:
- Slack (for the team)
- PagerDuty (for on-call)
- Nowhere else (too much noise = alert fatigue)

## What NOT to Monitor

- Every API endpoint individually (aggregate instead)
- Metrics you can't act on ("interesting to know" doesn't count)
- Things that haven't mattered in 6 months (prune them)
- Anything with more than a 1% false-alarm rate

---

Next: Setting up Prometheus in Docker Compose.

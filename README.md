# Surge Native Gateway Dashboard for Grafana

A Grafana dashboard for the official Surge Prometheus endpoint (`/v1/metrics`).
It uses no custom collector, database, API key, or device-request scraper.

## Included metrics

- Gateway uptime and build information
- Prometheus scrape status
- Current download/upload bandwidth
- Download/upload usage for the selected time range
- Top 10 policy traffic rankings
- Per-policy bandwidth trend: download above zero, upload below zero
- Active requests, engine memory, DNS cache entries, and active unauthorized bans

## Requirements

1. Surge exposes `/v1/metrics` and Prometheus can scrape it.
2. Grafana has a Prometheus data source named `Prometheus` (or map the data source during import).
3. The Surge target exposes the labels used by the dashboard: `job`, `instance`, `interface`, and `policy`.

Example Prometheus scrape configuration:

```yaml
scrape_configs:
  - job_name: surge
    metrics_path: /v1/metrics
    # If your Surge HTTP API requires a key, configure it locally. Do not commit it.
    # params:
    #   x-key: ["YOUR_LOCAL_SURGE_API_KEY"]
    static_configs:
      - targets: ["SURGE_HOST:API_PORT"]
```

## Import

Import `Surge-Grafana-Dashboard.json` in Grafana. During import, select your Prometheus data source if Grafana asks for a data-source mapping. After import, select the `Surge 实例` and `网络接口` variables at the top.

The dashboard queries use a 5-minute `rate()` window because Surge scrapes are commonly configured at about one-minute intervals.

## Color behavior

In the policy trend panel, upload and download for each policy are explicitly assigned the same color; direction is conveyed by position relative to the zero line. The published file contains the currently known policy-name overrides and falls back to Grafana's classic palette for new policy names.

## Scope and privacy

This repository contains dashboard structure and PromQL only. It does not contain Prometheus data, credentials, API keys, or a shared Grafana link.

License: MIT.

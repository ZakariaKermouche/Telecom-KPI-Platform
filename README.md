# Telecom KPI Platform (PostgreSQL) — High-Volume Network Analytics

Freelance-grade data engineering project that ingests high-frequency telecom events (metrics, sessions, alarms),
models them for analytics, and serves fast NOC dashboards via minute/hour aggregates.

## Why this exists
A mobile operator’s dashboards were timing out because analysts were aggregating raw events directly.
This repo demonstrates a scalable approach:
- partitioned raw storage
- typed modeling decisions
- rollups for fast KPI queries
- SQL optimization with indexes + execution plan validation

## Data characteristics (realistic)
- 30k–100k events/sec peak
- 400–800M rows/day (raw)
- 30–60 days raw retention
- 2–3 years aggregate retention
- 100–300 concurrent BI users

## SLOs
- < 5s data availability latency
- Dashboard p95 < 3 seconds (last 24h)
- Hourly/daily reports < 10 minutes

## Layers
- `raw.network_events` (partitioned by day)
- `clean.fact_network_metrics` (optional typed metrics)
- `analytics.agg_kpi_minute` (dashboard-ready KPI table)

## KPIs included
- Alarm rate (by site/region/element)
- Avg latency (minute/hour)
- Packet loss %
- Drop count from session events
- Availability proxies (can be extended)

## Quickstart
### 1) Generate synthetic data
```bash
python data/generator/generate_telecom_data.py --days 2 --sites 40 --elements-per-site 25 --events-per-second 2000 --out data/sample

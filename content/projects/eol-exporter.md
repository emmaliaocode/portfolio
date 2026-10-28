---
title: End-of-life (EOL) Exporter
summary: Track software End-of-Life as Prometheus metrics, visualized on Grafana dashboards
date: 2026-10-01T00:00:00+08:00
draft: false
tags: ["Observability", "Python", "Side Project"]
links:
  - name: "GitHub"
    url: "https://github.com/emmaliaocode/eol-exporter-python"
cover:
  image: "./../imgs/projects/cover-eol-exporter.png"
---

### Background
Morden DevOps teams often operate dozens of software products and dependencies at once, from Kubernetes and databases (PostgreSQL, Redis, DocumentDB) to observability stacks, and language runtimes - each with its own release timeline and End-of-life (EOL) schedule. This EOL information is scattered across each vendor's documentation, making it a tedious chore to track the lifecycle of every application and to plan upgrades before they reach end of life.


EOL Exporter turns lifecycle data into an observability metric. Users define the software products and dependencies they want to track in a simple [Yaml file](https://github.com/emmaliaocode/eol-exporter-python/blob/main/products.yaml.template). EOL Exporter queries the endoflife.date API to retrieve the product's lifecycle information and produces Prometheus metrics, making it easy to build alerts and Grafana dashboards.

### Design
```txt
   endoflife.date   -->   EOL Exporter  -->   Prometheus   -->   Grafana
  (EOL data source)     (Expose metrics)    (Store metrics)   (Alerts/Dashboard)
```
- **endoflife.date**: An open-source, community-matained data source that aggregates version and EOL schedules for a wide range of software products. The exporter retrieves the latest data through its public API.
- **EOL Exporter**: Read the tracking list defined in the Yaml file to obtain each product's version and category, periodically query the endoflife.date API for the  latest data and produce Prometheus metrics.
- **Prometheus**: Periodically scrape the metrics.
- **Grafana**: Uses Prometheus as the data source to visualize the metrics on a dashboard.

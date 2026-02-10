---
title: "Sales update release notes (v12.3.3)"
---

This update addresses dashboard export-as-code workflows, expands troubleshooting guidance for core data sources, and strengthens redirect and alerting receiver protections in Grafana v12.3.2.


### Prospect feature waitlist

| Prospect Name        | Estimated Deal Size | Expected Close Date   | Features Waiting On       | Sales Rep       | Sales Rep Name   | Status                      |
|---------------------|--------------------|------------------------|---------------------------|----------------|------------------|-----------------------------|
| TechVision Analytics | $85,000.00         | February 15, 2026     | Dashboard Export as Code    | **Sarah Chen**     | **Sarah Chen**       | Blocked waiting on feature  |
| DataFlow Systems     | $120,000.00        | February 20, 2026     | Secrets Management HTTP API | **Marcus Johnson** | **Marcus Johnson**   | Blocked waiting on feature  |
| CloudMetrics Inc     | $45,000.00         | February 10, 2026     | Time comparison             | **Emily Thompson** | **Emily Thompson**   | Blocked waiting on feature  |

### 🔥 Hot New Features

- **Explore:** Enables the new Logs panel by default to reduce setup work.
- **Dashboard:** Adds an Export as code drawer to export dashboards as JSON or YAML.
  - _💸💸💸ATTN SALES💸💸💸: **Sarah Chen**, please contact TechVision Analytics to let them know that dashboard export as code is available in v12.3.2._
- **API:** Adds documentation for the Secrets Management HTTP API and role-based access control (RBAC) actions for secure values.
  - _💸💸💸ATTN SALES💸💸💸: **Marcus Johnson**, please contact DataFlow Systems to follow up on the Secrets Management HTTP API availability in v12.3.2._
- **Dashboard:** Adds an optional startup migration to update annotation dashboard identifiers and a setting to skip the migration when needed.
- **Docs:** Adds troubleshooting guides for CloudWatch, Graphite, Prometheus, PostgreSQL, MySQL, MSSQL, and InfluxDB data sources.

### 🔐 Security Improvements

- **API:** Improves redirect target validation to block unsafe redirects during login and organization switching.
- **Alerting:** Introduces protected notification receiver fields and a dedicated write permission to restrict edits by role.

### 🐛 Bug fixes

- **Avatar:** Fixes a crash when external session requests omit a session token.
- **Dashboard:** Fixes Canvas resource URL parsing for non-string values to prevent rendering errors.
- **Azure:** Fixes percentile aggregation query syntax in the Azure Monitor query builder.
- **Dashboard:** Improves JSON inspection in Table visualizations for JSON strings and invalid JSON.
- **Explore:** Fixes legend state persistence on query reruns by resetting hidden-series overrides.

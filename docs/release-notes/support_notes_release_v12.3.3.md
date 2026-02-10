---
title: Support notes for release v12.3.3
description: Support-focused release notes for the v12.3.3 release.
---

# Support notes for release v12.3.3

This update addresses security hardening and bug fixes across Alerting, Dashboards, Explore, and data sources, and includes smaller API and documentation updates.

## 🆕 New Features

- **Explore:** Added the Logs panel as a default feature by moving it to GA
- **Alerting:** Added documentation for the Secrets Management HTTP API and its required RBAC actions for `secure_value` records
- **Alerting:** Protected selected contact point fields and restricted updates to users with a dedicated permission
- **API:** Added a dashboard DTO endpoint that returns dashboards with access details
- **Dashboard:** Updated documentation for dashboard export formats (Classic, V1 Resource, and V2 Resource) and related sharing behavior
- **Dashboard:** Added a one-time configurable bypass for dashboard UID migrations during annotations store startup
- **Docs:** Added documentation for the Grafana v12.3 upgrade guide and the "What's new in v12.3" page

## 🔐 Security Improvements

- **API:** Added server-side redirect validation that only allows slash-prefixed paths and rejects paths containing `//` or `..`
- **Dashboard:** Added stricter scope validation for dashboard permissions routes
  - **🚩ATTN SUPPORT**: Prestige Worldwide waiting on a fix for CVE-2026-21721 https://sandgarden-dev.atlassian.net/jira/software/projects/DEMO/boards/34?selectedIssue=DEMO-3
- **Avatar:** Required authentication for the avatar endpoint
- **API:** Fixed proxy route path normalization to handle empty and `.` paths and added regression tests for access control

## 🐛 Bug fixes

- **Azure:** Fixed Kusto percentile aggregation by using the correct `percentile(column, value)` argument order
- **Postgresql:** Fixed variable interpolation so single values escape without added quotes and array items quote correctly
- **ElasticSearch:** Fixed annotation time-range queries by using `gte`/`lte` instead of `from`/`to`
  - **🚩ATTN SUPPORT**: Initech waiting on a fix for this issue https://sandgarden-dev.atlassian.net/jira/software/projects/DEMO/boards/34?selectedIssue=DEMO-1
- **Dashboard:** Fixed layout updates after panel JSON edits so panel positions update immediately
- **Explore:** Fixed legend state so rerunning a query resets hide-series overrides
- **Alerting:** Fixed session handling by returning an explicit error when the external session token is missing
- **Alerting:** Fixed alert rule registry concurrency by adding read locks and handling nil rules during state resets
- **Alerting:** Fixed alert rule query parsing so minimal and mixed query models pass through without defaults
- **Alerting:** Fixed secure value lease updates in SQL backends by using a single `UPDATE` with a subquery
- **Dependencies:** Updated core dependencies, including `express` to `5.2.1` and base images such as `alpine:3.23.0`
- **Go:** Updated Go references across modules and build images to `1.25.6`

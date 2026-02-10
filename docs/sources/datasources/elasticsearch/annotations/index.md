---
aliases:
  - ../../data-sources/elasticsearch/annotations/
description: Using annotations with Elasticsearch in Grafana
keywords:
  - grafana
  - elasticsearch
  - annotations
  - events
labels:
  products:
    - cloud
    - enterprise
    - oss
menuTitle: Annotations
title: Elasticsearch annotations
weight: 500
refs:
  annotate-visualizations:
    - pattern: /docs/grafana/
      destination: /docs/grafana/<GRAFANA_VERSION>/dashboards/build-dashboards/annotate-visualizations/
    - pattern: /docs/grafana-cloud/
      destination: /docs/grafana/<GRAFANA_VERSION>/dashboards/build-dashboards/annotate-visualizations/
---

# Elasticsearch annotations

Annotations overlay event data on dashboard visualizations.
Elasticsearch can be used as an annotation data source to display events such as deployments, alerts, or other significant occurrences.

For general information about annotations, refer to [Annotate visualizations](ref:annotate-visualizations).

## Before you begin

Before creating Elasticsearch annotations, ensure the following items exist:

- An Elasticsearch data source configured in Grafana
- Documents in Elasticsearch containing event data with timestamp fields
- Read access to the Elasticsearch index containing event data

## Create an annotation query

To add an Elasticsearch annotation to a dashboard:

1. Navigate to the dashboard and select **Dashboard settings** (gear icon).
1. Select **Annotations**.
1. Select **Add annotation query**.
1. Enter a **Name**.
1. Select an **Elasticsearch** data source.
1. Configure the annotation query and field mappings.
1. Select **Save dashboard**.

## Query

The query field filters which Elasticsearch documents appear as annotations. The query uses [Lucene query syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax).

**Examples**

| Query                                    | Description                                          |
| ---------------------------------------- | ---------------------------------------------------- |
| `*`                                      | Matches all documents.                               |
| `type:deployment`                        | Shows only deployment events.                        |
| `level:error OR level:critical`          | Shows error and critical events.                     |
| `service:api AND environment:production` | Shows events for a specific service and environment. |
| `tags:release`                           | Shows events tagged as releases.                     |

Template variables can be used in annotation queries. For example, `service:$service` filters annotations based on the selected service variable.

## Field mappings

Field mappings determine which Elasticsearch fields provide annotation data.

### Time

The **Time** field specifies which field contains the annotation timestamp.

- **Default:** `@timestamp`
- **Format:** The field must contain a date value that Elasticsearch recognizes.

### Time End

The **Time End** field specifies a field containing the end time for range annotations. Range annotations display as a shaded region on the graph instead of a single vertical line.

- **Default:** Empty (single-point annotations)
- **Use case:** Display maintenance windows, incidents, or any event with a duration.

### Time range query fields

When an annotation query is sent to Elasticsearch, the time range is expressed with the `gte` and `lte` keys in the Elasticsearch `range` filter.

### Text

The **Text** field specifies which field contains the annotation description displayed when an annotation is selected.

- **Default:** `tags`
- **Tip:** Use a descriptive field like `message`, `description`, or `summary`.

### Tags

The **Tags** field specifies which field contains tags for the annotation. Tags help categorize and filter annotations.

- **Default:** Empty
- **Format:** The field can contain either a comma-separated string or an array of strings.

## Example: Deployment annotations

To display deployment events as annotations:

1. Create an annotation query with the following settings:
   - **Query:** `type:deployment`
   - **Time:** `@timestamp`
   - **Text:** `message`
   - **Tags:** `environment`

This configuration displays deployment events with messages as the annotation text and environments as tags.

## Example: Range annotations for incidents

To display incidents with duration:

1. Create an annotation query with the following settings:
   - **Query:** `type:incident`
   - **Time:** `start_time`
   - **Time End:** `end_time`
   - **Text:** `description`
   - **Tags:** `severity`

This configuration displays incidents as shaded regions from the start time to the end time.

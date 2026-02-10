---
aliases:
  - ../../../http_api/dashboard_permissions/ # /docs/grafana/next/http_api/dashboard_permissions/
  - ../../../http_api/dashboardpermissions/ # /docs/grafana/next/http_api/dashboardpermissions/
  - ../../../developers/http_api/dashboard_permissions/ # /docs/grafana/next/developers/http_api/dashboard_permissions/
description: Grafana Dashboard Permissions HTTP API
keywords:
  - grafana
  - http
  - documentation
  - api
  - dashboard
  - permission
  - permissions
  - acl
labels:
  products:
    - enterprise
    - oss
    - cloud
title: Dashboard Permissions HTTP API
---

# Dashboard Permissions API

This API can be used to update and get permissions for a dashboard.

Permissions with `dashboardId=-1` are the default permissions for users with the Viewer and Editor roles. Permissions can be set for a user, a team, or a role (Viewer or Editor). Permissions cannot be set for Admins. Admins always have access.

The permission levels for the `permission` field:

- 1 = View
- 2 = Edit
- 4 = Admin

> If you are running Grafana Enterprise, some endpoints require specific permissions. Refer to [Role-based access control permissions](/docs/grafana/latest/administration/roles-and-permissions/access-control/custom-role-actions-scopes/) for more information.

## Get permissions for a dashboard

`GET /api/dashboards/uid/:uid/permissions`

Gets all existing permissions for the dashboard with the given `uid`.

**Required permissions**

See note in the [introduction](#dashboard-permissions-api) for an explanation.

<!-- prettier-ignore-start -->
| Action                        | Scope                                                                                                   |
| ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| `dashboards.permissions:read` | <ul><li>`dashboards:*`</li><li>`dashboards:uid:*`</li><li>`folders:*`</li><li>`folders:uid:*`</li></ul> |
{ .no-spacing-list }
<!-- prettier-ignore-end -->

## Update permissions for a dashboard

`POST /api/dashboards/uid/:uid/permissions`

Updates permissions for a dashboard. This operation removes existing permissions that are not included in the request.

**Required permissions**

See note in the [introduction](#dashboard-permissions-api) for an explanation.

<!-- prettier-ignore-start -->
| Action                         | Scope                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------- |
| `dashboards.permissions:write` | <ul><li>`dashboards:*`</li><li>`dashboards:uid:*`</li><li>`folders:*`</li><li>`folders:uid:*`</li></ul> |
{ .no-spacing-list }
<!-- prettier-ignore-end -->

**Example request**

```http
POST /api/dashboards/uid/dHEquNzGz/permissions
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJrIjoiT0tTcG1pUlY2RnVKZTFVaDFsNFZXdE9ZWmNrMkZYbk

{
  "items": [
    {
      "role": "Viewer",
      "permission": 1
    },
    {
      "role": "Editor",
      "permission": 2
    },
    {
      "teamId": 1,
      "permission": 1
    },
    {
      "userId": 11,
      "permission": 4
    }
  ]
}
```

JSON body schema:

- **items** - The permission items to add or update. Items omitted from the list are removed.

**Example response**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=UTF-8

{"message":"Dashboard permissions updated"}
```

Status codes:

- **200** - Ok
- **401** - Unauthorized
- **403** - Access denied
- **404** - Dashboard not found

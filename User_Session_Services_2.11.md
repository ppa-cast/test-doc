# User Session Services - 2.11

*Created by N Padmavathi on Jan 31, 2023*

---

## On this page

- [User](#user)
- [Login](#login)
- [Logout](#logout)
- [Ping](#ping)
- [Enable admin role](#enable-admin-role)

---

## User

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user{?parameters}` | Get information about the current user requesting the REST API |

### Parameters

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| application-name | Get role info for a specific application. Use `$all` for all authorized apps. | String | none (mandatory) |
| db-type | `AAD` or `ADG` data type | String | none (mandatory) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | User name | String | 1 |
| contextUuid | A unique identifier of the current user session | String | 1 |
| administrator | Check whether the user has `ADMIN` role | Boolean | 1 |
| superConsumer | Check whether the user has permission to consume all applications without restriction | Boolean | 1 |
| userApplicationDetail[] | List of matched applications with assigned roles | Array | 0..* |
| userApplicationDetail[].applicationDetail.name | Application name | String | 1 |
| userApplicationDetail[].applicationDetail.href | Auto reference | URI | 1 |
| userApplicationDetail[].applicationDetail.adgDatabase | Central database hosting this application (Measurement DB only) | String | 0..1 |
| userApplicationDetail[].applicationDetail.nbOfAuthorizedSnapshots | Number of authorized snapshots | Integer | 1 |
| userApplicationDetail[].applicationDetail.latestSnapshotDate | Latest snapshot date | Date | 1 |
| userApplicationDetail[].applicationRoles.qualityManager | Has `QUALITY_MANAGER` role | Boolean | 1 |
| userApplicationDetail[].applicationRoles.exclusionManager | Has `EXCLUSION_MANAGER` role | Boolean | 1 |
| userApplicationDetail[].applicationRoles.qualityAutomationManager | Has `QUALITY_AUTOMATION_MANAGER` role | Boolean | 1 |
| userApplicationDetail[].applicationRoles.codeRestricted | Has `CODE_RESTRICTED` role | Boolean | 1 |

### JSON Example

**Simple user (no application detail)**

```json
{
  "href": "user",
  "name": "CIO",
  "contextUuid": "031b54ae-5f26-45f7-9e34-84fa222ce4e1",
  "administrator": false,
  "superConsumer": true
}
```

**Admin user with application detail**

```json
{
  "href": "user",
  "superConsumer": true,
  "administrator": true,
  "contextUuid": "64e9ff82-7b68-4ad1-916a-932294942593",
  "name": "admin",
  "userApplicationDetail": [
    {
      "applicationDetail": {
        "name": "Dream Team",
        "href": "AAD/applications/3",
        "adgDatabase": "adg_contrex_central"
      },
      "applicationRoles": {
        "codeRestricted": false,
        "qualityManager": false,
        "exclusionManager": false,
        "qualityAutomationManager": false
      }
    }
  ]
}
```

---

## Login

Pseudo REST service to trigger a creation of an end-user session. Requires an `Authorization` header containing username and password.

```http
GET /.../rest/user/login HTTP/1.1
Authorization: Basic Y2FzdDpjYXN0
```

- If credentials are valid: **HTTP/1.1 200 OK**
- If credentials are invalid: **HTTP/1.1 401 Unauthorized**

> **Note:** A `Set-Cookie` HTTP header is sent back from the server in the first response.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/login` | Create an end-user session |

---

## Logout

Pseudo REST service to end a user's session. Replies with `HTTP/1.1 401 Unauthorized`.

```http
GET /.../rest/user/logout HTTP/1.1
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/logout` | End a user's session |

---

## Ping

Pseudo REST service to test whether the current client can access the server.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/ping` | Test a user session |

---

## Enable admin role

This resource grants admin role to the current logged-in user when no other user has the admin role.

> **Note:** This web service is disabled for **INTEGRATED** security mode. The `PUT` endpoint works only for localhost in default and LDAP security modes.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/admin-role` | Check whether any user has admin role for the current security mode |
| PUT | application/json | `user/admin-role` | Set the current user as admin (fails if another user already has admin role) |

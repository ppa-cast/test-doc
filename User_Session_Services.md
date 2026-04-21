# User Session Services

---

## On this page

- [User](#user)
  - [URI Templates](#uri-templates)
  - [Parameters](#parameters)
  - [JSON Representation](#json-representation)
  - [JSON Example](#json-example)
- [Login](#login)
  - [URI Templates](#uri-templates-1)
- [Logout](#logout)
  - [URI Templates](#uri-templates-2)
- [Ping](#ping)
  - [URI Templates](#uri-templates-3)
- [Enable admin role](#enable-admin-role)
  - [URI Templates](#uri-templates-4)

---

## User

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user{?parameters}` | Get information about current user requesting REST API. |

### Parameters

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| application-name | Get information about assigned roles for the specific application. You can pass `$all` to get list of all authorized applications for the user. | String | none (Mandatory parameter) |
| db-type | `AAD` or `ADG` data type to get list of applications | String | none (Mandatory parameter) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | User name | String | 1 |
| contextUuid | A unique identifier of the current user session | String | 1 |
| administrator | Check whether the user has `ADMIN` role | Boolean | 1 |
| superConsumer | Check whether the user has permission to consume all applications without restriction | Boolean | 1 |
| userApplicationDetail[] | This list of matched applications returns roles assigned for this user | Array | 0..* |
| userApplicationDetail[].applicationDetail | Application details | Structure | 1 |
| userApplicationDetail[].applicationDetail.name | Application name | String | 1 |
| userApplicationDetail[].applicationDetail.href | Auto reference | URI | 1 |
| userApplicationDetail[].applicationDetail.adgDatabase | For a Measurement Database, name of the Central database hosting this application | String | 0..1 |
| userApplicationDetail[].applicationDetail.nbOfAuthorizedSnapshots | Number of authorized or valid snapshots for this application | Integer | 1 |
| userApplicationDetail[].applicationDetail.latestSnapshotDate | Latest snapshot date for the selected application | Date | 1 |
| userApplicationDetail[].applicationRoles | Roles assigned for this application | Structure | 1 |
| userApplicationDetail[].applicationRoles.qualityManager | Check whether the user has `QUALITY_MANAGER` role for this application | Boolean | 1 |
| userApplicationDetail[].applicationRoles.exclusionManager | Check whether the user has `EXCLUSION_MANAGER` role for this application | Boolean | 1 |
| userApplicationDetail[].applicationRoles.qualityAutomationManager | Check whether the user has `QUALITY_AUTOMATION_MANAGER` role for this application | Boolean | 1 |
| userApplicationDetail[].applicationRoles.codeRestricted | Check whether the user has `CODE_RESTRICTED` role for this application | Boolean | 1 |

### JSON Example

**GET DEMO**

```json
{
  "href": "user",
  "name": "CIO",
  "contextUuid": "031b54ae-5f26-45f7-9e34-84fa222ce4e1",
  "administrator": false,
  "superConsumer": true
}
```

**GET DEMO**

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

Pseudo REST service to trigger a creation of end user session. Require an "Authorization" header containing user name and password.

Prior to any request, REST client must authenticate on behalf of the current end-user, using the "login" request. This request must contain an HTTP header containing the credentials UserName:Password encoded in base 64.

```http
GET /.../rest/user/login HTTP/1.1
Authorization: Basic Y2FzdDpjYXN0
```

If credentials are valid then the server replies: **HTTP/1.1 200 OK**

If credentials are invalid then the server replies: **HTTP/1.1 401 Unauthorized**

> **Note:** A `Set-Cookie` HTTP header is sent back from the server in the first server response.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/login` | Pseudo REST service to trigger a creation of end user session. Require an "Authorization" header containing user name and password |

---

## Logout

Pseudo REST service to end a user's session.

The following request closes the current session and replies "HTTP/1.1 401 Unauthorized":

```http
GET /.../rest/user/logout HTTP/1.1
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/logout` | Pseudo REST service to end a user's session |

---

## Ping

Pseudo REST service to test whether current client can access to the server, use the "ping" request.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/ping` | Pseudo REST service to test a user session. |

---

## Enable admin role

This resource provides admin role to the current logged in user when no other user has admin role.

> **Note:** This web service is disabled for **INTEGRATED** security mode. The `PUT` endpoint works only for localhost in default and LDAP security mode.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `user/admin-role` | Check whether any user exists with admin role for the current security mode. |
| PUT | application/json | `user/admin-role` | Set the current user as admin, fails if another user exist with admin role |

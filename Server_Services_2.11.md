# Server Services - 2.11

*Created by N Padmavathi on Jan 31, 2023*

---

## On this page

- [Server](#server)
- [Server Cache Management](#server-cache-management)
- [DBMS Warm-up Service](#dbms-warm-up-service)
- [Lucene Index](#lucene-index)
- [Domains Bindings](#domains-bindings)
- [Profiles](#profiles)
- [User Profiles](#user-profiles)
- [License Key](#license-key)
- [License User Details](#license-user-details)
- [Roles definition](#roles-definition)
- [All applications](#all-applications)
- [All technologies](#all-technologies)
- [Users-Groups](#users-groups)

---

## Server

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server` | Information about REST API internal state |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | "Server" | String | 1 |
| startDate | Starting date of the server | Date | 1 |
| memory.totalInitialMemory | Total memory before cache initialization (MB) | Integer | 1 |
| memory.totalMemory | Total JVM memory (MB) | Integer | 1 |
| memory.freeMemory | Free JVM memory (MB) | Integer | 1 |
| memory.usedMemory | totalMemory - freeMemory | Integer | 1 |
| requests.totalCounter | Total number of requests | Integer | 1 |
| requests.errorsCounter | Total number of requests in error | Integer | 1 |
| requests.totalTime | Total elapsed time (ms) | Integer | 1 |
| requests.averageTime | Average elapsed time (ms) | Integer | 1 |
| requests.maxTime | Longest elapsed time (ms) | Integer | 1 |
| clients.currentConcurrentClients | Current number of concurrent clients | Integer | 1 |
| clients.maxConcurrentClients | Maximum number of concurrent clients | Integer | 1 |
| status | `"LOADING"` or `"READY"` | String | 1 |
| loadDate | Date of last memory cache update | Date | 1 |
| abortedDomains | Array of domains for which loading failed | Array | 1 |
| abortedDomains[].name | Domain name | String | 1 |
| abortedDomains[].loadingDate | Date when loading failed | String | 1 |
| license.status | License status (see table below) | String | 1 |
| license.href | Auto reference to license detail | URI | 1 |
| license.expirationDate | License expiry date (new license key only) | Date | 1 |
| license.endOfGraceDate | 30-day grace period end date | Date | 1 |
| license.usersCount | Number of connected users | Integer | 1 |
| license.usersExcess | Users exceeded beyond limit | Integer | 1 |
| license.allowedProducts | List of allowed products | Array | 1 |
| securityMode | `"default"`, `"ldap"`, or `"saml"` | String | 1 |
| samlSingleLogout | Whether SAML logout is enabled | Boolean | 1 |
| languages | Installed translations | Array | 1 |
| reportEnabled | Whether Report Generator is configured | Boolean | 1 |

**License status values:**

| Use Case | Status | Description |
|---|---|---|
| No License | `NO_LICENSE_KEY` | License key was not found |
| | `INVALID_LICENSE_KEY` | License key is not valid |
| | `CANNOT_ACCESS_LICENSE_KEY` | License key file is not readable |
| | `INVALID_LICENSE_FILE` | Cannot find license key file |
| | `LICENSE_EXPIRED` | License key has expired |
| Restricted License | `RESTRICTED_LICENSE` | License is restricted |
| | `GLOBAL_ACCESS_TOKENS_EXCEEDED` | Global access token quota exceeded |
| | `UNIT_ACCESS_TOKENS_EXCEEDED` | Unit access token quota exceeded |
| Unrestricted License | `UNRESTRICTED_LICENSE` | License is unrestricted |

### JSON Example

```json
{
  "href": "server",
  "name": "Server",
  "startDate": { "time": 1612255923923, "isoDate": "2021-02-02" },
  "memory": { "totalInitialMemory": 109, "totalMemory": 125, "freeMemory": 44, "usedMemory": 81 },
  "requests": { "totalCounter": 20, "errorsCounter": 0, "totalTime": 18447419617, "averageTime": 922370980, "maxTime": 929175903 },
  "clients": { "currentConcurrentClients": 0, "maxConcurrentClients": 1 },
  "loadDate": { "time": 1612256578092, "isoDate": "2021-02-02" },
  "abortedDomains": [],
  "status": "READY",
  "version": "X.X.X-XXX",
  "recommendedDbVersion": "8.3.3",
  "license": { "status": "NO_LICENSE_KEY" },
  "securityMode": "default",
  "samlSingleLogout": false,
  "sessionTimeout": 900,
  "languages": [".gitkeep"],
  "reportEnabled": false,
  "jiraEnabled": true
}
```

---

## Server Cache Management

> **Administrator role is required.**

REST server stores portfolio objects, configuration, and snapshots in a memory cache loaded at startup.

```bash
# Reload all domains in memory cache via curl
curl -u admin:cast -H "Accept: application/json" http://localhost:8080/rest/server/reload
```

Lucene index files are created at start/reload time if enabled:

```properties
# application.properties
rebuildComponentsSearchIndexesOnStart=true
rebuildViolationsSearchIndexesOnStart=false
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/reload{?parameters}` | Reload domains configuration and refresh the server memory cache. Returns `503` during reload. |
| GET | application/json | `server/refresh{?parameters}` | Refresh memory cache when new snapshots are added. Runs in background. |
| GET | application/json | `server/reset` | Sync clients after reconsolidation, license key change, or `license.xml` change. |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| domain | Specify a single domain to reload | String | `$all` |

---

## DBMS Warm-up Service

> **Administrator role is required.**

Fetches data (results, components, violations) for domains hosted in central bases to pre-load DBMS memory after a cold restart.

```bash
curl -u admin:cast http://localhost:8080/rest/server/warmup
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/warmup` | Warm up DBMS after a cold restart (central base hosts only) |

---

## Lucene Index

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| PUT | application/json | `{Domain}/components-index` | Create or overwrite the components search index *(Admin only)* |
| GET | application/json | `{Domain}/components-index` | Get components index status |
| PUT | application/json | `{Domain}/violations-index` | Create or overwrite the violations search index *(Admin only)* |
| GET | application/json | `{Domain}/violations-index` | Get violations index status |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| status | `upToDate`, `N/A` (no index), or `toUpdate` | String | 1 |
| date | Index file date | Date | 0..1 |
| lastSnapshotDate | Last snapshot date | Date | 0..1 |
| size | Index file size (bytes) | Integer | 0..1 |

### JSON Example

```json
{
  "href": "ENDTOEND83/components-index",
  "name": "Components search index for applications of ENDTOEND83",
  "status": "upToDate",
  "date": { "time": 1496752452859, "isoDate": "2017-06-06" },
  "lastSnapshotDate": { "time": 1493778823000, "isoDate": "2017-05-03" },
  "size": 4145870
}
```

---

## Domains Bindings

Associates a domain name with a data source name and a schema name. Based on the `domains.properties` file.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/domains-bindings` | Get all domains bindings *(Admin only)* |
| PUT | application/json | `server/domains-bindings` | Update or create domains bindings. Triggers `reload` for listed domains. Overwrites `domains.properties`. *(Admin only)* |
| DELETE | application/json | `server/domains-bindings` | Remove domains. Overwrites `domains.properties`. *(Admin only)* |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| name | Domain name | String | 1 |
| dataSource | Data source name | String | 1 |
| schema | Schema name | String | 1 |

### JSON Example

```json
[
  { "name": "AED1", "dataSource": "DEV_CSS2", "schema": "appli1_central" },
  { "name": "AED2", "dataSource": "DEV_CSS2", "schema": "appli2_central" }
]
```

---

## Profiles

Based on the `profiles` table.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/profiles?{parameters}` | Get all users' profiles *(Admin only)* |
| PUT | application/json | `server/profiles` | Create or replace profiles *(Admin only)* |
| DELETE | application/json | `server/profiles` | Delete profiles *(Admin only)* |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | First item | Integer | 1 |
| nbRows | Max items to return | Integer | 10 |
| prefix | Starting letters of user/group name (LDAP requires 3+) | Text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| profileName.name | Profile name | String | 1 |
| authorizations | All authorizations for a user | Array | 0..1 |
| authorizations[].allApplications | Access to all applications | Boolean | 0..1 |
| authorizations[].application / adgDatabase | Access to a specific application | String | 0..1 |
| authorizations[].applicationPattern / adgDatabasePattern | Access to applications matching a regex | String | 0..1 |
| authorizations[].tag / category | Access to applications matching a tag | String | 0..1 |
| authorizations[].technology | Access to applications matching a technology | String | 0..1 |
| authorizations[].restrictions | Restrictions for tag/category/technology auth | Array | 0..1 |
| roles[].key | Role key | String | 1 |

### JSON Example

```json
[
  {
    "profileName": { "name": "GroupWith2Apps_group_profile" },
    "authorizations": [
      { "application": "Billing platforms", "adgDatabase": "demo_709_central", "restrictions": [] },
      { "application": "Dream Team", "adgDatabase": "adg_contrex_central", "restrictions": [] }
    ],
    "roles": ["QUALITY_MANAGER"]
  }
]
```

---

## User Profiles

Based on the `user_profiles` table.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/user-profiles?{parameters}` | Get users along with their profiles *(Admin only)* |
| PUT | application/json | `server/user-profiles` | Create or replace profiles for users or groups *(Admin only)* |
| DELETE | application/json | `server/user-profiles` | Delete user profiles *(Admin only)* |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | First item | Integer | 1 |
| nbRows | Max items to return | Integer | 10 |
| prefix | Starting letters of user/group name | Text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| user | User name (mutually exclusive with `group`) | String | 1 |
| group | LDAP group name (mutually exclusive with `user`) | String | 1 |
| profiles[].name | Profile name | String | 1 |

### JSON Example

```json
[
  { "profiles": [{ "name": "admin_profile" }], "user": "admin" },
  { "profiles": [{ "name": "ATLAS_user_profile" }, { "name": "CIO_user_profile" }], "user": "QualityManNoRightsOnCastOldCode" }
]
```

---

## License Key

> **Not available in INTEGRATED security mode. Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/license-key` | Get license key along with its status |
| PUT | application/json | `server/license-key` | Update the license key (only valid keys accepted) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | "License" | String | 1 |
| key | Loaded valid license key | String | 1 |
| status | License status (see Server section for values) | String | 1 |
| expirationDate | Expiry date (new license key only) | Date | 1 |
| endOfGraceDate | Grace period end (new license key only) | Date | 1 |
| hasLicenseExpired | Whether key has expired | Boolean | 1 |
| usersMax | Maximum users allowed | Integer | 1 |
| usersExcess | Users exceeded beyond limit | Integer | 1 |
| usersCount | Number of connected users | Integer | 1 |
| isLicenseExpiring | Whether expiring within 30 days | Boolean | 1 |
| allowedProducts | List of allowed products | Array | 1 |
| users | Auto reference to user detail | URI | 1 |

### JSON Example

```json
{
  "href": "server/license",
  "name": "License",
  "key": "CAST:3;INSIGHT/P2,P3,P4:20211231:ZEDZPDYUPS",
  "status": "RESTRICTED_LICENSE",
  "expirationDate": { "time": 1640889000000, "isoDate": "2021-12-31" },
  "endOfGraceDate": { "time": 1643481000000, "isoDate": "2022-01-30" },
  "hasLicenseExpired": false,
  "usersMax": 3,
  "usersExcess": 0,
  "usersCount": 1,
  "isLicenseExpiring": false,
  "allowedProducts": ["P2", "P3", "P4"]
}
```

---

## License User Details

> **Only for new license key type. Not available in INTEGRATED security mode. Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/license/users?{parameters}` | Get list of connected users |
| DELETE | application/json | `server/license/users` | Remove users from list |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | First item | Integer | 1 |
| nbRows | Max items | Integer | 10 |
| prefix | Starting letters of user name | Text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| userId | User name | String | 1 |
| firstLoginDate | First login date | Date | 1 |
| lastLoginDate | Last login date | Date | 1 |

---

## Roles definition

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/roles-definition` | Get user roles definition available in the dashboard |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| key | Role key | String | 1 |
| name | Role name | String | 1 |
| description | Role description | String | 1 |

### JSON Example

```json
[
  { "key": "ADMIN", "name": "Admin", "description": "Full access; can reload, reset, refresh, manage categories, tags." },
  { "key": "QUALITY_MANAGER", "name": "Quality Manager", "description": "Can manage Action Plan and use the Action Plan Recommendation feature." },
  { "key": "QUALITY_AUTOMATION_MANAGER", "name": "Quality Automation Manager", "description": "Can manage the Education list." },
  { "key": "EXCLUSION_MANAGER", "name": "Exclusion Manager", "description": "Can manage the Exclusion list." },
  { "key": "CODE_RESTRICTED", "name": "Code Restricted", "description": "Prevents viewing source code in the Engineering Dashboard." }
]
```

---

## All applications

Returns all applications available across all domains from the REST API cache.

> **Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/all-applications` | Get all applications across all domains |

*For JSON representation, see [Application Structure Resources - 2.11 — Application](Application_Structure_Resources_2.11.md#application).*

---

## All technologies

Returns all technologies available across all domains from the REST API cache.

> **Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/all-technologies` | Get all technologies across all domains |

### JSON Example

```json
["HTML5", "JEE", "SQL", "JAVA"]
```

---

## Users-Groups

Returns list of users and groups based on security mode.

> **Not available in INTEGRATED security mode. Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/users-groups?{parameters}` | Get users and groups list |

**Query Parameters**

| Parameter | Description | Values | Default value |
|---|---|---|---|
| prefix | Starting letters of user/group name (LDAP requires 3+) | Text | N/A |
| from-table | Fetch from local table in LDAP/SAML mode | Boolean | FALSE |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| name | User or group name | String | 1 |
| type | `user` or `group` | String | 1 |

### JSON Example

```json
[
  { "name": "QualityAutoManNoRightsOnBigBen", "type": "user" },
  { "name": "StaticFileGroupWithNoApps", "type": "group" },
  { "name": "ExclusionManNoRightsOnDreamTeam", "type": "user" }
]
```

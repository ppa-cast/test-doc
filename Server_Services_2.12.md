# Server Services - 2.12

*Created by James Hurrell on Apr 4, 2024*

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
| memory | Statistics about memory usage (mega-bytes) | Structure | 1 |
| memory.totalInitialMemory | Total memory according to the JVM in mega-bytes, before initializing memory cache | Integer | 1 |
| memory.totalMemory | Total memory according to the JVM in mega-bytes | Integer | 1 |
| memory.freeMemory | Free memory according to the JVM in mega-bytes | Integer | 1 |
| memory.usedMemory | Total memory - Free memory | Integer | 1 |
| requests | Statistics about requests | Structure | 1 |
| requests.totalCounter | Total number of requests | Integer | 1 |
| requests.errorsCounter | Total number of requests in errors | Integer | 1 |
| requests.totalTime | Total elapsed time of requests in milliseconds | Integer | 1 |
| requests.averageTime | Total elapsed time of requests in milliseconds | Integer | 1 |
| requests.maxTime | Longest elapsed time of all requests in milliseconds | Integer | 1 |
| clients | Statistics about concurrent clients | Structure | 1 |
| clients.currentConcurrentClients | The current number of concurrent clients in read time. Note that the current request to fetch this information is not included in the count. | Integer | 1 |
| clients.maxConcurrentClients | The maximum number of concurrent clients | Integer | 1 |
| status | Server status either `"LOADING"` or `"READY"` | String | 1 |
| loadDate | Date of memory cache update. This date is set at start time of the server or when a reload is requested. | Date | 1 |
| abortedDomains | Array of aborted domains. An aborted domain is a domain for which loading has failed. | Array | 1 |
| abortedDomains[] | An aborted domain | Structure | 0..1 |
| abortedDomains[].name | Domain name | String | 1 |
| abortedDomains[].loadingDate | Date of loading start when the loading has failed | String | 1 |
| license.status | License status regarding access to Central Bases and measurement base (see table below) | String | 1 |
| license.href | Auto reference to license detail | URI | 1 |
| license.expirationDate | When license key expires (present only in new license key) | Date | 1 |
| license.endOfGraceDate | 30 days grace after license key expiration date (present only in new license key) | Date | 1 |
| license.usersCount | Number of users connected with this license key (present only in new license key) | Integer | 1 |
| license.usersExcess | Number of users count exceeded after the limit (present only in new license key) | Integer | 1 |
| license.allowedProducts | List all allowed products with this license key (present only in new license key) | Array | 1 |
| license.hasNewLicenseKeyType | This tells whether we are using old or new license key type | Boolean | 1 |
| domainsLocations | Get data source name and schema name for each domain. The ADMINISTRATOR role is required. | Structure | 0..1 |
| recommendedDbVersion | The preferred version of AIP (for compliancy with database schema) | | |
| securityMode | Configuration value of property `security.mode` from `application.properties`: `"default"` (authentication based on configuration files), `"ldap"` (authentication based on LDAP protocol), `"saml"` (authentication based on SAML protocol) | String | 1 |
| samlSingleLogout | Configuration value of `security.saml.single.logout` from `application.properties`: `true` (logout action enabled for "saml" mode), `false` (logout action disabled for "saml" mode) | Boolean | 1 |
| languages | Installed translations | Array | 1 |
| languages[] | An available locale language | String | 0..1 |
| reportEnabled | Check whether the configuration variable `report.reportGenerator` is set | Boolean | 1 |

**License status values:**

| Use Case | Status | Description |
|---|---|---|
| No License | `NO_LICENSE_KEY` | License key was not found |
| | `INVALID_LICENSE_KEY` | License key is not valid |
| | `CANNOT_ACCESS_LICENSE_KEY` | License key file is not readable |
| | `INVALID_LICENSE_FILE` | Cannot find license key file |
| | `LICENSE_EXPIRED` | When license key expired |
| Restricted License | `RESTRICTED_LICENSE` | License is a restricted license |
| | `GLOBAL_ACCESS_TOKENS_EXCEEDED` | License is a restricted license, and quota of global access tokens is exceeded |
| | `UNIT_ACCESS_TOKENS_EXCEEDED` | License is a restricted license, and quota of unit access tokens is exceeded |
| Unrestricted License | `UNRESTRICTED_LICENSE` | License is an unrestricted license |

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
  "jiraEnabled": true,
  "domainsBinding": {
    "href": "server/domains-bindings",
    "name": "Domains/schemas bindings"
  },
  "authorizations": {
    "href": "server/authorizations",
    "name": "Authorized applications definitions per users"
  }
}
```

---

## Server Cache Management

> **Administrator role is required.**

REST server stores portfolio objects, configuration, and snapshots in a memory cache. This memory cache is loaded as soon as the REST Server is started.

An URL allows to reload all domains in memory cache. This action may be required when a new snapshot has been added, and can be performed from a command line with a tool such as "curl":

```bash
curl -u admin:cast -H "Accept: application/json" http://localhost:8080/rest/server/reload
```

For each domain, Lucene index files can be created to allow components or violations search.

Lucene index files are created at start time and reload time if these options are enabled:

```properties
# application.properties
# Rebuild Lucene components index on start if outdated (true or false)
rebuildComponentsSearchIndexesOnStart=true
# Rebuild Lucene violations index on start if outdated (true or false)
rebuildViolationsSearchIndexesOnStart=false
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/reload{?parameters}` | Sync clients with new domains. Reload the domains configuration and refresh the server memory cache. Example of use: A new application onboarding requires to create a new ED domain in the domains.properties file. The client may receive an HTTP Status 503 "Service not available" if a server/refresh call is already in progress. In case of intensive traffic, the service awaits for the end of current processing request to start. Effects: reload the configuration of domains (domains.properties file), reload the server memory cache, rebuild Lucene Indexes, respond a "503 Service unavailable" to any client during the reload, invalidate the browser cache. |
| GET | application/json | `server/refresh{?parameters}` | Sync clients with new snapshots. Refresh the server memory cache when a new snapshot has been added. This processing is made in the background with no interruption for HD clients, and reduced interruption for ED clients. Effects: reload the server memory cache, rebuild Lucene Indexes, respond a "503 Service unavailable" to ED clients requesting Lucene index data while it is under construction, invalidate the browser cache. **Note:** If two concurrent requests are sent for 2 different domains, those domains will be refreshed in parallel. **Note:** Onboarding of a new application must be performed with the "server/reload" or the "server/domains-bindings" web service. |
| GET | application/json | `server/reset` | Sync clients after a snapshot reconsolidation, a license key change, or a change of the license.xml file (authorizations in case of restricted license). The web service reloads the license.key file, the license.xml file and invalidates the browser cache. |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| domain | Specify a single domain to reload (for example to refresh a domain after a snapshot) | a string | `$all` |

---

## DBMS Warm-up Service

> **Administrator role is required.**

This service fetches data (results, components, violations) for domains hosted in central bases, in order to pre-load data in memory after a DBMS cold restart.

It avoids penalizing the first user fetching data.

This service loops on each domain hosted by a central base, and triggers some queries on components, violations and assessment results.

```bash
curl -u admin:cast http://localhost:8080/rest/server/warmup
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/warmup` | Warm up DBMS after a cold restart (central base hosts only) |

---

## Lucene Index

For each domain, a Lucene index is created to allow the search for components.

Another Lucene index can be created to allow the search for violations.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| PUT | application/json | `{Domain}/components-index` | Create or overwrite the components search index for a domain *(Admin only)* |
| GET | application/json | `{Domain}/components-index` | Get index status for a domain |
| PUT | application/json | `{Domain}/violations-index` | Create or overwrite the violations search index for a domain *(Admin only)* |
| GET | application/json | `{Domain}/violations-index` | Get index status for a domain |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| status | `upToDate` (index is up to date), `N/A` (not applicable, no index), or `toUpdate` (last snapshot is more recent than the index date, a rebuild is required) | String | 1 |
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

A Domain binding associates a domain name with a data source name and a schema name.

This resource is based on the use of the `domains.properties` file.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/domains-bindings` | Get all domains bindings *(Admin only)* |
| PUT | application/json | `server/domains-bindings` | Update the domains bindings, or create new domains bindings. A triplet (domain, data source, central) is replaced for an existing domain or added if new. This web service triggers the "reload" service for the domains listed in the payload. This web service overwrites the `domains.properties` file. *(Admin only)* |
| DELETE | application/json | `server/domains-bindings` | Remove domains. Overwrites the `domains.properties` file. *(Admin only)* |

#### ⚠️ Warning for PUT Operation

This service accepts the "Authorization" HTTP header to transmit user's credentials, so that a prior call to the login "service" is not required. Thus, we can start the Web Server with an empty list of domains, and bypass the "login" service that prevents connection when no domain is defined.
In case of exception when writing this file, an HTTP Status "403 Forbidden" is returned. Check the permissions of this file.

Assuming there are two existing domains AED1, AED2, add a new domain:
```json
[
	{
		"name": "AED3",
		"dataSource": "DEV_CSS2",
		"schema": "appli1_central"
	}
]
```

Assuming there are three existing domains AED1, AED2, AED3, change schemas for domains AED1, AED2:
```json
[
	{
		"name": "AED1",
		"dataSource": "DEV_CSS2",
		"schema": "appliA_central"
	},
	{
		"name": "AED2",
		"dataSource": "DEV_CSS2",
		"schema": "appliB_central"
	}
]
```

#### ⚠️ Warning for DELETE Operation

This service accepts the **`Authorization`** HTTP header to transmit the user’s credentials, so a prior call to the login *service* is not required.  
Thus, we can start the Web Server with an empty list of domains and bypass the *login* service that prevents connection when no domain is defined.

If an exception occurs while writing this file, an HTTP status **`403 Forbidden`** is returned.  
Check the permissions of this file.

Assuming there are two existing domains **AED1** and **AED2**, to remove domain **AED2**:

```json
[
  {
    "name": "AED2"
  }
]
```

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

This resource is based on the use of the `profiles` table.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/profiles?{parameters}` | Get all users' profiles *(Admin only)* |
| PUT | application/json | `server/profiles` | Update users' profiles. Create or replace profiles *(Admin only)* |
| DELETE | application/json | `server/profiles` | Delete users' profiles *(Admin only)* |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |
| prefix | Specify the starting letters of the user name or group name. For LDAP 3 letters are required. | a text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| profileName.name | Profile name | String | 1 |
| authorizations | All authorizations for a user | Array | 0..1 |
| authorizations[] | An authorization defines which applications can be accessed | Structure | 1 |
| authorizations[].allApplications | Set applications access to all applications | Boolean | 0..1 |
| authorizations[].application / authorizations[].adgDatabase | Allow access to an application matching application name and adgDatabase name | String | 0..1 |
| authorizations[].applicationPattern / authorizations[].adgDatabasePattern | Allow access to all applications matching applicationPattern regular expression and adgDatabasePattern regular expression | String | 0..1 |
| authorizations[].tag / authorizations[].category / authorizations[].restrictions | Allow access to all applications matching a tag of a category | String | 0..1 |
| authorizations[].technology / authorizations[].restrictions | Allow access to all applications matching a technology | String | 0..1 |
| authorizations[].restrictions | All restrictions, applicable for authorizations defined with tag, category and technology attributes | Array | 0..1 |
| authorizations[].restrictions[] | A restriction | Structure | 0..1 |
| authorizations[].restrictions[].application / authorizations[].restrictions[].adgDatabase | Deny access to an application matching application name and adgDatabase name | String | 0..1 |
| authorizations[].restrictions[].applicationPattern / authorizations[].restrictions[].adgDatabasePattern | Deny access to all applications matching applicationPattern regular expression and adgDatabasePattern regular expression | String | 0..1 |
| authorizations[].restrictions[].tag / authorizations[].restrictions[].category | Deny access to all applications matching a tag of a category | String | 0..1 |
| authorizations[].restrictions[].technology | Deny access to all applications matching a technology | String | 0..1 |
| roles | All roles for a user | Array | 0..1 |
| roles[] | Defines all roles available for user | Structure | 1 |
| roles[].key | Specify which role key | String | 1 |

### JSON Example

```json
[
  {
    "profileName": { "name": "GroupWith2Apps_group_profile" },
    "authorizations": [
      { "application": "Billing platforms", "adgDatabase": "demo_709_central", "restrictions": [] },
      { "application": "Dream Team", "adgDatabase": "adg_contrex_central", "restrictions": [] },
      { "application": "TransactionNet", "adgDatabase": "ice_800_central", "restrictions": [] }
    ],
    "roles": [QUALITY_MANAGER]
  }
]
```

---

## User Profiles

This resource is based on the use of the `user_profiles` table.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/user-profiles?{parameters}` | Get all users along with profiles assigned to each user *(Admin only)* |
| PUT | application/json | `server/user-profiles` | Update users' profiles. Create or replace profiles for users or groups. **WARNING:** there is no checking of user/group names validity. *(Admin only)* |
| DELETE | application/json | `server/user-profiles` | Delete users' profiles *(Admin only)* |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |
| prefix | Specify the starting letters of the user name or group name. For LDAP 3 letters are required. | a text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| user | User name. Excludes the `group` property. | String | 1 |
| group | LDAP group name. Excludes the `user` property. | String | 1 |
| profiles | All profiles for a user | Array | 0..1 |
| profiles[] | Defines all profiles available for user | Structure | 1 |
| profiles[].name | Specify which profile name | String | 1 |

### JSON Example

```json
[
  {
    "profiles": [{ "name": "admin_profile" }],
    "user": "admin"
  },
  {
    "profiles": [
      { "name": "ATLAS_user_profile" },
      { "name": "CIO_user_profile" }
    ],
    "user": "QualityManNoRightsOnCastOldCode"
  }
]
```

---

## License Key

> **This resource is disabled in INTEGRATED security mode. Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/license-key` | Get license key along with its status in dashboard *(Admin only)* |
| PUT | application/json | `server/license-key` | Update license key only when key is valid *(Admin only)* |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference to license detail | URI | 1 |
| name | "License" | String | 1 |
| key | Loaded valid license key | String | 1 |
| status | License status (see Server section for values) | String | 1 |
| expirationDate | When license key expires (present only in new license key) | Date | 1 |
| endOfGraceDate | 30 days grace after license key expiration date (present only in new license key) | Date | 1 |
| hasLicenseExpired | Specify whether key has expired or not (present only in new license key) | Boolean | 1 |
| usersMax | Maximum users allowed with this key (present only in new license key) | Integer | 1 |
| usersExcess | Number of users count exceeded after the limit (present only in new license key) | Integer | 1 |
| usersCount | Number of users connected with this license key (present only in new license key) | Integer | 1 |
| isLicenseExpiring | Tells whether license key is expiring within 30 days (present only in new license key) | Boolean | 1 |
| allowedProducts | List all allowed products with this license key (present only in new license key) | Array | 1 |
| users | Auto reference to license user detail (present only in new license key) | URI | 1 |

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
  "users": {
    "href": "server/license/users",
    "name": "Registered users"
  },
  "isLicenseExpiring": false,
  "allowedProducts": ["P2", "P3", "P4"]
}
```

---

## License User Details

> **This resource works only for new license key type and is disabled in INTEGRATED security mode. Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/license/users?{parameters}` | Get list of connected users *(Admin only)* |
| DELETE | application/json | `server/license/users` | Remove users from list *(Admin only)* |

### Query Parameters

| Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |
| prefix | Specify the starting letters of the user name or group name. For LDAP 3 letters are required. | a text | N/A |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| userId | User name | String | 1 |
| firstLoginDate | First login date with this userId | Date | 1 |
| lastLoginDate | Last login date with this userId | Date | 1 |

### JSON Example

```json
[
  {
    "userId": "admin",
    "firstLoginDate": { "time": 1637605800000, "isoDate": "2021-11-23" },
    "lastLoginDate": { "time": 1637605800000, "isoDate": "2021-11-23" }
  }
]
```

---

## Roles definition

This resource returns the definition for each role available in the dashboard.

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
  {
    "key": "ADMIN",
    "name": "Admin",
    "description": "The Admin has rights to access all applications and his role provides permission to execute reload, reset and refresh dashboard memory, create, update and delete categories and tags."
  },
  {
    "key": "QUALITY_MANAGER",
    "name": "Quality Manager",
    "description": "The Quality manager role provides permission to add and remove objects from the Action Plan and to use the Engineering Dashboard - Action Plan Recommendation feature."
  },
  {
    "key": "QUALITY_AUTOMATION_MANAGER",
    "name": "Quality Automation Manager",
    "description": "The Quality automation manager role provides permission to add and remove objects from the Education list."
  },
  {
    "key": "EXCLUSION_MANAGER",
    "name": "Exclusion Manager",
    "description": "The Exclusion manager role provides permission to add and remove objects from the Exclusion list."
  },
  {
    "key": "CODE_RESTRICTED",
    "name": "Code Restricted",
    "description": "The Code restricted role prevents users from viewing source code in the Engineering Dashboard."
  }
]
```

---

## All applications

This resource returns all applications available across all domains from the REST API cache.

> **Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/all-applications` | Get all applications available across all domains *(Admin only)* |

*For JSON representation and JSON example, see [Application Structure Resources - 2.11 — Application](https://doc.castsoftware.com/export/DASHBOARDS/Application+Structure+Resources+-+2.11.html#ApplicationStructureResources2.11-Application).*

---

## All technologies

This resource returns all technologies available across all domains from the REST API cache.

> **Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/all-technologies` | Get all technologies available across all domains *(Admin only)* |

### JSON Representation

This web service returns a simple list from the REST API cache.

### JSON Example

```json
[HTML5, JEE, SQL, JAVA]
```

---

## Users-Groups

This resource returns a list of users and groups based on security mode. This resource is not available in **INTEGRATED** security mode.

> **Administrator role is required.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `server/users-groups?{parameters}` | Get users and groups list for the default security mode, and LDAP/SAML when LDAP is configured *(Admin only)* |

**Query Parameters**

| Parameter | Description | Values | Default value |
|---|---|---|---|
| prefix | Specify the starting letters of the user name or group name. For LDAP 3 letters are required. | a text | N/A |
| from-table | To fetch result from local table in LDAP/SAML mode | Boolean | FALSE |

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

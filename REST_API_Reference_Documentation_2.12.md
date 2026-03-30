# REST API Reference Documentation - 2.12

---

## On this page

- [Scope](#scope)
- [Discovering the REST API](#discovering-the-rest-api)
  - [WAR archive](#war-archive)
  - [ZIP archive](#zip-archive)
- [Uniform Resource Identifiers](#uniform-resource-identifiers)
- [URI Templates](#uri-templates)
- [Request Patterns](#request-patterns)
- [Resource Representation](#resource-representation)
  - [Media Types](#media-types)
  - [JSON Representation](#json-representation)
  - [Collections](#collections)
- [Configuration](#configuration)
  - [Configuration files](#configuration-files)
    - [Data Source/Domain](#data-sourcedomain)
  - [Security](#security)
  - [License](#license)
- [HTTP Protocol](#http-protocol)
  - [Authentication](#authentication)
    - [Login](#login)
    - [Logout](#logout)
    - [Test](#test)
  - [Client Cache Management](#client-cache-management)
  - [API key](#api-key)
  - [X-Client HTTP header](#x-client-http-header)
  - [Query Parameters Encoding](#query-parameters-encoding)
  - [Errors](#errors)

---

## Resources and Services

- [Application Structure Resources - 2.11](Application_Structure_Resources_2.12.md)
- [Engineering Resources - 2.11](Engineering_Resources_2.11.md)
- [Health Results Resources - 2.11](Health_Results_Resources_2.11.md)
- [Quality and Sizing Model Resources - 2.11](Quality_and_Sizing+Model_Resources_2.11.md)
- [Report Service - 2.12](Report_Service_2.12.md)
- [Server Services - 2.12](Server_Services_2.12.md)
- [User Session Services - 2.11](User_Session_Services_2.11.md)

**Target audience:**

- CAST AI Administrators
- Consumers of information via the REST API

---

## Scope

This REST API is provided:

- either for data stored in a **Measurement Database** for a Health Dashboard (**HD**)
- or for data stored in a **Central database** for an Engineering Dashboard (**ED**)

---

## Discovering the REST API

A simple Web Application, called "Simple REST Client", is available to discover the REST API. This Web application allows you to test REST URIs, check JSON response, and use the "preview" form to navigate a resource to linked resources.

### WAR archive

If you use the war archive of Dashboards/REST API (e.g. `com.castsoftware.aip.dashboard.2.0.0-funcrel.war`), this web application can be accessed via the following URL:

```
http://localhost:8080/com.castsoftware.aip.dashboard.2.0.0-funcrel/static/default.html
```

The "Server" text box in the "Simple REST Client" must contain: `/com.castsoftware.aip.dashboard.2.0.0-funcrel/rest/`

### ZIP archive

If you use the zip archive of Dashboards/REST API (e.g. `com.castsoftware.aip.dashboard.2.0.0-funcrel.zip`), this web application can be accessed via the following URL:

```
http://localhost:8080/static/default.html
```

The "Server" text box in the "Simple REST Client" must contain: `/rest/`

---

## Uniform Resource Identifiers

All URIs are relative to a domain of a REST Server. Therefore, a REST client must always concatenate an URI with the URI of the REST Server.

For example, if the REST Server URL is:

```
http://localhost:9090/Dashboard-WebService
```

and the URI is `AAD/applications`, then the REST server is invoked with the following URL:

```
http://localhost:9090/Dashboard-WebService/rest/AAD/applications
```

---

## URI Templates

URI templates notation is specified below (see also <http://tools.ietf.org/html/rfc6570>):

- **`{string}` notation:** this syntax denotes a string expansion.
- **`{?parameters}` notation:** this syntax denotes an optional parameter to expand. The URL template is expanded as follows:
  - append `?` to the result string if this is the first defined value, or append `&`
  - append the parameter name
  - append `=`
  - append a parameter value which is a list of items, with `,` as a separator (brackets separators are optional)

---

## Request Patterns

| Requests | Content-Type: application/json | Content-Type: text/csv | Comment |
|---|---|---|---|
| `GET .../{resources}/{ID}` | Yes | No | Report a full representation of a resource |
| `GET ../{resources}?{filters}` | Yes | Yes | Report a collection of resources. Each item is a short representation of a resource |
| `GET ../{ressources}-summary` | Yes | No | Report resources counters |
| `POST ../{resources}` | Yes | No | Insert a collection of resources. This action is not idempotent, each call will insert new items |
| `DELETE ../{resources}` | Yes | No | Remove a collection of resources or items |
| `PUT ../{resources}` | Yes | Yes | Update a collection of resources. This action is idempotent, a second call will not change anything. In CSV mode, replace a collection of resources |

POST, PUT, DELETE payloads are always a collection of resources. Note that a resource is an item with an `href` attribute.

---

## Resource Representation

### Media Types

This REST API supports the following media types:

| Media Type | Resource Representation |
|---|---|
| `application/json` | A JSON text (for all resources) |
| `text/plain` | A plain text (for file contents). Note this will only list the first 10 violations. |
| `text/csv` | A Comma Separated Value text. Separator is `;`. Decimal separator is `.`. Empty column is represented with `null` value. |
| `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` | An Excel document with `.xlsx` extension |

### JSON Representation

A JSON representation is a structure compliant with JSON format. Each property can be of type:

- **Array** – a collection of items
- **String**
- **Integer** (JSON number)
- **Double** (JSON number)
- **Number**
- **Structure** – a nested structure
- **URI** – a resource reference
- **Date** – JSON structure with:
  - `time` attribute: date as a number of milliseconds since 1970/01/01
  - `isoDate` attribute: date as an international date format `YYYY-MM-DD`

A hyperlink is denoted with 2 properties:

- **`href`**: a URI to the resource
- **`name`**: a name of the hyperlink

Each property specifies value occurrences:

- **`0..1`**: an optional item
- **`1`**: a mandatory item
- **`0..*`**: an array of items or an empty array
- **`1..*`**: an array of items

### Collections

Some URIs refer to a collection of resources. A resource can itself refer to a collection of strings (see technologies).

Collections are ordered according to the items:

| Collections | Order |
|---|---|
| List of items with names | Alphabetic order of names |
| List of snapshots items | Numerical order of snapshots, from the most recent to the least recent |
| List of configuration items | Alphabetic order of keys |

---

## Configuration

REST API relies on Spring Boot configuration files and own configuration files.

### Configuration files

All configuration files settings are described in documentation related to AAD which embeds REST API.

| File | WAR location | ZIP location | Description |
|---|---|---|---|
| `application.properties` | `WEB-INF/classes` | zip root | Spring Boot application main configuration file. Contains: global variables, Tomcat configuration, data source configuration, security configuration (authentication modes, LDAP, SAML, Api Key, SSL), Jira configuration, Report Generator configuration |
| `authorizations.xml` | | | Authorizations applied for Measurement Service / Dashboard Service schemas |
| `domains.properties` | | | REST API Domain configuration, linked to data sources and a schema |
| `license.key` | | | A license key required to access Dashboard Services schemas |
| `license.xml` | | | Authorizations applied for Dashboard Services schema access in case of a restricted license |
| `log4j2-spring.xml` | | | Log settings |
| `roles.xml` | | | Assign a role to a user or group ("default authentication" or LDAP mode). Available roles: `ADMIN`, `QUALITY_MANAGER` (Action Plan - AED), `EXCLUSION_MANAGER` (Violations Exclusion - AED) |
| `user.properties` | | | Manage "default authentication" mode users/groups |

#### Data Source/Domain

Several domains can be defined on each RDBMS Data Source.

The following names are reserved and **cannot** be used as a domain name:

- `ping` *(deprecated, see `user/ping`)*
- `key` *(deprecated, see `server/key`)*
- `login` *(deprecated, see `user/login`)*
- `logout` *(deprecated, see `user/logout`)*
- `reload` *(deprecated, see `server/reload`)*
- `server`
- `user`

### Security

For GET actions, REST API filters data according to the user authorizations (see Dashboards documentation for more details on the configuration):

- Results, list of applications, list of modules, list of technologies, list of tags are filtered
- Domain resources and System resources are not filtered, even if the user does not have authorization on any application of this resource
- Categories are not filtered (as they do not depend on applications)
- Requesting a non-authorized application returns an **HTTP 403** exception (Forbidden)
- Requesting a resource on a non-authorized application (such as a module or snapshot) returns an **HTTP 403** status (Forbidden)

PUT, DELETE, UPDATE actions on tags and categories require an **ADMIN** privilege, otherwise HTTP 403 is returned.

If authorizations are changed, memory cache must be reloaded to impact connected users.

The following URLs require **ADMIN** privilege, otherwise HTTP 403 is returned:

- `/server/reload`
- `/server/key`

### License

A license key is required to get resources on a central domain.

In the case of a restricted license, the `license.xml` file is applied for authorizations.

There are 2 exceptions — the following URLs depend on `authorizations.xml` settings:

- `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions`
- `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions-evolution`

---

## HTTP Protocol

### Authentication

Authentication is based on basic authentication on the server side.

#### Login

Prior to any request, the REST client must authenticate on behalf of the current end-user using the `login` request. This request must contain an HTTP header with the credentials `UserName:Password` encoded in base 64.

```http
GET /.../rest/login HTTP/1.1
Authorization: Basic Y2FzdDpjYXN0
```

- If credentials are valid, the server replies: **HTTP/1.1 200 OK**
- If credentials are invalid, the server replies: **HTTP/1.1 401 Unauthorized**

> **Note:** a `Set-Cookie` HTTP header is sent back from the server in the first server response.

#### Logout

The following request closes the current session and replies with **HTTP/1.1 401 Unauthorized**:

```http
GET /.../rest/logout HTTP/1.1
```

#### Test

To test if the current client can access the server, use the `ping` request:

```http
GET /.../rest/user/ping HTTP/1.1
```

---

### Client Cache Management

In order to benefit from navigator response cache on the client side, each response is replied with an `ETag` directive header using the cache loading date.

**Example:**

```
ETag: "application/json, text/javascript, */*; q=0.01:Mon Aug 19 17:04:16 CEST 2013"
```

> **Note:** No cache control directive is sent to the client.

Upon a request with the directive `If-None-Match`, the server compares the provided date with the cache loading date. If they are equal, the server replies with HTTP status 304:

```
HTTP/1.1 304 Not Modified
```

---

### API key

The API key is an alternative to the Basic Authentication mechanism.

A key is a secret text (it can be a unique random string) stored in the `application.properties` file:

```properties
security.apikey=22b56696-7d59-11e9-8f9e-2a86e4085a59
```

Any client can use this key to authenticate any user, with a request containing two HTTP headers:

- `X-API-KEY`: a key matching the one in `application.properties`
- `X-API-USER`: a user name to get a role and authorizations

**Example:**

```bash
curl -k https://localhost:8080/Dashboard-WebService/rest/server/reload \
  -H "X-API-KEY: 22b56696-7d59-11e9-8f9e-2a86e4085a59" \
  -H "X-API-USER: admin" -v
```

The API KEY is required to bypass the SAML Single-Sign On protocol for non-browser clients, and for the Report Generator executed in standalone or integrated mode.

> **Note:** The user is granted the `ROLE_ADMIN` role when the API KEY is used.

---

### X-Client HTTP header

This header can be set by any REST API client in order to identify the effective client in the audit trail.

**Example:**

```bash
curl -k https://localhost:8080/Dashboard-WebService/rest/server \
  -H "X-Client: CURL" \
  -H "X-API-KEY: 22b56696-7d59-11e9-8f9e-2a86e4085a59" \
  -H "X-API-USER: admin" -v
```

---

### Query Parameters Encoding

Query parameters must be encoded, especially if they contain special characters such as: space, `#`, `+`, etc.

For example, the following URL:

```
http://localhost:8080/Dashboard-WebService/rest/AAD/results?technologies=(C++)
```

must be encoded as:

```
http://localhost:8080/Dashboard-WebService/rest/AAD/results?technologies=(C%2B%2B)
```

> **Note:** The Simple REST Client (`default.html`) encodes parameters automatically.

---

### Errors

| HTTP Status | Code | Example of messages | Circumstances |
|---|---|---|---|
| **400 Bad Request** | 1 | `Cannot process this request` | URI not recognized, or inappropriate/unsafe query parameter value |
| | 2 | `Incorrect parameter` / `This request may return too many items.` | Unexpected parameter value (e.g. `nbRows=-1`) |
| | 3 | `Resource not implemented for this domain` | DBMS schema is too old; an upgrade is required |
| | 4 | `Invalid content` | PUT/POST/DELETE: JSON syntax error, incorrect property, missing property |
| **403 Forbidden** | 1 | `You are not allowed to access this data.` | User has no access according to `authorizations.xml` or `license.xml` |
| **404 Not Found** | 1 | `Resource not found` | URI contains IDs matching no resource |
| **406 Not Acceptable** | 1 | `The requested media type is not appropriate for this resource` | `Accept` HTTP header does not match any expected content type |
| **500 Internal Server Error** | 1 | Run-time exception cause and call stack | Unexpected exception |
| | 20 | `{ "cause": "...", ... }` | Report Generator generation has failed |
| **503 Service Unavailable** | 1 | `Server status: LOADING` | Domains are being loaded into memory cache |
| | 2 | `DBMS connections has failed or all domains loading have been aborted` | DBMS not started, resources unavailable, or datasource misconfigured |
| | 20 | `{ "cause": "...", ... }` | Report Generator configuration settings is not correct |

**JSON error response example:**

```json
{"code": 1, "message": "Cannot process this request"}
```

> **Note:** Unrecognized parameters in a URL (e.g. `aplications` instead of `applications`) will be silently ignored. The remainder of the URL (if correct) will return results without an error code.

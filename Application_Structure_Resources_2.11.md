# Application Structure Resources - 2.11

*Created by N Padmavathi on Jan 31, 2023*

---

## On this page

- [Glossary](#glossary)
- [Domain](#domain)
- [System](#system)
- [Application](#application)
- [Module](#module)
- [Application Snapshot](#application-snapshot)
- [Module Snapshot](#module-snapshot)
- [Attachments](#attachments)
- [Application to Tags Assignments](#application-to-tags-assignments)
- [Category](#category)

---

## Glossary

| Term | Definition |
|---|---|
| Application | An **application** represents an application as a measurable object. Each application snapshot is a measure point of an application. |
| Application Snapshot | An **application snapshot** is a measure point of an application. It is made up of a set of data (results, objects, etc.) computed and frozen at a given time. |
| Attachment | A JSON content attached to an application; it can be a list of frameworks, a survey, etc. |
| Category | A tag belongs to a **category**. A category is a set of tags. |
| Domain | A **domain** is an identifier of a database. It is defined with a schema name and a connection string to a DBMS. |
| Module | A **module** represents a module as a measurable object. Each snapshot of a module is part of an application snapshot. |
| Module Snapshot | A **module snapshot** is a measure point of a module. It is made up of a set of data computed and frozen at a given time. A module snapshot is a part of an application snapshot. |
| System | A **system** represents a group of applications. |
| Tag | A **tag** is a keyword that can be assigned to an application. |
| Technology | A subset of source code belonging to a **technology** (e.g. JEE code, .NET code, ABAP code, etc.) |
| Tree Node | A structure that organizes application, modules, and components in a tree. |

---

## Domain

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `/` | Array of domains |
| GET | application/json | `{Domain}` | A domain content |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Domain name | String | 1 |
| dbType | Database type: `"AAD"` for Measurement Service or `"ADG"` for Dashboard Service | String | 1 |
| version | Package version of `ADG_FULL_CENTRAL` (CAST AIP version number) | String | 1 |
| schema | Name of the database schema | String | 1 |
| dbmsVersion | Product name and version of the DBMS host | String | 1 |
| componentsIndexStatus | Status of the Lucene components index | String | 1 |
| violationsIndexStatus | Status of the Lucene violations index | String | 1 |
| systems.href | For a Central database, reference to get an array of systems | URI | 1 |
| applications.href | Reference to get an array of applications | URI | 1 |
| nbOfApplications | Total applications count in domain | int | 1 |
| nbOfAuthorizedApplications | Authorized applications count (only for domain content `/{domainName}`) | int | 1 |
| nbOfAuthorizedSnapshots | Authorized snapshots count (only for domain content `/{domainName}`) | int | 1 |
| configurations.href | Reference to the history of configurations | URI | 1 |
| results.href | Reference to get results of all applications | URI | 1 |
| commonCategories.href | For a Measurement Database, reference to get all defined categories | URI | 1 |
| tags.href | For a Measurement Database, reference to get all application assignments | URI | 1 |
| technologyResults | Array of references to get results for each technology | Array | 1 |
| technologyResults.href | For each technology, reference to get results | URI | 1 |

### JSON Example

**GET DEMO — Array of domains**

```json
{
  "href": "ENDTOEND833",
  "name": "ENDTOEND833",
  "dbType": "ADG",
  "version": "8.3.3",
  "schema": "endtoend_833_central",
  "dbmsVersion": "PostgreSQL 9.2.3, compiled by Visual C++ build 1600, 64-bit",
  "componentsIndexStatus": "upToDate",
  "violationsIndexStatus": "toUpdate",
  "systems": {
    "href": "ENDTOEND833/systems",
    "name": "All Systems. A system represents a portfolio of applications."
  },
  "applications": {
    "href": "ENDTOEND833/applications",
    "name": "All Applications. An application represents ..."
  },
  "nbOfApplications": 5,
  "configurations": {
    "href": "ENDTOEND833/configuration/snapshots",
    "name": "All configuration snapshots ..."
  },
  "results": {
    "href": "ENDTOEND833/results",
    "name": "Results by snapshots, by systems, by ..."
  },
  "commonCategories": null,
  "tags": null
}
```

**GET DEMO — Domain content (with authorized counts)**

```json
{
  "href": "ENDTOEND833",
  "name": "ENDTOEND833",
  "dbType": "ADG",
  "version": "8.3.3",
  "schema": "endtoend_833_central",
  "dbmsVersion": "PostgreSQL 9.2.3, compiled by Visual C++ build 1600, 64-bit",
  "componentsIndexStatus": "upToDate",
  "violationsIndexStatus": "toUpdate",
  "nbOfApplications": 5,
  "nbOfAuthorizedApplications": 2,
  "nbOfAuthorizedSnapshots": 4,
  "technologyResults": [
    {
      "href": "ENDTOEND833/technologies-results?name=ABAP",
      "name": "Filtered results for ENDTOEND833 by technology ABAP"
    },
    {
      "href": "ENDTOEND833/technologies-results?name=CICS",
      "name": "Filtered results for ENDTOEND833 by technology CICS"
    }
  ]
}
```

---

## System

> **Central Database only**

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | System name | String | 1 |
| technologies | Array of technologies (union of all applications) | Array | 1 |
| technologies[] | A technology | String | 1..* |
| applications.href | Reference to get an array of applications | URI | 1 |
| results.href | Reference to get an array of result items | URI | 1 |

### JSON Example

```json
{
  "href": "DEMO/systems/2",
  "name": "BT",
  "technologies": ["FLEX", "JEE", "Oracle Server", "PL/SQL"],
  "applications": {
    "href": "DEMO/systems/2/applications",
    "name": "Applications of BT"
  },
  "results": {
    "href": "DEMO/systems/2/results",
    "name": "results breakdown by applications, by snapshots, by technologies."
  }
}
```

---

## Application

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications` | Array of applications of a domain |
| GET | application/json | `{Domain}/applications/{ApplicationID}` | An application content |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Application name | String | 1 |
| technologies | Array of technologies used by the application | Array | 1 |
| technologies[] | A technology | String | 1..* |
| system.href | For a Central database, reference to the system owner | URI | 1 |
| modules.href | For a Central database, reference to get an array of modules | URI | 1 |
| snapshots.href | Reference to get an array of application snapshots | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| origin.href | For a Measurement Database, reference to the origin application in Central Database | URI | 1 |
| attachments.href | Reference to get an array of attachments | URI | 1 |
| adgDatabase | For a Measurement Database, name of the Central database hosting this application | String | 0..1 |
| adgWebSite | For a Measurement Database, full http address to AED web site | String | 0..1 |
| adgLocalId | For a Measurement Database, local ID defined on the adgWebSite database | String | 0..1 |
| adgVersion | For a Measurement Database, AED web site version | String | 0..1 |
| technologyResults | Array of references to get results for each technology | Array | 1 |
| technologyResults.href | For each technology, reference to get results | URI | 1 |

### JSON Example

**GET DEMO/applications/6**

```json
{
  "href": "DEMO/applications/6",
  "name": "Application 006",
  "technologies": ["Cobol", "DB2 Server", "JCL"],
  "systems": null,
  "modules": null,
  "snapshots": {
    "href": "DEMO/applications/6/snapshots",
    "name": "Snapshots of Application 006"
  },
  "results": null,
  "adgDatabase": "demo_710_central",
  "adgWebSite": "http://10.75.225.94:8080/GM_DRAS/",
  "adgLocalId": "3",
  "adgVersion": "7.1.0",
  "technologyResults": [
    {
      "href": "DEMO/applications/6/technologies-results?name=JCL",
      "name": "Filtered results for DEMO by technology JCL"
    }
  ]
}
```

---

## Module

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Module name | String | 1 |
| technologies | Array of technologies used by this module | Array | 1 |
| technologies[] | A technology | String | 1..* |
| applications.href | Reference to get the application owner *(note: misspelled with an 's')* | URI | 1 |
| snapshots.href | Reference to get an array of application snapshots | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| technologyResults | Array of references to get results for each technology | Array | 1 |
| technologyResults.href | For each technology, reference to get results | URI | 1 |

### JSON Example

```json
{
  "href": "DEMO/modules/5",
  "name": "Java",
  "applications": {
    "href": "DEMO/modules/5/application",
    "name": "DEMO"
  },
  "technologies": ["JEE"],
  "snapshots": {
    "href": "DEMO/modules/5/snapshots",
    "name": "Snapshots of Java"
  },
  "results": {
    "href": "DEMO/modules/5/results",
    "name": "results breakdown by snapshots, by technologies."
  },
  "technologyResults": [
    {
      "href": "DEMO/modules/5/technologies-results?name=JEE",
      "name": "Filtered results for Java by technology JEE"
    }
  ]
}
```

---

## Application Snapshot

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots` | Array of snapshots of an application |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}` | An application snapshot content |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Application name | String | 1 |
| number | Snapshot order number (ordered by `annotation.date`) | Integer | 1 |
| technologies | Array of technologies | Array | 1 |
| technologies[] | A technology | String | 1..* |
| annotation | User annotations describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | `"EFP"` or `"AEP"` | String | 1 |
| configurationSnapshot.href | Reference to the snapshot configuration | URI | 1 |
| system.href | For a Central database, reference to the system owner | URI | 1 |
| application.href | Reference to get the application owner | URI | 1 |
| modulesSnapshots.href | For an ADG database, reference to get an array of module snapshots | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| actionPlan.issues.href | Array of issues for this application snapshot | URI | 1 |
| actionPlan.summary.href | Summary of all issues | URI | 1 |
| activeExclusions.exclusions.href | Array of excluded violations | URI | 1 |
| activeExclusions.summary.href | Summary of excluded violations | URI | 1 |
| scheduledExclusions.exclusions.href | Array of scheduled exclusions | URI | 1 |
| scheduledExclusions.summary.href | Summary of scheduled exclusions | URI | 1 |
| components.href | For a Central database, reference to components with a PRI | URI | 1 |
| transactions.href | For a Central database, reference to transactions | URI | 1 |
| violations.href | For a Central database, reference to violations | URI | 1 |
| treeNode.href | For a Central database, reference to this snapshot's tree-node (null if not last snapshot) | URI | 1 |
| origin.href | For a Measurement Database, reference to origin snapshot in Central Database | URI | 1 |
| adgLocalId | For a Measurement Database, local ID for this snapshot | String | 0..1 |
| technologyResults | Array of references to get results for each technology | Array | 1 |
| technologyResults.href | For each technology, reference to get results | URI | 1 |

### JSON Example

**GET DEMO/applications/3/snapshots/8**

```json
{
  "href": "DEMO/applications/3/snapshots/8",
  "name": "DEMO",
  "number": 2,
  "technologies": ["FLEX", "JEE", "Oracle Server", "PL/SQL"],
  "annotation": {
    "version": "V2.0",
    "date": { "time": 1336514400000 },
    "description": "",
    "name": "Computed on 201205090914",
    "enhancementMeasure": "EFP"
  },
  "configurationSnapshot": {
    "href": "DEMO/configuration/snapshots/8",
    "name": "Configuration Snapshot #2"
  },
  "application": {
    "href": "DEMO/applications/3",
    "name": "DEMO"
  },
  "actionPlan": {
    "issues": {
      "href": "DEMO/applications/3/snapshots/8/action-plan/issues",
      "name": "Action Plan Issues"
    },
    "summary": {
      "href": "DEMO/applications/3/snapshots/8/action-plan/summary",
      "name": "Action plan summary for application DEMO in snapshot 2"
    }
  }
}
```

---

## Module Snapshot

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots` | Array of snapshots of a module |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}` | A module snapshot content |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/modules` | Array of snapshots for modules |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Module name | String | 1 |
| number | Snapshot order number | Integer | 1 |
| technologies | Array of technologies | Array | 1 |
| annotation | User annotations describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | `"EFP"` or `"AEP"` | String | 1 |
| configurationSnapshot.href | Reference to the snapshot configuration | URI | 1 |
| module.href | Reference to get the module owner | URI | 1 |
| applicationSnapshot.href | Reference to get the application snapshot owner | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| actionPlan.issues.href | Array of issues for this module snapshot | URI | 1 |
| actionPlan.summary.href | Summary of all issues | URI | 1 |
| components.href | For a Central database, reference to components with a PRI | URI | 1 |
| diagnosisResults.violations.href | For a Central database, reference to violations | URI | 1 |
| treeNode.href | Reference to this module tree-node (null if not last snapshot) | URI | 1 |
| technologyResults | Array of references to get results for each technology | Array | 1 |

---

## Attachments

### Description

A free JSON content can be attached to an application. Each JSON content is referred with a key.

> **Warning:** keys starting with `"CAST:"` are reserved for CAST AIP Product.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/attachments` | Get all references to JSON contents |
| GET | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Get a JSON content for a given key |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments` | Get all references to JSON contents for a snapshot |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Get a JSON content for a snapshot and key |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Map a JSON Content to an application and key |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Map a JSON Content to a snapshot and key |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Delete a JSON Content for an application and key |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Delete a JSON Content for a snapshot and key |

---

## Application to Tags Assignments

> **Measurement Database only**

### Description

A tag is used to filter a set of applications when results are requested. The REST API minimizes request/response flows by relying on client cache management (based on `304 NOT MODIFIED` responses).

#### Notions

- **Tag**: A keyword.
- **Category**: A tag belongs to a category. A category is a set of tags.
- **Application to Tags assignment**: A tag can be assigned to an application, but not to a specific snapshot.

#### Rules

1. Common categories, tags, and assignments are created for all users.
2. Technologies are similar to tags but are not tags.
3. When an application is not assigned to any tag of a category, the webapp considers it attached to the `"other"` tag for this category.
4. Tag labels cannot be translated.
5. Tags and categories labels can be changed.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/tags` | Get all application to tag assignments |
| PUT | application/json | `{Domain}/tags` | Replace application to tag assignments |

### JSON Representation

#### Response (GET)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| application.href | Application Reference | URI | 1 |
| technologies | Technologies | Array | 1 |
| technologies[] | A technology | String | 1..* |
| commonTags | Array of labels | Array | 1 |
| commonTags[].key | Tag key | String | 1 |
| commonTags[].label | Tag label | String | 1 |
| ownTags | Reserved | Array | 0..1 |

#### Payload (PUT)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| application.href | Application Reference | URI | 1 |
| commonTags | Array of tags | Array | 1 |
| commonTags[] | Tag | Structure | 0..* |
| commonTags[].key | A tag key | String | 1 |

---

## Category

> **Measurement Database only**

### Description

A regular category is a container of tags. A parent category is a container of other categories (tree structure).

Constraints:

- A category must define items (at least one tag or one sub-category)
- A category has a unique name
- A category cannot define both items and categories
- For a given category, a tag has a unique name
- Parent categories MUST NOT define circular relationships
- A category cannot be empty

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/common-categories` | Get all common categories definitions |
| PUT | application/json | `{Domain}/common-categories` | Update a collection of common categories |
| PUT | text/csv | `{Domain}/common-categories` | Bulk update of ALL common categories (create, update, remove) |
| POST | application/json | `{Domain}/common-categories` | Create a collection of common categories |
| DELETE | application/json | `{Domain}/common-categories` | Delete a collection of common categories |

### JSON Representations

| Properties | Response | POST Payload | DELETE Payload | PUT Payload |
|---|---|---|---|---|
| key | Unique global ID | N/A | Required ID of category to delete | Required ID of category to update |
| label | Category label | Required | N/A | Optional label update |
| tags | Array of tags | Required array | N/A | Full array (missing tags will be removed) |
| tags[].key | Unique global tag ID | N/A | N/A | Optional: if not set, tag will be added |
| tags[].label | Tag label | Required | N/A | Optional: if set, label update or new tag |
| categories | Array of sub-categories (parent only) | Required | N/A | Full array |
| categories[].key | Sub-category key | Optional | N/A | Required |
| categories[].label | Sub-category label | Optional | N/A | N/A |

### JSON Example

```json
[
  { "key": "26", "label": "Departments", "tags": [{ "key": "181", "label": "Dept1" }, { "key": "165", "label": "Dept2" }] },
  { "key": "4",  "label": "Countries", "tags": [{ "key": "3", "label": "France" }, { "key": "93", "label": "Germany" }] },
  { "key": "100", "label": "Location", "categories": [{ "key": "26", "label": "Departments" }, { "key": "4", "label": "Countries" }] }
]
```

### CSV Representation

| Column | Type | Description |
|---|---|---|
| Category Label | String | Category label |
| Category Key | String | Category key (empty for creation) |
| Item Type | String | `TAG` or `CATEGORY` |
| Item Label | String | Tag or sub-category label |
| Item Key | String | Tag or sub-category key (empty for new items) |

### CSV Example

```csv
Category Label;Category Key;Item Type;Item Label;Item Key
Team Size;;TAG;Small Size;
Team Size;;TAG;Large Size;
Region;;CATEGORY;West;
Region;;CATEGORY;East;
West;;CATEGORY;Europe;
West;;CATEGORY;U.S.;
East;;TAG;China;
East;;TAG;India;
Europe;;TAG;France;
Europe;;TAG;U.K.;
U.S.;;TAG;Texas;
U.S.;;TAG;New York;
```

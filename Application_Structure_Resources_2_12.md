# Application Structure Resources - 2.12

*Created by James Hurrell on Apr 4, 2024*

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
| Application Snapshot | An **application snapshot** is a measure point of an application. It is made up of a set of data (results, objects, etc.) computed and frozen at a given time |
| Attachment | A JSON content attached to an application, it can be a list of frameworks, a survey, etc. |
| Category | A tag belongs to a **category**. A category is a set of tags. |
| Domain | A **domain** is an identifier of a database. It is defined with a schema name and a connection string to a DBMS. |
| Module | A **module** represents a module as a measurable object. Each snapshot of a module is part of an application snapshot. |
| Module Snapshot | A **module snapshot** is a measure point of a module. It is made up of a set of data (results, objects, etc.) computed and frozen at a given time. A module snapshot is a part of an application snapshot. |
| System | A **system** represents a group of applications. |
| Tag | A **tag** is a keyword that can be assigned to an application. |
| Technology | A subset of source code belonging to a **technology** (i.e. JEE code, .NET code, ABAP code, etc.) |
| Tree Node | A structure that organizes application, modules, and components in a tree |

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
| dbType | Database type either `"AAD"` for Measurement Service database or `"ADG"` for Dashboard Service database | String | 1 |
| version | Package version of `ADG_FULL_CENTRAL`; which is considered as CAST AIP version number | String | 1 |
| schema | Name of the database schema | String | 1 |
| dbmsVersion | Product name and version of the Database Management System host | String | 1 |
| componentsIndexStatus | Status of the Lucene components index | String | 1 |
| violationsIndexStatus | Status of the Lucene violations index | String | 1 |
| systems.href | For a Central database, reference to get an array of systems | URI | 1 |
| applications.href | Reference to get an array of applications | URI | 1 |
| nbOfApplications | Returns total applications count present in domain | int | 1 |
| nbOfAuthorizedApplications | Returns authorized applications count, returns only for domain content (`/{domainName}`) | int | 1 |
| nbOfAuthorizedSnapshots | Returns authorized snapshots count, returns only for domain content (`/{domainName}`) | int | 1 |
| configurations.href | Reference to the history of configurations | URI | 1 |
| results.href | Reference to get results of all applications | URI | 1 |
| commonCategories.href | For a Measurement Database, reference to get all defined categories | URI | 1 |
| tags.href | For a Measurement Database, reference to get all application assignments | URI | 1 |
| technologyResults | For selected domain, return array of reference to get results for each technology | Array | 1 |
| technologyResults.href | For each technology present in selected domain, returns reference to get results | URI | 1 |

### JSON Example

JSON format for Array of domains

**GET DEMO**

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

JSON format for domain content

**GET DEMO**

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
  "nbOfAuthorizedApplications": 2,
  "nbOfAuthorizedSnapshots": 4,
  "configurations": {
    "href": "ENDTOEND833/configuration/snapshots",
    "name": "All configuration snapshots ..."
  },
  "results": {
    "href": "ENDTOEND833/results",
    "name": "Results by snapshots, by systems, by ..."
  },
  "commonCategories": null,
  "tags": null,
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
| technologies | Array of technologies as an union of technologies used by all applications of this system | Array | 1 |
| technologies[] | A technology | String | 1..* |
| applications.href | Reference to get an array of applications | URI | 1 |
| results.href | Reference to get an array of result items | URI | 1 |

### JSON Example

**GET DEMO**

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
| technologies | Array of technologies as an union of technologies used by the application | Array | 1 |
| technologies[] | A technology | String | 1..* |
| system.href | For a Central database, reference to get the system owner | URI | 1 |
| modules.href | For a Central database, reference to get an array of modules | URI | 1 |
| snapshots.href | Reference to get an array of application snapshots | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| origin.href | For a Measurement Database, reference to the origin application stored in Central Database. The related domain must be defined | URI | 1 |
| attachments.href | Reference to get an array of attachments | URI | 1 |
| adgDatabase | For a Measurement Database, name of the Central database hosting this application | String | 0..1 |
| adgWebSite | For a Measurement Database, full http address to AED web site hosting information detail about this application. | String | 0..1 |
| adgLocalId | For a Measurement Database, local ID for this application defined on the database of adgWebSite | String | 0..1 |
| adgVersion | For a Measurement Database, AED web site version. | String | 0..1 |
| technologyResults | For all Applications present in database, return array of reference to get results for each technology | Array | 1 |
| technologyResults.href | For each technology present in application, returns reference to get results | URI | 1 |

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
    },
    {
      "href": "DEMO/applications/6/technologies-results?name=Cobol",
      "name": "Filtered results for DEMO by technology Cobol"
    },
    {
      "href": "DEMO/applications/6/technologies-results?name=DB2 Server",
      "name": "Filtered results for DEMO by technology DB2 Server"
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
| technologyResults | For all modules present in database, return array of reference to get results for each technology | Array | 1 |
| technologyResults.href | For each technology present in module, returns reference to get results | URI | 1 |

### JSON Example

**GET DEMO/applications/6**

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
| number | Snapshot order number. Snapshots are ordered according to the annotation.date | Integer | 1 |
| technologies | Array of technologies used by this application snapshot | Array | 1 |
| technologies[] | A technology | String | 1..* |
| annotation | User annotations/information describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values: `"EFP"` or `"AEP"` | String | 1 |
| configurationSnapshot.href | Reference to the snapshot configuration | URI | 1 |
| system.href | For a Central database, reference to the system owner | URI | 1 |
| application.href | Reference to get the application owner | URI | 1 |
| modulesSnapshots.href | For an ADG database, reference to get an array of module snapshots | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| actionPlan | For a Central database. | Structure | 1 |
| actionPlan.issues.href | Array of issues for this application snapshot | URI | 1 |
| actionPlan.summary.href | Summary of all issues | URI | 1 |
| activeExclusions | For a central database | Structure | 1 |
| activeExclusions.exclusions.href | Array of excluded violations for this application snapshot | URI | 1 |
| activeExclusions.summary.href | Summary of excluded violations | URI | 1 |
| scheduledExclusions | For a central database | Structure | 1 |
| scheduledExclusions.exclusions.href | Array of scheduled exclusions in this application snapshot (exclusions will be effective in the next snapshot) | URI | 1 |
| scheduledExclusions.summary.href | Summary of scheduled exclusions | URI | 1 |
| components.href | For a Central database, reference to this application snapshot's components having a PRI | URI | 1 |
| transactions.href | For a Central database, reference to the transactions of this application snapshot | URI | 1 |
| violations.href | For a Central database, reference to the violations of this application snapshot | URI | 1 |
| treeNode.href | For a Central Database, reference to this application tree-node to get nested components. null if current snapshot is not the last snapshot | URI | 1 |
| origin.href | For a Measurement Database, reference to the origin application snapshot stored in Central Database. The related domain must be defined | URI | 1 |
| adgLocalId | For a Measurement Database, local ID for this snapshot defined on the original central database | String | 0..1 |
| technologyResults | For all snapshots present in selected application, return array of reference to get results for each technology | Array | 1 |
| technologyResults.href | For each technology present in selected application snapshot, returns reference to get results | URI | 1 |

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
  "systems": {
    "href": "DEMO/applications/3/systems",
    "name": "Systems of DEMO"
  },
  "application": {
    "href": "DEMO/applications/3",
    "name": "DEMO"
  },
  "moduleSnapshots": {
    "href": "DEMO/applications/3/snapshots/8/modules",
    "name": "Module Snapshots for DEMO #2"
  },
  "results": {
    "href": "DEMO/applications/3/snapshots/8/results",
    "name": "Results breakdown by modules, by technologies"
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
  },
  "activeExclusions": {
    "exclusions": {
      "href": "DEMO/applications/3/snapshots/8/excluded-violations",
      "name": "Active exclusions for application DEMO in snapshot 2"
    },
    "summary": {
      "href": "DEMO/applications/3/snapshots/8/excluded-violations-summary",
      "name": "Active exclusions summary for application DEMO in snapshot 2"
    }
  },
  "scheduledExclusions": {
    "exclusions": {
      "href": "DEMO/applications/3/snapshots/8/exclusions/scheduled",
      "name": "Scheduled exclusions for application DEMO in snapshot 2"
    },
    "summary": {
      "href": "DEMO/applications/3/snapshots/8/exclusions/scheduled-summary",
      "name": "Scheduled exclusions summary for application DEMO in snapshot 2"
    }
  },
  "components": {
    "href": "DEMO/applications/3/snapshots/8/components",
    "name": "Components for application DEMO in snapshot 2"
  },
  "transactions": {
    "href": "DEMO/applications/3/snapshots/8/transactions",
    "name": "Transactions for application DEMO in snapshot 2"
  },
  "violations": {
    "href": "DEMO/applications/3/snapshots/8/violations",
    "name": "Violations for application DEMO in snapshot 2"
  },
  "technologyResults": [
    {
      "href": "DEMO/applications/3/snapshots/8/technologies-results?name=JEE",
      "name": "Filtered results for applications DEMO in snapshot 69 by technology JEE"
    },
    {
      "href": "DEMO/applications/3/snapshots/8/technologies-results?name=FLEX",
      "name": "Filtered results for applications DEMO in snapshot 69 by technology FLEX"
    },
    {
      "href": "DEMO/applications/3/snapshots/8/technologies-results?name=Oracle Server",
      "name": "Filtered results for applications DEMO in snapshot 69 by technology Oracle Server"
    },
    {
      "href": "DEMO/applications/3/snapshots/8/technologies-results?name=PL/SQL",
      "name": "Filtered results for applications DEMO in snapshot 69 by technology PL/SQL"
    }
  ]
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
| number | Snapshot order number. Snapshots are ordered according to the annotation.date | Integer | 1 |
| technologies | Array of technologies by this module | Array | 1 |
| technologies[] | A technology | String | 1..* |
| annotation | User annotations describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values: `"EFP"` or `"AEP"` | String | 1 |
| configurationSnapshot.href | Reference to the snapshot configuration | URI | 1 |
| module.href | Reference to get the module owner | URI | 1 |
| applicationSnapshot.href | Reference to get the application snapshot owner | URI | 1 |
| results.href | Reference to get an array of results | URI | 1 |
| actionPlan | For a Central database. | Structure | 1 |
| actionPlan.issues.href | Array of issues for this module snapshot | URI | 1 |
| actionPlan.summary.href | Summary of all issues | URI | 1 |
| components.href | For a Central database, reference to this module snapshot's components having a PRI | URI | 1 |
| diagnosisResults.violations.href | For a Central database, reference to the violations of this application snapshot | URI | 1 |
| treeNode.href | For a Central Database, reference to this module tree-node to get nested components. null if current snapshot is not the last snapshot | URI | 1 |
| technologyResults | For all snapshots present in selected module, return array of reference to get results for each technology | Array | 1 |
| technologyResults.href | For each technology present in selected snapshot, returns reference to get results | URI | 1 |

### JSON Example

**GET DEMO/modules/4/snapshots/8**

```json
{
  "href": "DEMO/modules/4/snapshots/8",
  "name": "Flex",
  "number": 2,
  "technologies": ["FLEX"],
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
    "href": "DEMO/modules/4/application",
    "name": "Applications for Flex"
  },
  "module": {
    "href": "DEMO/modules/4",
    "name": "Flex"
  },
  "applicationSnapshot": {
    "href": "DEMO/applications/3/snapshots/8",
    "name": "DEMO"
  },
  "results": {
    "href": "DEMO/modules/4/snapshots/8/results",
    "name": "Results breakdown by technologies"
  },
  "actionPlan": {
    "issues": null,
    "summary": {
      "href": "DEMO/modules/4/snapshots/8/action-plan/summary",
      "name": "Action plan summary for module Flex in snapshot 2"
    }
  },
  "components": {
    "href": "DEMO/modules/4/snapshots/8/components",
    "name": "Components for module Flex in snapshot 2"
  },
  "diagnosisResults": {
    "violations": {
      "href": "DEMO/applications/3/snapshots/8/violations",
      "name": "Violations for application DEMO in snapshot 2"
    }
  },
  "technologyResults": [
    {
      "href": "DEMO/applications/3/snapshots/8/technologies-results?name=FLEX",
      "name": "Filtered results for applications DEMO in snapshot 69 by technology FLEX"
    }
  ]
}
```

---

## Attachments

### Description

A free JSON content can be attached to an application. Each JSON content is referred with a key.

> **Warning:** keys starting with `"CAST:"` are reserved for CAST AIP Product.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/attachments` | Get an array of all references to JSON contents |
| GET | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Get a JSON content attached to an application for a given key |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments` | Get an array of all references to JSON contents |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Get a JSON content attached to an application snapshot for a given key |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Map a JSON Content to an application and a key |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Map a JSON Content to an application snapshot and a key |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/attachments/{Key}` | Delete a JSON Content for an application and a key |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{snapshotId}/attachments/{Key}` | Delete a JSON Content for an application snapshot and a key |

---

## Application to Tags Assignments

> **Measurement Database only**

### Description

A tag is used to filter a set of applications when results are requested. It is up to the REST client to filter applications according to tags using the service listing all applications.

The purpose of the REST API is to minimize request & response flows between client and server relying on client cache management (which is based on `304 NOT MODIFIED` response status). Therefore the client navigator should emit the same requests to the server. If ever the server has been restarted then the client cache would be refreshed.

Therefore, the client should rely on a subset of requests. For instance, to get all applications TQI on a subset of applications, requests TQI for all applications, then filter the results.

#### Notions

- **Tag**: A tag is a keyword.
- **Category**: A tag belongs to a category. A category is a set of tags.
- **Application to Tags assignment**: A tag can be assigned to an application, but cannot be assigned to a specific application snapshot.

#### Rules

1. Common categories, tags, and assignments are created for all users.
2. Technologies are similar to tags but are not tags. It is up to the webapp to display these data as tags.
3. When an application is not assigned to any tag of a category, then the webapp considers this application attached to the `"other"` tag for this category. It is up to the webapp to provide this feature.
4. Tags labels cannot be translated.
5. Tags and categories label can be changed.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/tags` | Get an array of all application to tag assignments |
| PUT | application/json | `{Domain}/tags` | Replace application to tag assignments |

### JSON Representation

#### Response (GET)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| application.href | Application Reference | URI | 1 |
| technologies | Technologies | Array | 1 |
| technologies[] | A technology | String | 1..* |
| commonTags | Array of Label | Array | 1 |
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

A regular category is a container of tags.

A parent category is a container of other categories in order to organize categories as a tree structure.

Web Services may raise errors if following constraints are not verified:

- A category must define items (at least one tag or one category)
- A category has a unique name
- A category cannot define both items and categories
- For a given category a tag has a unique name
- Parent categories MUST NOT define circular relationship
- A category cannot be empty

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/common-categories` | Get an array of all common categories definitions. |
| PUT | application/json | `{Domain}/common-categories` | Update a collection of common categories. **Warning:** If some tags are missing, then they will be deleted in database. |
| PUT | text/csv | `{Domain}/common-categories` | Bulk updates of ALL common categories. This call creates new categories and tags, updates categories and tags labels, removes categories and tags. **Warning:** All existing categories must be put in this payload (see result of GET), otherwise they will be deleted. **Warning:** A category must define at least one tag or one sub-category. **Warning:** A tag or a category without key is considered as new item to create. |
| POST | application/json | `{Domain}/common-categories` | Create a collection of common categories. Request content example: `[ { "label":"MyTags", "tags": [ {"label":"Favorites"}, {"label":"Major"}, {"label":"Minor"} ] } ]` Response content example: `[{ "key":"5", "label":"MyTags", "tags": [ {"key":"10", "label":"Favorites"}, {"key":"11", "label":"Major"}, {"key":"12", "label":"Minor"} ] } ]`|
| DELETE | application/json | `{Domain}/common-categories` | Delete a collection of common categories (tags to Applications assignments are deleted). Request content example: `[ { "key" : "1" }, { "key" : "2" }, { "key" : "3" } ]`|

### JSON Representations

| Properties | Response Structure | Payload POST Structure | Payload DELETE Structure | Payload PUT Structure |
|---|---|---|---|---|
| key | Unique global ID | N/A | Required ID of a category to delete | Required ID of a category to update |
| label | Category Label | Required label of a category to create | N/A | Optional label: if set this is a label update |
| **Regular Category Only** | | | | |
| tags | Array of tags | Required array of tags to create | N/A | Full array of tags (if some existing tags are not set then they will be removed) |
| tags[] | A tag | Required tag to create | N/A | A tag |
| tag[].key | Unique global tag ID | N/A | N/A | Optional tag ID: if not set a tag will be added |
| tag[].label | Tag label | Required tag label | N/A | Optional label: if set this is a label update, or a tag to create |
| **Parent Category Only** | | | | |
| categories | Array of sub-categories | Required array of sub categories | N/A | Full array of sub categories (if some existing sub categories are not set then they will be removed) |
| categories[] | A sub-category | Required sub category | N/A | A sub category |
| categories[].key | Sub-category key | Optional sub category key: if not set a label is used | N/A | Required a sub category ID |
| categories[].label | Sub-category label | Optional sub category label: if set this is a category created in this payload | N/A | N/A |

### JSON Example

```json
[
  { "key": "26", "label": "Departments", "tags": [{ "key": "181", "label": "Dept1" }, { "key": "165", "label": "Dept2" }] },
  { "key": "4",  "label": "Countries", "tags": [{ "key": "3", "label": "France" }, { "key": "93", "label": "Germany" }, { "key": "4", "label": "Italy" }] },
  { "key": "100", "label": "Location", "categories": [{ "key": "26", "label": "Departments" }, { "key": "4", "label": "Countries" }] }
]
```

### CSV Representation

| Column | Type | Response | PUT Payload |
|---|---|---|---|
| Category Label | String | Category label | A label for a category to create or to update |
| Category Key | String | Category key (empty for a creation) | A key for an existing category |
| Item Type | String | Item types: either `TAG` for a Regular Category or `CATEGORY` for a Parent Category |Item types: either `TAG` for a Regular Category or `CATEGORY` for a Parent Category|
| Item Label | String | Tag or Sub Category label | If Item Type is `TAG`, a label for a tag to create or update. If Item Type is `CATEGORY`, a sub category referred with a label for a category created in this payload |
| Item Key | String | Tag or Sub Category key (empty for an item created with this payload) | If Item Type is `TAG`, a key for an existing tag. If Item Type is `CATEGORY`, a key for an existing category |

### CSV Example

Categories and Tags creation:

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

Rename 'UK' as 'United Kingdom', add 'Italy' as new tag for 'Europe', create a new category 'Toplevel' as parent of 'Team Size' and 'Region':

```csv
Category Label;Category Key;Item Type;Item Label;Item Key
Team Size;1;TAG;Small Size;1
Team Size;1;TAG;Large Size;2
Region;2;CATEGORY;West;3
Region;2;CATEGORY;East;4
West;3;CATEGORY;Europe;5
West;3;CATEGORY;U.S.;6
East;4;TAG;China;5
East;4;TAG;India;6
Europe;5;TAG;France;7
Europe;5;TAG;United Kingdom;8
Europe;5;TAG;Italy;
U.S.;6;TAG;Texas;9
U.S.;6;TAG;New York;10
Toplevel;;CATEGORY;Team Size;1
Toplevel;;CATEGORY;Region;2
```

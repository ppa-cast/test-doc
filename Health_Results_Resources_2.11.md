# Health Results Resources - 2.11

*Created by N Padmavathi on Feb 12, 2024*

---

## On this page

- [Glossary](#glossary)
- [Assessment Result](#assessment-result)
- [Quality Standards](#quality-standards)
- [Custom Quality Tags](#custom-quality-tags)
- [Background Facts Injection](#background-facts-injection)

---

## Glossary

| Term | Definition |
|---|---|
| Result | An assessment result of an application or a module |
| Result Detail | Additional values related to a result: intermediate calculations, breakdowns, quantitative values |
| Quality Standard Reference | A reference to a Quality Standard such as CWE-352 |

---

## Assessment Result

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/results{?parameters}` | Array of results split by snapshots and applications |
| PUT | text/csv | `{Domain}/results` | Create or update background facts for several applications in the last snapshot |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/results{?parameters}` | Results for a given application |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/results{?parameters}` | Results for a given application and snapshot |
| PUT | text/csv | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/results` | Create or update background facts for a given application and snapshot |
| GET | application/json, text/csv, xlsx | `{Domain}/modules/{ModuleID}/results{?parameters}` | Results for a given module |
| GET | application/json, text/csv, xlsx | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/results{?parameters}` | Results for a given module and snapshot |
| GET | application/json | `{Domain}/transactions/{TransactionID}/results{?parameters}` | Results for a given transaction |
| GET | application/json, text/csv, xlsx | `{Domain}/technologies-results{?parameters}` | Results split by technology |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/technologies-results{?parameters}` | Results by technology for a selected application |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/technologies-results{?parameters}` | Results by technology for a selected application and snapshot |

**URI Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| quality-indicators | List of quality indicators | IDs or keywords: `business-criteria`, `technical-criteria`, `quality-rules`, `quality-distributions`, `quality-measures`. Use `c:`, `cc:`, `nc:` modifiers for contributors. | 60017 (Total Quality Index) if none set |
| quality-standards | List of quality standards references (e.g. `CISQ,CISQ-Maintainability`). Use `select=(evolutionSummary)` to get violation counts. | String | None |
| sizing-measures | List of sizing measures. Keywords: `technical-size-measures`, `functional-weight-measures`, `critical-violation-statistics`, `violation-statistics`, `technical-debt-statistics`, `run-time-statistics` | IDs or keywords | None |
| background-facts | List of background fact IDs | IDs | None |
| metrics | Any metric identifier or keyword | IDs or keywords; supports `c:`, `cc:`, `nc:` | 60017 |
| applications | List of applications | `$all` or comma-separated names | `$all` |
| modules | List of modules | `$all` or comma-separated names | None |
| technologies | List of technologies | `$all` or comma-separated names | None |
| snapshots | List of snapshots | Integer (`-1` = last, `1` = first), `$all`, ISO date, or `$months(N)` | -1 |
| snapshot-ids | List of snapshot IDs (exclusive with `snapshots`) | Comma-separated integers | None |
| select | Result options to display | `violationRatio`, `evolutionSummary`, `categories`, `aggregators`, `improvementGap`, `omgTechnicalDebt` | None |
| unselect | Result options to hide | `grade` | None |
| format | Output format options | `snapshotsAsRows` (CSV/Excel: show snapshots as rows instead of columns) | None |
| order | Sorting options | `improvementGap`, `rule-pattern-name`, `rule-id` | `asc(rule-id)` |

### Excel/CSV Representation (Output)

| Columns | Description | Type |
|---|---|---|
| Application Name | Application name | String |
| Module Name | Module name | String |
| Technology | Technology name | String |
| Metric Name | Associated metric name | String |
| Snapshot Date #N | Date of assessment result (N=1 is most recent) | Date |
| Result #N | Grade (quality indicator) or integer (other) | Number |
| Violations (Result #N) | Failed checks (when `select=violationRatio`) | Number |
| Total Checks (Result #N) | Total checks (when `select=violationRatio`) | Number |
| Added Critical Violations (Result #N) | Added critical violations (when `select=evolutionSummary`) | Number |
| Critical Violations in New and Modified Code (Result #N) | Critical violations in new/modified code | Number |
| Removed Critical Violations (Result #N) | Removed critical violations | Number |
| Total Critical Violations (Result #N) | Total critical violations | Number |
| *CategoryName* (Result #N) | Category result (when `select=categories`) | Number |

### CSV Representation (Input for Background Facts)

| Columns | Type | Description |
|---|---|---|
| ADG Database | String | `adgDatabase` property (required for AAD, optional for AED) |
| Application Name | String | Name of the application |
| Module Name | String | Name of the module (empty = application itself) |
| Metric Id | Integer | Background Fact ID |
| Result | Double | Value of the Background Fact |

**CSV Example — AAD domain**

```csv
ADG Database;Application Name;Module Name;Metric Id;Result
ADG_GACD;DIDD Diccionarios;;2666003;21
ADG_GACD;DIDD Diccionarios;DIDD_CAPP_ESQUEMAS;2666003;9
ADG_GACD;DIDD Diccionarios;DIDD_J2EE;2666003;12
```

### JSON Representations & Examples

#### Result Item

| Properties | Description | Type | Occurs |
|---|---|---|---|
| number | Application snapshot order number | Integer | 1 |
| date | Date of the application snapshot | Date | 1 |
| application | Reference | URI | 1 |
| applicationSnapshot | Reference to the application snapshot | URI | 1 |
| applicationResults | All results | Array | 1 |

#### Application Result — Business Criterion

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | `"business-criterion"` | String | 1 |
| technologiesResults | Results breakdown by technology | Array | 0..1 |
| modulesResults | Results breakdown by module | Array | 0..1 |
| transactionResults | Results breakdown by transaction | Array | 0..1 |
| result.grade | Grade between 1.0 and 4.0 | Double | 0..1 |
| result.evolutionSummary | See Evolution Summary structure | Structure | 0..1 |
| reference.href | URI | 1 |
| reference.name | String | 1 |
| reference.shortName | String | 1 |
| reference.key | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "business-criteria",
        "technologiesResults": [],
        "result": { "grade": 3.04528, "boundaries": null, "evolutionSummary": null },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/60017/snapshots/5",
          "name": "Total Quality Index",
          "shortName": "TQI",
          "key": "60017"
        },
        "transactionResults": []
      }
    ]
  }
]
```

#### Application Result — Sizing Measure

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | One of: `technical-size-measures`, `functional-weight-measures`, `critical-violation-statistics`, `technical-debt-statistics`, `run-time-statistics` | String | 1 |
| result.value | Size value | Double | 0..1 |
| reference | Reference to the Sizing Measure | Structure | 1 |

#### Application Result — Background Fact

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | `"background-fact"` | String | 1 |
| result.value | Fact value | Double | 0..1 |
| reference | Reference to the Background Fact | Structure | 1 |

### Optional Result Detail — Violation Ratio

Returned when `select=violationRatio` is used with Quality Rules.

| Properties | Description | Type | Occurs |
|---|---|---|---|
| violationRatio.totalChecks | Total checked items | Integer | 1 |
| violationRatio.failedChecks | Failed items | Integer | 1 |
| violationRatio.successfulChecks | totalChecks - failedChecks | Integer | 1 |
| violationRatio.ratio | Compliance ratio (successfulChecks / totalChecks) | Double | 1 |
| violationRatio.violationOccurrences | Total violation occurrences (bookmarks, since AIP 8.3.33) | Integer | 1 |

### Optional Result Detail — Evolution Summary

Returned when `select=evolutionSummary` is used with Business Criteria.

| Properties | Description | Type | Occurs |
|---|---|---|---|
| evolutionSummary.removedCriticalViolations | Critical violations removed since previous snapshot | Integer | 1 |
| evolutionSummary.addedCriticalViolations | Critical violations added since previous snapshot | Integer | 1 |
| evolutionSummary.criticalViolationsInNewAndModifiedCode | Critical violations in new/modified code | Integer | 1 |
| evolutionSummary.totalCriticalViolations | Total critical violations | Integer | 1 |
| evolutionSummary.addedViolations | Violations added since previous snapshot | Integer | 1 |
| evolutionSummary.removedViolations | Violations removed since previous snapshot | Integer | 1 |

### Optional Result Detail — OMG Technical Debt

Returned when `select=omgTechnicalDebt` is used with CISQ Quality Indicators.

| Properties | Description | Type | Occurs |
|---|---|---|---|
| omgTechnicalDebt.total | Total Technical Debt | Integer | 1 |
| omgTechnicalDebt.numberOccurrences | Number of violation occurrences | Integer | 1 |
| omgTechnicalDebt.added | Technical Debt of added violations | Integer | 1 |
| omgTechnicalDebt.removed | Technical Debt of removed violations | Integer | 1 |

### Optional Result Detail — Categories

Returned when `select=categories` is used with Quality Distributions.

| Properties | Description | Type | Occurs |
|---|---|---|---|
| categories[] | A category result | Structure | 1..4 |
| categories[].key | Category ID | String | 1 |
| categories[].name | Category name | String | 1 |
| categories[].value | Category measure | Integer | 1 |

### Technology Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| technology | Technology name | String | 1 |
| result | See result structure | Structure | 1 |

### Module Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| module.href | Module snapshot URI | URI | 1 |
| module.name | Module snapshot name | String | 1 |
| module.number | Module snapshot number | Integer | 1 |
| result | Result structure | Double | 0..1 |
| technologiesResults | Results breakdown by technology | Array | 0..1 |

### Transaction Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| transaction.href | Transaction snapshot URI | URI | 1 |
| transaction.name | Transaction snapshot name | String | 1 |
| result | Result structure | Double | 0..1 |

---

## Quality Standards

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-standards-categories/{QualityStandardCategory}?{parameters}` | All quality standard references for a given category (e.g. OWASP-2017, STIG-V4R8-CAT1) |
| GET | application/json | `{Domain}/applications/{ID}/quality-standards` | Quality standard references for an application (only references with violations) |
| GET | application/json | `{Domain}/applications/{ID}/snapshots/{SnapshotID}/quality-standards` | Quality standard references for an application snapshot |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| standard | Quality Standard name | String | 1 |
| id | Reference name (aka tag) | String | 1 |
| name | Reference label | String | 1 |
| description | Reserved | String | 1 |
| applicable | Whether applicable to static code analysis | Boolean | 1 |
| contributors | List of rules associated to each Quality Standard | Structure | 0..* |

### Query Parameters

| Parameter | Description | Values | Default |
|---|---|---|---|
| select | Return list of contributing rules | `contributors` | N/A |

### JSON Example

```json
[
  { "standard": "CISQ", "id": "ASCMM-MNT-6", "name": "Commented Code Element Excessive Volume" },
  { "standard": "CISQ", "id": "ASCSM-CWE-681", "name": "Numeric Types Incorrect Conversion" }
]
```

---

## Custom Quality Tags

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | text/csv | `{Domain}/custom-quality-tags` | Download CUSTOM TAGS mapping |
| PUT | text/csv | `{Domain}/custom-quality-tags` | Add rule/CUSTOM TAGS mapping (tag must begin with `CUSTOM` prefix). New tags assigned to `CUSTOM` standard. |
| DELETE | text/csv | `{Domain}/custom-quality-tags` | Remove rule/CUSTOM TAGS mapping (tag must begin with `CUSTOM` prefix) |

**Curl examples:**

```bash
# GET
curl --header "Accept: text/csv" http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags

# PUT
curl -X PUT --header "Content-type: text/csv" --upload-file data.csv   http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags

# DELETE
curl -X DELETE --header "Content-type: text/csv" --upload-file data.csv   http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags
```

### CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Rule ID | A rule ID | Integer | 1 |
| Tag | A label starting with `CUSTOM` | String | 1 |

### CSV Example

```csv
Rule ID;Tag
3626;CUSTOM-TOP-PRIORITY-RULES
```

---

## Background Facts Injection

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| PUT | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/results` | Update an array of Background Facts results for a given snapshot and application |

### JSON Representation

A result item:

| Properties | Description | Type | Occurs |
|---|---|---|---|
| key | A background fact ID | String | 1 |
| result | A numeric value | Decimal | 1 |

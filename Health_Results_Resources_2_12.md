# Health Results Resources - 2.12

*Created by James Hurrell on Apr 4, 2024*

---

## On this page

- [Glossary](#glossary)
- [Assessment Result](#assessment-result)
  - [URI Templates & Parameters](#uri-templates--parameters)
  - [Excel/CSV Representation (Output)](#excelcsv-representation-output)
  - [CSV Representation (Input)](#csv-representation-input)
  - [CSV Examples](#csv-examples)
  - [JSON Representations & JSON Examples](#json-representations--json-examples)
    - [Result Item](#result-item)
    - [Application Result](#application-result)
      - [Case of a Business Criterion](#case-of-a-business-criterion)
      - [Case of a Technical Criterion](#case-of-a-technical-criterion)
      - [Case of a Quality Rule](#case-of-a-quality-rule)
      - [Case of a Quality Distribution](#case-of-a-quality-distribution)
      - [Case of a Quality Measure](#case-of-a-quality-measure)
      - [Case of a Sizing Measure](#case-of-a-sizing-measure)
      - [Case of a Background Fact](#case-of-a-background-fact)
    - [Optional Quality Indicator Result Detail](#optional-quality-indicator-result-detail)
      - [Violation Ratio (option coming with Quality Rules)](#violation-ratio-option-coming-with-quality-rules)
      - [Evolution Summary (option coming with Business Criteria)](#evolution-summary-option-coming-with-business-criteria)
      - [OMG Technical Debt (option coming with Business Criteria, Technical Criteria, Quality Rules)](#omg-technical-debt-option-coming-with-business-criteria-technical-criteria-quality-rules)
      - [Category (option coming with Quality Distributions)](#category-option-coming-with-quality-distributions)
    - [Technology Result Detail](#technology-result-detail)
    - [Module Result Detail](#module-result-detail)
    - [Transaction Result Detail](#transaction-result-detail)
- [Quality Standards](#quality-standards)
  - [URI Templates & Parameters](#uri-templates--parameters-1)
  - [JSON Representation](#json-representation)
  - [Query Parameters](#query-parameters)
  - [JSON Example](#json-example)
- [Custom Quality Tags](#custom-quality-tags)
  - [URI Templates & Parameters](#uri-templates--parameters-2)
  - [CSV Representation](#csv-representation)
  - [CSV Example](#csv-example)
- [Background Facts Injection](#background-facts-injection)
  - [URI Templates & Parameters](#uri-templates--parameters-3)
  - [JSON Representation](#json-representation-1)

---

## Glossary

| Term | Definition |
|---|---|
| Result | An assessment result of an application or a module. |
| Result Detail | Additional values, indicators, related to a result:<br/>intermediate calculation results;<br/>breakdown of a measure;<br/>related quantitative values |
| Quality Standard Reference | A reference to a Quality Standard such as CWE-352 |

---

## Assessment Result

### URI Templates 

- **GET**  `{Domain}/results{?parameters}`

  - *Description*:

    Array of results of a domain split by snapshots, by applications

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **PUT**  `{Domain}/results{?parameters}`

  - *Description*:

    Create or update background facts values in a AED or AAD domain for several applications in the last snapshot

  - *Media Type*:
    - `text/csv`

- **GET**  `{Domain}/applications/{ApplicationID}/results{?parameters}`

  - *Description*:

    Array of results for a given application

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/results{?parameters}`

  - *Description*:

    Array of results for a given application and snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **PUT**  `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/results{?parameters}`

  - *Description*:

    Create or update background facts values in a AED or AAD domain for a given application and snapshot

  - *Media Type*:
    - `text/csv`

- **GET**  `{Domain}/modules/{ModuleID}/results{?parameters}`

  - *Description*:

    Array of results for a given module

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/results{?parameters}`

  - *Description*:

    Array of results for a given module and snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/transactions/{TransactionID}/results{?parameters}`

  - *Description*:

    Array of results for a given transaction

  - *Media Type*:
    - `application/json`

- **GET**  `{Domain}/technologies-results{?parameters}`

  - *Description*:

    Array of results of a domain split by technology

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/applications/{ApplicationID}/technologies-results{?parameters}`

  - *Description*:

    Array of results of a domain split by technology for selected application

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/technologies-results{?parameters}`

  - *Description*:

    Array of results of a domain split by technology for selected application and snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/modules/{ModuleID}/technologies-results{?parameters}`

  - *Description*:

    Array of results of a domain split by technology for selected module

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET**  `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/technologies-results{?parameters}`

  - *Description*:

    Array of results of a domain split by technology for selected module and snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`


### URI Parameters

- **quality-indicators**

    - *Description:* Specify a list of quality indicators

    - *Values:* Quality Indicators identifiers or keywords, separated by a comma

        - `.../results/?quality-indicators=(business-criteria)`
        - `.../results/?quality-indicators=(technical-criteria)`
        - `.../results/?quality-indicators=(quality-rules)`
        - `.../results/?quality-indicators=(quality-distributions)`
        - `.../results/?quality-indicators=(quality-measures)`

        Use `c:` modifier to specify direct grade contributors of a Quality Indicator or a Quality Standard reference (aka tag)

        - `c:61027`
        - `c:CISQ`

        > Note: this list is based on the union set of targeted snapshots

    - *Value Modifiers:*

        - `c:` — specify direct grade contributors of a Quality Indicator or a Quality Standard reference (aka tag)
        - `cc:` — specify critical grade contributors of a Quality Indicator (e.g. `cc:61027`)

            > Note: the critical contributors of a Business Criterion (eg. `cc:61027`) are the BC's critical children, union the TCs' critical children. A Quality Rule is said to be critical if it is critical for at least one of its parents. In the Assessment Model, some metrics have multiple parents therefore a Quality Rule can be critical for parent A, and non-critical for parent B. In other words, using the `cc` modifier can return non-critical Quality Rules.

        - `nc:` — specify non-critical grade contributors of a Quality Indicator (e.g. `nc:61027`)

           > Note: the non-critical contributors (`nc`) of a Business Criterion (eg. `nc:61027`) are the BC's non-critical children, union the TCs' non-critical children. A Quality Rule is said to be critical if it is critical for at least one of its parents. In the Assessment Model, some metrics have multiple parents therefore a Quality Rule can be critical for parent A, and non-critical for parent B.

    - *Default value:* `60017` (Total Quality Index) — used if `quality-indicators`, `sizing-measures` and `background-facts` parameters are not set

- **quality-standards**

    - *Description:* Specify a list of quality standards references

    - *Values:* Quality Standards references, separated by a comma

        - `.../results/?quality-standards=(CISQ,CISQ-Maintainability)`

        > Must be requested with the option in URL: `select=(evolutionSummary)` to retrieve the counts of violations

    - *Value Modifiers:* None
    - *Default value:* None

- **sizing-measures**

    - *Description:* Specify a list of sizing measures

    - *Values:* Sizing measures identifiers or keywords, separated by a comma

        - `.../results/?sizing-measures=(technical-size-measures)`
        - `.../results/?sizing-measures=(functional-weight-measures)`
        - `.../results/?sizing-measures=(critical-violation-statistics)`
        - `.../results/?sizing-measures=(violation-statistics)`
        - `.../results/?sizing-measures=(technical-debt-statistics)`
        - `.../results/?sizing-measures=(run-time-statistics)`

    - *Value Modifiers:* None
    - *Default value:* None

- **background-facts**

    - *Description:* Specify a list of background facts

    - *Values:* Background Facts identifiers, separated by a comma

        - `.../results/?background-facts=(66061)`

    - *Value Modifiers:* None
    - *Default value:* None

- **metrics**

    - *Description:* Specify a list of metrics

    - *Values:* Any metric identifier (quality-indicator, sizing-measure or background-fact) or keyword, separated by a comma

    - *Value Modifiers:* `c:`, `cc:`, `nc:`

    - *Default value:* `60017`

- **applications**

    - *Description:* Specify a list of applications

    - *Values:* Keyword `$all` or application names separated by a comma

        - `.../results/?applications=($all)`
        - `.../results/?applications=(name1,name with space 2)`

    - *Value Modifiers:* None
    - *Default value:* `$all` (all applications)

- **modules**

    - *Description:* Specify a list of modules

    - *Values:* Keyword `$all` or module names separated by a comma

        - `.../results/?modules=($all)`
        - `.../results/?modules=(name1,name with space 2)`

    - *Value Modifiers:* None
    - *Default value:* None

- **technologies**

    - *Description:* Specify a list of technologies

    - *Values:* Keyword `$all` or technology names separated by a comma

        - `.../results/?technologies=($all)`
        - `.../results/?technologies=(name1,name with space 2)`

    - *Value Modifiers:* None
    - *Default value:* None

- **snapshots**

    - *Description:* Specify a list of snapshots

    - *Values:* The input value can be one of the following:

        - A positive or negative integer

            - `.../results/?snapshots=(-1)`
            - `.../results/?snapshots=(1)`

            A negative number matches snapshots from the end (e.g. `-1` matches the last snapshot, `-2` matches the last and previous snapshots).
            A positive number matches snapshots from the beginning (e.g. `1` matches the first snapshot, `2` matches the first and second snapshots).

        - The keyword `$all`

            - `.../results/?snapshots=($all)`

            This means all snapshots.

        - A snapshot date

            - `.../results/?snapshots=(2012-10-01)`

            The date format is the standard W3C Date format: `YYYY-MM-DD`

            If there is a snapshot at this date, select this snapshot, otherwise select the oldest snapshot after this date, otherwise select the most recent snapshot before this date.

        - A snapshot month

            - `.../results/?snapshots=$months(12)`

            Return all snapshot results within the given months from last analysis date.

    - *Value Modifiers:* None
    - *Default value:* `-1` (last snapshot)

- **snapshot-ids**

    - *Description:* Specify a list of snapshot ids

    - *Values:* The input value is a list of snapshot ids separated by commas.

        - `.../results/?snapshot-ids=(5,12,3)`

        Note: the `snapshot-ids` and `snapshots` parameters are exclusive. You cannot use both in the same request.

    - *Value Modifiers:* None
    - *Default value:* None

- **select**

    - *Description:* Specify result options to be displayed

    - *Values:*

        - `violationRatio`

            - `.../results?quality-indicators=(quality-rules)&select=(violationRatio)`

            Must be used along with a Quality Indicator parameter specifying Quality Rules.
            ViolationRatio can apply to applications, modules and technologies.

        - `evolutionSummary`

            - `.../results?quality-indicators=(business-criteria,technical-criteria,quality-rules)&select=(evolutionSummary)`

            Must be used along with a Quality Indicator parameter specifying one or more Business Criteria, Technical Criteria or Quality Rules.
            EvolutionSummary can apply to applications, modules and technologies.

        - `categories`

            - `.../results?quality-indicators=(quality-distributions)&select=(categories)`

            Must be used along with a Quality Indicator parameter specifying one or more Quality Distributions.
            Categories can apply to applications, modules and technologies.

        - `aggregators`

            - `.../results?sizing-measures=(c:10202)&select=aggregators`

            Add gradeAggregators on each result. An aggregator is an indicator impacted by the result.

        - `improvementGap`

            - `.../results?quality-indicators=(quality-rules)&select=(improvementGap)`

            Must be used along with a Quality Indicator parameter specifying Quality Rules.

        - `omgTechnicalDebt`

            - `.../results?metrics=c:1062100&select=omgTechnicalDebt`

            Must be used along with a CISQ Quality Indicator (Business Criterion, Technical Criterion, or a rule).
            See also: the com.castsoftware.cisq-index documentation

    - *Value Modifiers:* None
    - *Default value:* None

- **unselect**

    - *Description:* Specify result options to be hidden

    - *Values:*

        - `grade`

            - `.../results?quality-indicators=(business-criteria)&unselect=(grade)`

            It prevents the grade parameter to be displayed for a Quality Indicator.
            It applies to applications, modules and technologies.

    - *Value Modifiers:* None
    - *Default value:* None

- **format**

    - *Description:* Specify output format options

    - *Values:*

        - `snapshotsAsRows`

            - `.../results?snapshots=(-2)&format=(snapshotsAsRows)`

            For CSV and Excel output, this displays the values for different snapshots on new rows instead of new columns (default behaviour).

    - *Value Modifiers:* None
    - *Default value:* None

- **order**

    - *Description:* Specify result sorting options

    - *Values:* Possible sorting parameters are:

        - `improvementGap`
        - `rule-pattern-name`
        - `rule-id`
        - `order=(asc(param1),desc(param2))`

    - *Default value:* `order=(asc(rule-id))`

- **name**

    - *Description:* Technology name

    - *Values:* Correct technology name present in selected domain or application or module or snapshot

        - `.../technology-results?name=JEE`

    - *Value Modifiers:* None
    - *Default value:* None


### Excel/CSV Representation (Output)

| Columns | Description | Type |
|---|---|---|
| Application Name | Application name | String |
| Module Name | Module name. If specified then assessment results are in the scope of this module | String |
| Technology | Technology name. If specified then assessment results are in the scope of this technology | String |
| Metric Name | Associated name of the metric key | Array |
| Snapshot Date #N | Date of the assessment result. Assessment results are ordered from the most recent (N=1) to the less recent (N > 1). Effective selected snapshots depend on the snapshot query parameter. | Date |
| Result #N | Assessment result as a grade for a quality indicator, or an integer for other results | Number |
| Violations (Result #N) | Number of failed checks when query parameter "select" is set with "violationRatio" | Number |
| Total Checks (Result #N) | Number of total checks when query parameter "select" is set with "violationRatio" | Number |
| Added Critical Violations (Result #N) | Number of added critical violations when query parameter "select" is set with "evolutionSummary" | Number |
| Critical Violations in New and Modified Code (Result #N) | Number of critical violations in new and modified code when query parameter "select" is set with "evolutionSummary" | Number |
| Removed Critical Violations (Result #N) | Number of removed critical violations when query parameter "select" is set with "evolutionSummary" | Number |
| Total Critical Violations (Result #N) | Number of total critical violations when query parameter "select" is set with "evolutionSummary" | Number |
| *CategoryName* (Result #N) | Result for a category of a distribution when query parameter "select" is set with "categories" | Number |

### CSV Representation (Input)

AAD (Application-Analytics-Dashboard) is the domain name for HD (Health Dashboard).

AED (Application-Engineering-Dashboard) is domain name for ED (Engineering Dashboard).

| Columns Titles | Type | Put Payload |
|---|---|---|
| ADG Database | String | If AAD domain, the `adgDatabase` property of the application. If AED domain, this value is optional. If set, it must match the `adgDatabase` property of the application. |
| Application Name | String | Name of the application to set Background Facts to |
| Module Name | String | Name of the application modules to set Background Facts to. For a given background fact id and application name, this column must contain each application module, plus the empty value (referring to the application itself) |
| Metric Id | Integer | A Background Fact id |
| Result | Double | Value of the Background Fact for an application or one of its modules |

### CSV Examples

**CSV payload for a AAD domain — Set Background Fact values for a single application**

```csv
ADG Database;Application Name;Module Name;Metric Id;Result
ADG_GACD;DIDD Diccionarios;;2666003;21
ADG_GACD;DIDD Diccionarios;DIDD_CAPP_ESQUEMAS;2666003;9
ADG_GACD;DIDD Diccionarios;DIDD_J2EE;2666003;12
ADG_GACD;DIDD Diccionarios;;66061;2.0
ADG_GACD;DIDD Diccionarios;DIDD_CAPP_ESQUEMAS;66061;3.0
ADG_GACD;DIDD Diccionarios;DIDD_J2EE;66061;4.0
```

**CSV payload for a AED domain — Set Background Fact values for multiple applications**

```csv
ADG Database;Application Name;Module Name;Metric Id;Result
;DIDD Diccionarios;;2666003;21
;DIDD Diccionarios;DIDD_CAPP_ESQUEMAS;2666003;9
;DIDD Diccionarios;DIDD_J2EE;2666003;12
;BO;;2666003;17
;BO;BO full content;2666003;17
;CastOld;;66061;4.83
;CastOld;Adg;66061;3.2
;CastOld;Central;66061;4.7
;CastOld;DssAdmin;66061;0.8
;CastOld;Pchit;66061;10.5
;DIDD Diccionarios;;66061;2.0
;DIDD Diccionarios;DIDD_CAPP_ESQUEMAS;66061;3.0
;DIDD Diccionarios;DIDD_J2EE;66061;4.0
```

### JSON Representations & JSON Examples

#### Result Item

| Properties | Description | Type | Occurs |
|---|---|---|---|
| number | Order number of the application snapshot | Integer | 1 |
| date | Date of the application snapshot | Date | 1 |
| application | Reference | URI | 1 |
| applicationSnapshot | Reference to get an application snapshot | URI | 1 |
| applicationResults | All results | Array | 1 |
| applicationResults[] | *See application result* | Structure | 0..* |

#### Application Result

##### Case of a Business Criterion

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is `"business-criterion"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.grade | Grade value between 1.0 and 4.0 | Double | 0..1 |
| result.boundaries | *Reserved* | Structure | 0..1 |
| result.evolutionSummary | See Evolution Summary structure | Structure | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Business Criterion | Structure | 1 |
| reference.href | Reference to a Business Criterion | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName |  | String | 1 |
| reference.key | | Integer | 1 |
| transactionResults | Results Breakdown by transaction | Array | 0..1 |
| transactionResults[] | See Transaction Result structure | Structure | 0..* |

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
        "result": {
          "grade": 3.04528,
          "boundaries": null,
          "evolutionSummary": null
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/60017/snapshots/5",
          "name": "Total Quality Index",
          "shortName": "TQI",
          "key": "60017",
          "gradeAggregators": null
        },
        "transactionResults": []
      }
    ]
  }
]
```

##### Case of a Technical Criterion

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is `"technical-criterion"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.grade | Grade value between 1.0 and 4.0 | Double | 0..1 |
| result.boundaries | *Reserved* | Structure | 0..1 |
| result.violationRatio | *Reserved* | Structure | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Quality Indicator | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |
| reference.gradeAggregators | Indicators computed with this result | Array | 1 |
| reference.gradeAggregators[].key | A parent Quality Indicator key | String | 0..* |
| reference.gradeAggregators[].weight | Contribution weight | Integer | 1 |
| reference.gradeAggregators[].criticity | `"N/A"` | String | 1 |
| transactionResults | Results Breakdown by transaction | Array | 0..1 |
| transactionResults[] | See Transaction Result structure | Structure | 0..* |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(61007)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "technical-criteria",
        "technologiesResults": [],
        "result": {
          "grade": 4,
          "boundaries": null,
          "violationRatio": null
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/61007/snapshots/5",
          "name": "Documentation - Bad Comments",
          "shortName": null,
          "key": "61007",
          "gradeAggregators": null
        },
        "transactionResults": []
      }
    ]
  }
]
```

##### Case of a Quality Rule

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is `"quality-rules"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.grade | Grade value between 1.0 and 4.0 | Double | 0..1 |
| result.boundaries | *Reserved* | Structure | 0..1 |
| result.violationRatio | See Violation Ratio Structure | Structure | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Quality Indicator | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |
| reference.gradeAggregators | Indicators computed with this result | Array | 1 |
| reference.gradeAggregators[].key | A parent Quality Indicator key | String | 0..* |
| reference.gradeAggregators[].weight | Contribution weight | Integer | 1 |
| reference.gradeAggregators[].criticity | Contribution criticity | Boolean | 1 |
| transactionResults | Results Breakdown by transaction | Array | 0..1 |
| transactionResults[] | See Transaction Result structure | Structure | 0..* |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(5080)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "quality-rules",
        "technologiesResults": [],
        "result": {
          "grade": 3.59457,
          "boundaries": null,
          "violationRatio": null
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/5080/snapshots/5",
          "name": "Avoid Procedure Paragraphs that contains no statements",
          "shortName": null,
          "key": "5080",
          "gradeAggregators": null
        },
        "transactionResults": []
      }
    ]
  }
]
```

##### Case of a Quality Distribution

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is `"quality-distributions"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.grade | Grade value between 1.0 and 4.0 | Double | 0..1 |
| result.boundaries | *Reserved* | Structure | 0..1 |
| result.categories | See Category Structure | Array | 1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Quality Indicator | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |
| reference.gradeAggregators | Indicators computed with this result | Array | 1 |
| reference.gradeAggregators[].key | A parent Quality Indicator key | String | 0..* |
| reference.gradeAggregators[].weight | Contribution weight | Integer | 1 |
| reference.gradeAggregators[].criticity | Contribution criticity | Boolean | 1 |

> Note that when retrieving Cost Complexity data the information you receive will vary:
> - If you are querying the Measurement Service, then only the categories High and Very High will be available.
> - If you are querying the Dashboard Service, then all categories (Low, Moderate, High and Very High) will be available.

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(65105)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "quality-distributions",
        "technologiesResults": [],
        "result": {
          "grade": 2.91687,
          "boundaries": null,
          "categories": null
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/65105/snapshots/5",
          "name": "Size Distribution",
          "shortName": null,
          "key": "65105",
          "gradeAggregators": null
        }
      }
    ]
  }
]
```

##### Case of a Quality Measure

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is `"quality-measures"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.grade | Grade value between 1.0 and 4.0 | Double | 0..1 |
| result.boundaries | See Boundaries Structure | Structure | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Quality Indicator | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |
| reference.gradeAggregators | Indicators computed with this result | Array | 1 |
| reference.gradeAggregators[].key | A parent Quality Indicator key | String | 0..* |
| reference.gradeAggregators[].weight | Contribution weight | Integer | 1 |
| reference.gradeAggregators[].criticity | Contribution criticity | Boolean | 1 |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(66067)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "quality-measures",
        "technologiesResults": [],
        "result": {
          "grade": 3.03494,
          "boundaries": null
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/66067/snapshots/5",
          "name": "Avoid High Volume of Copy Pasted Code",
          "shortName": null,
          "key": "66067",
          "gradeAggregators": null
        }
      }
    ]
  }
]
```

##### Case of a Sizing Measure

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | Value is among:<br/>`technical-size-measures`,<br/> `functional-weight-measures`,<br/> `critical-violation-statistics`, <br/>`technical-debt-statistics`, <br/>`run-time-statistics` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.value | Size | Double | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Sizing Measure | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results?sizing-measures=(10151)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "technical-size-measures",
        "technologiesResults": [],
        "result": { "value": 225727 },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/sizing-measures/10151/snapshots/5",
          "name": "Number of Code Lines",
          "shortName": "kLOCs",
          "key": "10151",
          "gradeAggregators": null
        }
      }
    ]
  }
]
```

##### Case of a Background Fact

| Properties | Description | Type | Occurs |
|---|---|---|---|
| type | `"background-fact"` | String | 1 |
| technologiesResults | Results Breakdown by technology | Array | 0..1 |
| technologiesResults[] | See Technology Result structure | Structure | 0..* |
| result | Application snapshot own result | Double | 0..1 |
| result.value | Fact | Double | 0..1 |
| modulesResults | Results Breakdown by module | Array | 0..1 |
| modulesResults[] | See Module Result structure | Structure | 0..* |
| reference | Reference to a Sizing Measure | Structure | 1 |
| reference.href | | URI | 1 |
| reference.name | | String | 1 |
| reference.shortName | | String | 1 |
| reference.key | | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results?background-facts=(66061)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "background-facts",
        "technologiesResults": [],
        "result": { "value": 1 },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/background-facts/66061/snapshots/5",
          "name": "Business Value",
          "shortName": "Biz Value",
          "key": "66061",
          "gradeAggregators": null
        }
      }
    ]
  }
]
```

### Optional Quality Indicator Result Detail

#### Violation Ratio (option coming with Quality Rules)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| violationRatio | For a quality rule, this ratio is an input value required to compute the grade according to 4 thresholds. | Structure | 0..1 |
| violationRatio<br/>.totalChecks | Total number of checked items in the current scope | Integer | 1 |
| violationRatio<br/>.failedChecks | Total number of failed items in the current scope | Integer | 1 |
| violationRatio<br/>.successfulChecks | Result of totalChecks - failedChecks | Integer | 1 |
| violationRatio<br/>.ratio | Compliance Ratio, i.e. successfulChecks / totalChecks | Double | 1 |
| violationRatio<br/>.violationOccurrences | Total number of violation occurrences (ex: number of bookmarks). Since AIP 8.3.33 | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(5080)&select=(violationRatio)**

```json
[
  {
    "number": 1,
    "date": { "time": 1338328800000 },
    "application": { "href": "DEMO/applications/6", "name": "Application 006" },
    "applicationSnapshot": { "href": "DEMO/applications/6/snapshots/5", "name": "Application 006" },
    "applicationResults": [
      {
        "type": "quality-rules",
        "technologiesResults": [],
        "result": {
          "grade": 3.59457,
          "boundaries": null,
          "violationRatio": {
            "totalChecks": 267,
            "failedChecks": 7,
            "successfulChecks": 260,
            "ratio": 0.9737827715355806
          }
        },
        "modulesResults": [],
        "reference": {
          "href": "DEMO/quality-indicators/5080/snapshots/5",
          "name": "Avoid Procedure Paragraphs that contains no statements",
          "shortName": null,
          "key": "5080",
          "gradeAggregators": null
        }
      }
    ]
  }
]
```

#### Evolution Summary (option coming with Business Criteria)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| evolutionSummary | Evolution summary between this snapshot and the previous one | Structure | 0..1 |
| evolutionSummary<br/>.removedCriticalViolations | Number of critical violations removed since the previous snapshot | Integer | 1 |
| evolutionSummary<br/>.addedCriticalViolations | Number of critical violations added since the previous snapshot | Integer | 1 |
| evolutionSummary<br/>.criticalViolationsInNewAndModifiedCode | Number of critical violations in new and modified code since the previous snapshot | Integer | 1 |
| evolutionSummary<br/>.totalCriticalViolations | Total number of critical violations | Integer | 1 |
| evolutionSummary<br/>.addedViolations | Number of violations added since the previous snapshot | Integer | 1 |
| evolutionSummary<br/>.removedViolations | Number of violations removed since the previous snapshot | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(60014)&select=(evolutionSummary)**

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
        "reference": {
          "href": "DEMO/quality-indicators/60014/snapshots/5",
          "name": "Efficiency",
          "shortName": "Effy",
          "key": "60014",
          "gradeAggregators": null
        },
        "result": {
          "grade": 2.67281,
          "evolutionSummary": {
            "criticalViolationsInNewAndModifiedCode": 5,
            "totalCriticalViolations": 5,
            "addedCriticalViolations": 5,
            "removedCriticalViolations": 0,
            "addedViolations": 13,
            "removedViolations": 280
          }
        },
        "technologyResults": [],
        "moduleResults": []
      }
    ]
  }
]
```

#### OMG Technical Debt (option coming with Business Criteria, Technical Criteria, Quality Rules)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| total | Total Technical Debt | Integer | 1 |
| numberOccurrences | Number of violation occurrences (number of bookmarks for example) | Integer | 1 |
| added | Technical Debt of added violations | Integer | 1 |
| removed | Technical Debt of removed violations | Integer | 1 |

**GET DEMO/applications/6/snapshots/5/results?quality-indicators=(8216)&select=(omgTechnicalDebt)**

```json
{
  "result": {
    "grade": 4,
    "omgTechnicalDebt": {
      "total": 11040,
      "numberOccurrences": 176,
      "added": 0,
      "removed": 0
    },
    "violationRatio": {
      "totalChecks": 7411,
      "failedChecks": 33,
      "successfulChecks": 7378,
      "ratio": 0.9955471596275807
    }
  }
}
```

#### Category (option coming with Quality Distributions)

| Properties | Description | Type | Occurs |
|---|---|---|---|
| categories | Categories results | Array | 1 |
| categories[] | A category result | Structure | 1..4 |
| categories[].key | A category id | String | 1 |
| categories[].name | A category name | String | 1 |
| categories[].value | A category measure | Integer | 1 |

**GET wow7010/applications/12/results?quality-indicators=(66010)&select=(categories)**

```json
[
  {
    "number": 4,
    "date": { "time": 1279144800000 },
    "application": { "href": "wow7010/applications/12", "name": "IFPUG" },
    "applicationSnapshot": { "href": "wow7010/applications/12/snapshots/15", "name": "IFPUG" },
    "applicationResults": [
      {
        "type": "quality-distributions",
        "reference": {
          "href": "wow7010/quality-indicators/66010/snapshots/15",
          "name": "Reuse by Call Distribution",
          "shortName": null,
          "key": "66010",
          "gradeAggregators": null
        },
        "result": {
          "grade": 1,
          "categories": [
            { "key": "66014", "name": "Low Reuse by Call", "value": 478 },
            { "key": "66013", "name": "Average Reuse by Call", "value": 17 },
            { "key": "66012", "name": "High Reuse by Call", "value": 3 },
            { "key": "66011", "name": "Very High Reuse by Call", "value": 1 }
          ],
          "boundaries": null
        },
        "technologyResults": [],
        "moduleResults": []
      }
    ]
  }
]
```

### Technology Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| technology | Technology name | String | 1 |
| result | See result structure | Structure | 1 |

### Module Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| module | Reference to a module snapshot | Structure | 1 |
| module.href | Module snapshot URI | URI | 1 |
| module.name | Module snapshot name | String | 1 |
| module.number | Module snapshot number | Integer | 1 |
| result | *See Result Item Structure* | Double | 0..1 |
| technologiesResults | *Results Breakdown by technology* | Array | 0..1 |
| technologiesResults[] | *See Technology Result Detail* | Structure | 0..* |

### Transaction Result Detail

| Properties | Description | Type | Occurs |
|---|---|---|---|
| transaction | Reference to a transaction snapshot | Structure | 1 |
| transaction.href | Transaction snapshot URI | URI | 1 |
| transaction.name | Transaction snapshot name | String | 1 |
| result | *See Result Item Structure* | Double | 0..1 |

---

## Quality Standards

### URI Templates & Parameters

- **GET** `{Domain}/quality-standards-categories/{QualityStandardCategory}?{parameters}` 

  - *Description*:

    Array of all quality standard references (aka tags) for a given category (OWASP-2017, STIG-V4R8-CAT1, etc.)
    
  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/applications/{ID}/quality-standards` 

  - *Description*:

    Array of quality standard references for an application. Only quality standard references with violations are reported.
    
  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/applications/{ID}/snapshots/{SnapshotID}/quality-standards` 

  - *Description*:

    Quality standard references for an application snapshot
    
  - *Media Type*:
    - `application/json`
    

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| standard | Quality Standard name | String | 1 |
| id | Reference name (aka tag) | String | 1 |
| name | Reference label | String | 1 |
| description | Reserved | String | 1 |
| applicable | Specify whether this Quality Standard is applicable to static code analysis. Some Security Standards recommendations may be out of the scope of static code analysis. | Boolean | 1 |
| contributors | List of rules associated to each Quality Standard | Structure | 0..* |

### Query Parameters

| Parameter | Description | Values | Default Value |
|---|---|---|---|
| select | Specify to return list of rules contributing to each Quality Standard. | `contributors` | N/A |

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

- **GET** `{Domain}/custom-quality-tags` 

  - *Description*:

    Download CUSTOM TAGS mapping. 
    
    Example: `curl --header "Accept: text/csv" http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags`
    
  - *Media Type*:
    - `text/csv`

- **PUT** `{Domain}/custom-quality-tags` 

  - *Description*:

    Add rule/CUSTOM TAGS mapping. If the mapping already exists, it is left unchanged. The tag must begin with the `CUSTOM` prefix. All new TAGS are automatically assigned to the `CUSTOM` standard.

    Example: `curl -X PUT --header "Content-type: text/csv" --upload-file data.csv http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags`
    
  - *Media Type*:
    - `text/csv`


- **DELETE** `{Domain}/custom-quality-tags` 

  - *Description*:

    Remove rule/CUSTOM TAGS mapping. The tag must begin with the `CUSTOM` prefix. All deleted TAGS are automatically removed from `CUSTOM` standard.

    Example: `curl -X DELETE --header "Content-type: text/csv" --upload-file data.csv http://localhost:8080/CAST-RESTAPI/rest/{Domain}/custom-quality-tags`
   
  - *Media Type*:
    - `text/csv`
    

### CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Rule ID | A rule ID | Integer | 1 |
| Tag | A label starting with `"CUSTOM"` | String | 1 |

### CSV Example

```csv
Rule ID;Tag
3626;CUSTOM-TOP-PRIORITY-RULES
```

---

## Background Facts Injection

### URI Templates 

- **PUT** `{Domain}/applications/{ApplicationID}/snapshots/${SnapshotID}/results`

  - *Description*:

    Update an array of Background Facts results for a given snapshot and a given application
    
  - *Media Type*:
    - `application/json`


### JSON Representation

A result item:

| Columns | Description | Type | Occurs |
|---|---|---|---|
| key | A background fact ID | String | 1 |
| result | A numeric value | Decimal | 1 |

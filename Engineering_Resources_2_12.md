# Engineering Resources - 2.12

*Created by James Hurrell on Apr 4, 2024*

> **Warning: Dashboard Service database only**

---

## On this page

- [Glossary](#glossary)
- [OMG Data Functions and Transactions (Excel/CSV Representation only)](#omg-data-functions-and-transactions-excelcsv-representation-only)
- [Transaction Directory](#transaction-directory)
- [Transaction](#transaction)
- [Tree Node](#tree-node)
- [Defects Summary](#defects-summary)
- [Violated Rule Pattern](#violated-rule-pattern)
- [Components Directory](#components-directory)
- [Component Snapshot](#component-snapshot)
- [Code Fragment](#code-fragment)
- [File Contents](#file-contents)
- [Action Plan Summary](#action-plan-summary)
- [Remedial Action](#remedial-action)
- [Issue](#issue)
- [Action Plan Trigger](#action-plan-trigger)
- [Action Plan Recommendation](#action-plan-recommendation)
- [Exclusion Request Detail](#exclusion-request-detail)
- [Exclusion Request](#exclusion-request)
- [Exclusion Summary](#exclusion-summary)
- [Scheduled Exclusion](#scheduled-exclusion)
- [Scheduled Exclusions Summary](#scheduled-exclusions-summary)
- [Violation](#violation)
- [Indexed Violation](#indexed-violation)
- [OMG Technical Debt](#omg-technical-debt)
- [Diagnosis Findings](#diagnosis-findings)
- [Search Results](#search-results)
- [List of extensions](#list-of-extensions)
- [Findings Report](#findings-report)

---

## Glossary

| Term | Definition |
|---|---|
| Action Plan | An action plan is a set of issues (i.e. violations requiring a remedial action) |
| Code Fragment | A code fragment identifies a file contents extract. |
| Component Snapshot | A component is a source code item such as a class, a method, etc. The effective definition depends on the programming language and the technology of the analyzed application. Container of a component is either an application or a module. A defective component is a component in violation with a quality rule pattern. |
| Defects Summary | Statistics of defects impacting a business criterion for all components in the scope of a tree node |
| Diagnosis Findings | Diagnosis Findings pinpoint statements or properties of the defective component violating a quality rule pattern. |
| Diagnosis Value | A diagnosis findings reported as a value. |
| File Contents | Raw text of a source file. |
| OMG Functions | Data Function or Transaction as results of Function Point analysis (see "International Function Point Users Group") |
| Issue | An issue reports a remedial action in a context of an application snapshot. Component and quality rule pattern match either a violation that should have been addressed in previous snapshots, or that will have to be addressed before next snapshot. |
| Remedial Action | A remedial action describes a user input request to correct a component regarding a quality rule-pattern. |
| Transaction | A set of components involved in a transaction processing |
| Tree Node | Hierarchy relation of a component. This hierarchy relation depends on the programming language. For example, a node for a "Java Package" component, will be parent node of "Java Class" nodes.<br/>**WARNING: a component may be reached from several tree nodes** |
| Violation | A violation identifies a defective component breaking a quality rule pattern.<br/>**IMPORTANT: For a given component and a given quality rule pattern there is 0 or 1 violation. If a component breaks a rule N times, then each occurrence is detailed into diagnosis findings structure with a value counter equals to N, and/or with N values, and/or with N code bookmarks.**<br/>A critical violation is a violation of a quality rule identified as critical regarding a technical criterion or a business criterion.<br/>Risk assessment is based on defective components number compared to observable components number. |

---

## OMG Data Functions and Transactions (Excel/CSV Representation only)

> **Warning: Even in case of a restricted license, `authorizations.xml` configuration is applied for these URLs.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions` | Array of Functions. |
| GET | text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions-evolution` | Array of Functions. |
| GET | text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/omg-functions-functional-evolution{?parameters}` | Array of Functions |

### Parameters

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| sheet | Specify the sheet you want to retrieve (only for CSV format). Possible values are: `sheet=context`, `sheet=functional`, `sheet=technical` | a string | none |

### CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Element Type | Whether this object is a Data Function or a Transaction | String | 1 |
| Object Name | Full name | String | 1 |
| Nb of FPs | Number of Function Points for this Function | Integer | 1 |
| FP details | Details on the Function Points for this Function | String | 1 |
| Object Type | Type of this Function | String | 1 |
| Module name | Name of a functional module containing this Function | String | 1 |
| Technology | Technology to which this Function belongs | String | 1 |
| Transaction Name | Full name of the transaction | String | 1 |
| Risk | Risk propagation assessment index according to an input health factor | Integer | 0..1 |

### Excel Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Element Type | Whether this object is a Data Function or a Transaction | String | 1 |
| Object Name | Full name | String | 1 |
| Nb of FPs | Number of Function Points for this Function | Integer | 1 |
| Impact Factor (for AFP or EFP) / Complexity Factor (for AEFP) | Multiplying factor for Adjusted FP — available for Transactions only since version 8.0, for Data Functions since version 8.1, for AEFP since version 8.2 | Float | 1 |
| Type | OMG Function type | String | 0..1 |
| DET | Data Element Type | Integer | 0..1 |
| RET | Record Element Type | Integer | 0..1 |
| EIF | External Interface File | Integer | 0..1 |
| ILF | Internal Logical File | Integer | 0..1 |
| FTR | File Type Referenced | Integer | 0..1 |
| Object Type | Type of this Function | String | 1 |
| Module name | Name of a functional module containing this Function | String | 1 |
| Technology | Technology to which this Function belongs | String | 1 |
| Transaction Name | Full name of the transaction | String | 1 |
| Risk | Risk propagation assessment index according to an input health factor | Integer | 0..1 |

---

## Transaction Directory

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/transactions` | A Transaction directory |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| security.href | Reference to get transactions by risk propagation index for Security | URI | 1 |
| efficiency.href | Reference to get transactions by risk propagation index for Efficiency | URI | 1 |
| robustness.href | Reference to get transactions by risk propagation index for Robustness | URI | 1 |
| shortName | Transaction short name | String | 1 |
| transactionRiskIndex | Risk assessment according to a health factor (if the business criterion is a health factor, this value is displayed, otherwise it is not) | Integer | 0 or 1 |

### JSON Example

```json
{
  "href": "DEMO/applications/3/snapshots/8/transactions",
  "name": "Transactions for application DEMO in snapshot 2",
  "security": {
    "href": "DEMO/applications/3/snapshots/8/transactions/60016",
    "name": "Transactions ordered by Risk Propagation Index for Security"
  },
  "efficiency": {
    "href": "DEMO/applications/3/snapshots/8/transactions/60014",
    "name": "Transactions ordered by Risk Propagation Index for Efficiency"
  },
  "robustness": {
    "href": "DEMO/applications/3/snapshots/8/transactions/60013",
    "name": "Transactions ordered by Risk Propagation Index for Robustness"
  }
}
```

```json
[
  {
    "href": "U692/transactions/532276/snapshots/6",
    "name": "canvasm.myo2.app_navigation.BaseNavDrawerActivity",
    "shortName": "BaseNavDrawerActivity",
    "transactionRiskIndex": 5333
  }
]
```

---

## Transaction

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/transactions/{BusinessCriterionId}/{?parameters}` | Array of Transactions |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/transactions{?parameters}` | Array of Transactions |
| GET | application/json | `{ApplicationId}/transactions/{TransactionID}/snapshots/{snapshotID}/components/{HealthFactorID}/violations-summary{?parameters}` | Array of Objects |
| GET | application/json | `{ApplicationId}/transactions/{TransactionID}/snapshots/{snapshotID}/components/{HealthFactorID}/object-violations-count{?parameters}` | Array of Objects |

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (not available for tree-node transactions) | an integer | 1 |
| nbRows | Specify max number of items to return or the `$all` keyword (not available for tree-node transactions) | an integer or the `$all` keyword | 10 |
| business-criterion | Specify a business criterion to compute TRI (only for tree-node transactions). Possible values: 60013, 60014, 60016 | an integer | 60017 (TQI) |
| status | Specify a status or more than one status to get filtered objects. Possible values: `added`, `updated`, `unchanged` | a string | All statuses |
| critical | `true` or `false`. e.g. `critical=(true)` means all critical Violations, `critical=(false)` means all Violations | Boolean | All Violations |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Reference to a transaction | URI | 1 |
| name | Transaction full name | String | 1 |
| shortName | Transaction short name | String | 1 |
| transactionRiskIndex | Risk assessment according to a health factor (if the business criterion is a health factor, this value is displayed, otherwise it is not) | int | 0 or 1 |
| `nbOfViolations` | Number of violations | int | 0 or 1 |
| treeNodes.href | A reference to get an array of all tree nodes linked to this component | URI | 0 or 1 |
| `totalObjects` | Total Number of objects | String or int | 0 or 1 |
| `addedObjects` | Number of added objects | String or int | 0 or 1 |
| `updatedObjects` | Number of updated objects | String or int | 0 or 1 |
| `unchangedObjects` | Number of unchanged objects | String or int | 0 or 1 |

### JSON Example

```json
[
  {
    "href": "DEMO/transactions/21/snapshots/8",
    "name": "[F:\\CASTMS\\TSTAEP82\\Deploy\\Shopizer\\shopizer_src\\sm-shop\\WebContent\\customer\\address.jsp]",
    "shortName": "address.jsp",
    "transactionRiskIndex": 1291
  },
  {
    "href": "DEMO/transactions/22/snapshots/8",
    "name": "[F:\\CASTMS\\TSTAEP82\\Deploy\\Shopizer\\shopizer_src\\sm-central\\WebContent\\cartproperties\\cartproperties.jsp]",
    "shortName": "cartproperties.jsp",
    "transactionRiskIndex": 851
  }
]
```

```json
{
  "href": "ENDTOEND84/components/30904/snapshots/14",
  "name": "[c:\\jenkins7_slave\\workspace\\CAIP_Trunk_TestE2E_CSS_ADG\\Work\\CAST\\Deploy\\Big Ben\\Presales\\PROG-BATCH].CSVAB481",
  "shortName": "CSVAB481",
  "status": "added",
  "nbOfViolations": 16,
  "treeNodes": {
    "href": "ENDTOEND84/components/30904/snapshots/14/tree-nodes",
    "name": "Tree Nodes"
  }
}
```

```json
{
  "href": "ENDTOEND84/transactions/14486/snapshots/14/components/60013/object-violations-count?status=added",
  "name": "Objects count associated with transaction",
  "totalObjects": null,
  "addedObjects": {
    "number": 4
  },
  "updatedObjects": null,
  "unchangedObjects": null
}
```

### Excel/CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Application Name | Application Name | String | 1 |
| Transaction Name | Full name of the transaction | String | 1 |
| Object Name | Full name | String | 1 |
| Business Criterion | An optional input health factor to filter violations. If this business criterion is a health factor, then a Propagated Risk Propagation Assessment Index is calculated for this Health Factor | String | 0..1 |
| Risk | Risk propagation assessment index according to an input health factor | Integer | 0..1 |
| Snapshot date | Input snapshot Date with YYYY-MM-DD format | String | 1 |

### Excel/CSV Representation (object violations count)

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Application Name | Application Name | String | 1 |
| Object name location | Full name of the defective component | String | 1 |
| Object name | Short Name | String | 1 |
| Object status | Component status compared to the previous snapshot: "added", "unchanged", "updated" | String | 1 |
| Violations | Total Number Of Violation for Objects | Integer | 1 |
| Snapshot date | Input snapshot Date with YYYY-MM-DD format | String | 1 |

---

## Tree Node

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/tree-node` | Tree-node attached to this application. This is the root of all tree nodes. |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/tree-node` | Tree-node attached to this module. |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}` | Tree-node detail |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/children/{?parameters}` | Array of direct tree node children for TQI by default. Tree nodes are ordered by names. |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/ancestors` | Array of all tree node ancestors. Tree Nodes are ordered by descending node levels |
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/tree-nodes` | Array of tree-nodes related to this component as a component may be reached from several tree nodes. |

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item | an integer | 1 |
| nbRows | Specify max number of items to return | an integer | 10 |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| component | Related component. There is no component for a tree-node attached to an application or a module. | Structure | 0..1 |
| component.name | Related component name. There is no component for a tree-node attached to an application or a module. | URI | 1 |
| component.type.name | Component type name. This name can be used as a file name of a PNG image. | String | 1 |
| component.type.label | Component type label: "C++ Class", "Java Class", "Java Method" | String | 1 |
| defects-summary.href | URI to get statistics of defects: defective components, violated rule patterns, violations | URI | 1 |
| children.href | URI to get all direct children nodes. Warning: Undefined for leaf nodes. | URI | 0..1 |
| ancestors.href | URI to get all parents of this tree node. Warning: Undefined for the root tree-node which is the application. | URI | 0..1 |

### JSON Example

```json
{
  "href": "CASTONCAST/tree-nodes/3-63888-63923/snapshots/2",
  "name": "AMTActionsImpl::CAMTTestDomifier",
  "component": {
    "name": "[S:\\CAIP\\SRC\\...[CAMTTestDomifier]",
    "type": {
      "name": "C_CLASS",
      "label": "C++ Class"
    },
    "sourceCodes": {
      "href": "CASTONCAST/components/231310/snapshots/2/source-codes",
      "name": "Source Codes"
    }
  },
  "defectsSummary": {
    "href": "CASTONCAST/tree-nodes/3-63888-63923/snapshots/2/defects-summary",
    "name": "Defects"
  },
  "children": {
    "href": "CASTONCAST/tree-nodes/3-63888-63923/snapshots/2/children",
    "name": "Children"
  },
  "ancestors": {
    "href": "CASTONCAST/tree-nodes/3-63888-63923/snapshots/2/ancestors",
    "name": "Ancestors"
  }
}
```

```json
{
   "href":"ENDTOEND83/tree-nodes/4-3231-3234/snapshots/16",
   "name":"ListTitles.jsp",
   "component":{
      "href":"ENDTOEND83/components/36382/snapshots/16",
      "name":"[c:\\jenkins6_slave\\workspace\\CAIP_Trunk_TestE2E_CSS_ADG\\Work\\CAST\\Deploy\\Jurassic Park\\JSPBookDemo\\ListTitles.jsp]",
      "shortName":"ListTitles.jsp",
      "type":{
         "label":"eFile",
         "name":"CAST_Web_File"
      },
      "sourceCodes":{
         "href":"ENDTOEND83/components/36382/snapshots/16/source-codes",
         "name":"Source Codes"
      },
      "treeNodes":{
         "href":"ENDTOEND83/components/36382/snapshots/16/tree-nodes",
         "name":"Tree Nodes"
      },
      "codeLines":81,
      "commentedCodeLines":null,
      "commentLines":0,
      "coupling":1,
      "fanIn":1,
      "fanOut":17,
      "cyclomaticComplexity":4,
      "ratioCommentLinesCodeLines":0,
      "halsteadProgramLength":126,
      "halsteadProgramVocabulary":40,
      "halsteadVolume":464.798811218356,
      "distinctOperators":12,
      "distinctOperands":28,
      "integrationComplexity":4,
      "essentialComplexity":1
   },
   "defectsSummary":{
      "href":"ENDTOEND83/tree-nodes/4-3231-3234/snapshots/16/defects-summary",
      "name":"Defects"
   },
   "children":{
      "href":"ENDTOEND83/tree-nodes/4-3231-3234/snapshots/16/children",
      "name":"Children"
   },
   "ancestors":{
      "href":"ENDTOEND83/tree-nodes/4-3231-3234/snapshots/16/ancestors",
      "name":"Ancestors"
   }
}
```

---

## Defects Summary

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/defects-summary{?parameters}` | Summary of defects |

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| business-criterion | Business Criterion to compute statistics | An integer | 60017 (TQI) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| violations.number | Number of violations impacting a business criterion | Integer | 1 |
| defectiveComponents.number | Number of defective components impacting a business criterion | Integer | 1 |
| violatedRulePatterns.href | URI to get list of violated rule patterns in the scope of this node | URI | 1 |
| violatedRulePatterns.number | Number of violated rules patterns impacting a business criterion. For each rule-pattern, see "violation" resource to get list of violations. | Integer | 1 |
| criticalViolations.number | Number of critical violations impacting a business criterion | Integer | 1 |
| defectiveComponentsToCriticalRules.number | Number of defective components impacting the critical rules of a business criterion | Integer | 1 |
| violatedCriticalRulePatterns.number | Number of critical violated rules patterns impacting a business criterion | Integer | 1 |

### JSON Example

```json
{
  "href": "DEMO/tree-nodes/1-2-1827/snapshots/28/defects-summary?business-criterion=60017",
  "name": "eCommerce DataAccess",
  "violations": { "number": 9164 },
  "defectiveComponents": { "number": 3501 },
  "violatedRulePatterns": {
    "name": "Violated Rule Patterns",
    "href": "DEMO/tree-nodes/1-2-1827/snapshots/28/violated-rule-patterns?business-criterion=60017",
    "number": 227
  },
  "criticalViolations": { "number": 53 },
  "defectiveComponentsToCriticalRules": { "number": 256 },
  "violatedCriticalRulePatterns": { "number": 11 }
}
```

---

## Violated Rule Pattern

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violated-rule-patterns{?parameters}` | An array of violated rule patterns ordered by violations number |

| URI Parameters | Description | Values | Default value |
|---|---|---|---|
| business-criterion | Specify a business criterion to select rule patterns which associated Quality Rule indicator is contributing to a business criterion | an integer | 60017 (TQI) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern.href | Auto reference | URI | 1 |
| violations.name | Name | String | 1 |
| violations.href | Reference to get the list of violations | URI | 1 |
| violations.number | Number of violations/defective components for this rule pattern | Integer | 1 |

### JSON Example

```json
{
  "rulePattern": {
    "href": "DEMO/rule-patterns/554"
  },
  "violations": {
    "name": "Violations",
    "href": "DEMO/tree-nodes/0-1-64400/snapshots/28/violations?rule-pattern=554&business-criterion=60017",
    "number": 19579
  }
}
```

### Excel Example

| Object Name | Business criterion | Metric Name | Metric Id | Weight | Critical | Snapshot Date | Violations |
|---|---|---|---|---|---|---|---|
| IFPUG | Total Quality Index | Declare as Static all methods not using instance members | 7270 | 8 | true | 2013-6-21 | 430 |

---

## Components Directory

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components` | A Components directory |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components` | A Components directory |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| security.href | Reference to get components ordered by risk propagation index for Security | URI | 1 |
| efficiency.href | Reference to get components ordered by risk propagation index for Efficiency | URI | 1 |
| robustness.href | Reference to get components ordered by risk propagation index for Robustness | URI | 1 |
| transferability.href | Reference to get components ordered by risk propagation index for Transferability | URI | 1 |
| changeability.href | Reference to get components ordered by risk propagation index for Changeability | URI | 1 |

### JSON Example

```json
{
  "href": "DEMO/applications/3/snapshots/8/components",
  "name": "Components for object DEMO in snapshot 2",
  "security": {
    "href": "DEMO/applications/3/snapshots/8/components/60016",
    "name": "Components ordered by Risk Propagation Index for Security"
  },
  "efficiency": {
    "href": "DEMO/applications/3/snapshots/8/components/60014",
    "name": "Components ordered by Risk Propagation Index for Efficiency"
  },
  "robustness": {
    "href": "DEMO/applications/3/snapshots/8/components/60013",
    "name": "Components ordered by Risk Propagation Index for Robustness"
  },
  "transferability": {
    "href": "DEMO/applications/3/snapshots/8/components/60011",
    "name": "Components ordered by Risk Propagation Index for Transferability"
  },
  "changeability": {
    "href": "DEMO/applications/3/snapshots/8/components/60012",
    "name": "Components ordered by Risk Propagation Index for Changeability"
  }
}
```

---

## Component Snapshot

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` | Array of Components of an application snapshot filtered by Health Factor |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` | Array of Components of a module snapshot filtered by Health Factor |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{DistributionID}/{categoryRank}/{?parameters}` | Array of Components of a distribution for an application snapshot by Health Factor |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components/{DistributionID}/{categoryRank}/{?parameters}` | Array of Components of a distribution for module snapshot by Health Factor |
| GET | application/json | `{Domain}/applications/{ApplicationID}/components/65005/{?parameters}` | Array of Components for Cost Complexity |
| GET | application/json | `{Domain}/modules/{ModuleID}/components/65005/{?parameters}` | Array of Components for Cost Complexity |
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}` | A component snapshot |

#### Common parameters

| URI Parameters | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item | an integer | 1 |
| nbRows | Specify max number of items to return | an integer | 10 |
| business-criterion | A health factor ID to compute propagated risk index | String | None |

#### Additional parameters for `{HealthFactorID}` URLs

| URI Parameters | Description | Values | Default value |
|---|---|---|---|
| properties | List of component properties which should be non-null to filter the components. e.g. `properties=(cyclomaticComplexity,fanOut)`. Available properties: `codeLines`, `commentedCodeLines`, `commentLines`, `coupling`, `fanIn`, `fanOut`, `cyclomaticComplexity`, `ratioCommentLinesCodeLines`, `halsteadProgramLength`, `halsteadProgramVocabulary`, `halsteadVolume`, `distinctOperators`, `distinctOperands`, `integrationComplexity`, `essentialComplexity`. Their values are known only for the last snapshot. | a list of predefined strings | None |
| order | By default, components are ordered by PRI desc (if any), component full name asc, component id asc. The sortable columns are all the above properties, plus: `pri`, `component-name`, `component-id` | a list of sort orders | None |

#### Only parameters applicable to Cost Complexity (`65005`) URLs

| URI Parameters | Description | Values | Default value |
|---|---|---|---|
| snapshot-ids | Two snapshot ids (snapshot range to consider). e.g. `snapshot-ids=(12,14)` | Two comma separated integers | none |
| status | Status of the artifacts between snapshots. Possible values: `added`, `updated`, `deleted` | String | none |
| technologies | A technology name to filter artifacts | String | none |

### JSON Representation

| Properties | Description | Scope | Type | Occurs |
|---|---|---|---|---|
| href | Auto reference | n/a | URI | 1 |
| name | Full name of the component. | n/a | String | 1 |
| shortName | Short name of the component | n/a | String | 1 |
| propagationRiskIndex | Risk propagation assessment index according to an input health factor. This attribute is not set if health factor is not specified | Enabled if a business criterion is specified | Integer | 0..1 |
| status | An enumeration value of ["added", "updated", "unchanged"] | n/a | String | 0..1 |
| sourceCodes.href | A reference to get an array of code fragments. Each code fragment is a part of the source code. This data is set only for the last snapshot of this application | Enabled for last Application Snapshot only | URI | 0..1 |
| type.label | Component type label: "C++ Class", "Java Class", "Java Method" | n/a | String | 0..1 |
| type.name | Component type name. This name can be used as a file name of a PNG image. | n/a | String | 0..1 |
| treeNodes.href | A reference to get an array of all tree nodes linked to this component | Enabled for last snapshot only | URI | 0..1 |

### Additional properties

(available through the `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}` URL only, and the "component" parameter of `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}` URL only)

| Properties | Description | Scope | Type | Occurs |
|---|---|---|---|---|
| codeLines | Number of code lines | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| commentedCodeLines | Number of commented code lines | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| commentLines | Number of comment lines | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| coupling | Coupling | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| fanIn | Number of ingoing calls/references | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| fanOut | Number of outgoing calls/references | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| cyclomaticComplexity | Cyclomatic Complexity | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| ratioCommentLinesCodeLines | The total number of comments divided by the total number of code lines | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Double | 0..1 |
| halsteadProgramLength | Program length as the sum of number of operands and number of operators | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| halsteadProgramVocabulary | Sum of distinctOperators and distinctOperands | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| halsteadVolume | The volume as `programLength * log2(distinctOperands + distinctOperators)` | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Double | 0..1 |
| distinctOperators | Number of distinct operators | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| distinctOperands | Number of distinct operands | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| integrationComplexity | Integration Complexity measures the number of independent integration paths | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |
| essentialComplexity | Essential Complexity measures the number of non-structured independent paths | Enabled for last snapshot only, on requesting a single ComponentSnapshot | Integer | 0..1 |

### JSON Example

```json
{
  "href": "CENTRAL/components/4/snapshots/5",
  "name": "com.cast.monster_event.base.model.Monster.Monster",
  "shortName": "Monster",
  "status": "added",
  "sourceCodes": {
    "href": "CENTRAL/components/4/snapshots/5/source-codes",
    "name": "Source codes"
  },
  "treeNodes": {
    "href": "CENTRAL/components/4/snapshots/5/tree-nodes",
    "name": "Tree nodes"
  }
}
```

---

## Code Fragment

*No URI — a code fragment is always embedded in a parent structure.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| file.href | URI to get source file contents as a text using the related media-type | URI | 1 |
| file.name | path/name of the file | String | 1 |
| file.size | size of the file (number of octets) | Integer | 1 |
| startLine | start line of code fragment | Integer | 1 |
| startColumn | start column of code fragment. Note: column number may be set to null in some cases, for example for a diagnosis findings of type "path" | Integer | 1 |
| endLine | end line of code fragment | Integer | 1 |
| endColumn | end column of code fragment. Note: column number may be set to null in some cases. Note 2: this value is exclusive — code fragment ends at column (endColumn-1). This convention allows to locate a point in the text. | Integer | 1 |

### JSON Example

```json
{
  "file": {
    "href": "CENTRAL/local-sites/1/file-contents/123",
    "name": "test.java",
    "size": 15
  },
  "startLine": 6,
  "startColumn": 1,
  "endLine": 6,
  "endColumn": 10
}
```

---

## File Contents

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | text/plain | `{Domain}/local-sites/{SiteID}/file-contents/{ContentsID}/?{Parameters}` | Source file contents as a text |

| URI Parameters | Description | Values | Default value |
|---|---|---|---|
| start-line | Specify first line to extract | an integer | 1 |
| end-line | Specify last line to extract | an integer | Last line no |

### Text/Plain Representation

A raw text.

---

## Action Plan Summary

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/summary/{?parameters}` | Action plan summary for this application at this snapshot |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/action-plan/summary/{?parameters}` | Action plan summary for this module at this snapshot |

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| objectSamples | Specify a number of sample objects to display | An integer | 5 |
| rule-pattern | Filter the issues following the specified quality rules. The common syntax for this parameter can be found in the Violation section | a combination of integers and strings, e.g. `rule-pattern=(7156,8294)` | All quality rules |
| status | Filter the issues following the specified statuses. Possible values: `added`, `pending`, `solved` | e.g. `status=(added,pending)` | All statuses |
| tag | Filter the issues following the specified tags (an issue is returned if the tag equals the specified value) | e.g. `tag=(low,high)` | All tags |
| comment | Filter the issues following the specified comment (an issue is returned if the comment equals the specified value) | e.g. `comment=()` (blank comment), `comment=(test)`, `comment=(,test)` (blank or test) | All comments |
| object-fullname | Filter the issues following the specified object full name (an issue is returned if the object full name contains the specified value) | a string | All object full names |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern.href | Reference to get a rule pattern description | URI | 1 |
| objectSamples | A sample of defective components | Array | 1 |
| objectSamples[] | A defective component | Structure | 1..* |
| addedIssues | Number of issues added to the action plan in the requested snapshot | Integer | 1 |
| pendingIssues | Number of issues selected before the requested snapshot and remaining in the action plan | Integer | 1 |
| solvedIssues | Number of issues selected before the requested snapshot that have been corrected in the meantime | Integer | 1 |

### JSON Example

```json
[
  {
    "rulePattern": {
      "href": "DEMO/rule-patterns/1003114",
      "name": "Avoid class with too many fields"
    },
    "objectSamples": [
      { "name": "S:\\...MatchFilter" },
      { "name": "S:\\...UtilityManager" },
      { "name": "S:\\...SummaryGrid" },
      { "name": "S:\\...ParameterConstants" },
      { "name": "S:\\..." }
    ],
    "addedIssues": 283,
    "pendingIssues": 8,
    "solvedIssues": 1
  }
]
```

---

## Remedial Action

*No URI — a remedial action is always embedded in an issue or a violation.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | This property is set only in the scope of an issue or a violation. Computed with internal dates compared to the current snapshot date: **"added"**: A remedial action has been requested at this violation snapshot date; **"pending"**: A remedial action has been requested prior to this snapshot, violation is still an issue; **"solved"**: This violation has been solved and does no longer exist in this snapshot | String | 1 |
| dates.updated | Date of last change of this remedial action | Date | 1 |
| dates.solved | Date when action plan violation solved | Date | 0..1 |
| comment | A short comment provided by the requester who added manually a remedial action. End of lines are represented with characters `\r\n` | String | 1 |
| priority | Remediation priority: `extreme`, `high`, `moderate`, `low` | String | 1 |
| tag | A general text intended to replace priority | String | 1 |

### JSON Example

```json
{
  "dates": { "updated": { "time": "..." } },
  "comment": "...",
  "priority": "high"
}
```

---

## Issue

An issue represents a remedial action for a rule pattern and a component in a context of an application snapshot.

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}` | Array of issues applicable until this application snapshot. "status" property of remedial action indicates the violation status regarding this action. |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}` | Array of issues applicable until this module snapshot. |
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Create a set of issues from last application snapshot. Users MUST have `QUALITY_MANAGER` Role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Update a set of issues opened in last application snapshot. Users MUST have `QUALITY_MANAGER` Role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Remove a set of issues. Users MUST have `QUALITY_MANAGER` Role. |

**POST/PUT Payload structure:**

```json
[
  {
    "component": { "href": "D/components/C/snapshots/S" },
    "rulePattern": { "href": "D/rule-patterns/M" },
    "remedialAction": { "comment": "comment text", "tag": "mytag" }
  }
]
```

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |
| rule-pattern | Filter the issues following the specified quality rules | a combination of integers and strings, e.g. `rule-pattern=(7156,8294)` | All quality rules |
| status | Filter the issues following the specified statuses. Possible values: `added`, `pending`, `solved` | e.g. `status=(added,pending)` | All statuses |
| tag | Filter the issues following the specified tags | e.g. `tag=(low,high)` | All tags |
| comment | Filter the issues following the specified comment | e.g. `comment=(test)` | All comments |
| object-fullname | Filter the issues following the specified object full name (contains) | a string | All object full names |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Reference to an issue | URI | 1 |
| component | Defective component snapshot violating a quality rule pattern | Structure | 1 |
| rulePattern.href | Quality Rule Pattern to comply | URI | 1 |
| remedialAction | Remedial Action | Structure | 0..1 |

### JSON Example

```json
{
  "remedialAction": {
    "dates": { "updated": { "time": "..." } },
    "status": "added",
    "comment": "...",
    "priority": "high",
    "tag": "to exclude"
  },
  "component": {
    "href": "DEMO/components/858321/snapshots/12",
    "name": "com.cast.monster_event.base.model.Monster.Monster"
  },
  "rulePattern": {
    "href": "CENTRAL/rule-patterns/4672",
    "name": "Methods must have appropriate JavaDoc @param tags"
  }
}
```

### Excel/CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Action plan priority | Remediation priority: `extreme`, `high`, `moderate`, `low` | String | 1 |
| Action plan tag | A general text intended to replace priority | String | 1 |
| Action plan status | **"added"**: issue requested at this snapshot date; **"pending"**: issue prior to this snapshot still to solve; **"solved"**: issue has been corrected | String | 1 |
| Action plan comment | Remedial action comment | String | 1 |
| Quality rule name | Rule pattern name | String | 1 |
| Object name location | Full name of the defective component | String | 1 |
| Action plan last update | Date of last update in the action plan | Date | 1 |
| Metric Id | Quality Rule Id | Integer | 1 |

---

## Action Plan Trigger

An Action Plan Trigger is composed of a rule pattern and a remedial action. If active, all the violations of the corresponding rule will be automatically put in Action Plan in the next snapshot.

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/triggers{?Parameters}` | Array of action plan triggers applicable in the next snapshot |
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Create a set of action plan triggers. Users MUST have `QUALITY_AUTOMATION_MANAGER` Role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Update a set of action plan triggers. Users MUST have `QUALITY_AUTOMATION_MANAGER` Role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Remove a set of action plan triggers. Users MUST have `QUALITY_AUTOMATION_MANAGER` Role. |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| rule-pattern | Filter the action plan triggers following the specified quality rules. The common syntax for this parameter can be found in the Violation section | a combination of integers and strings | All quality rules |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern | Rule Pattern | Structure | 1 |
| remedialActionPattern | Remedial Action Pattern. Whether the input has been "comment + priority" or "comment + tag", the JSON will contain `{"priority": "value", "tag": null}` or `{"priority": null, "tag": "value"}`. | Structure | 1 |
| active | Whether the Action Plan Trigger is currently active or not | Boolean | 1 |

### JSON Example

```json
{
  "rulePattern": {
    "href": "ADG73/rule-patterns/7424",
    "name": "Avoid using SQL queries inside a loop"
  },
  "remedialActionPattern": {
    "priority": "high",
    "tag": null,
    "comment": "DBA - sprint 24",
    "dates": {
      "updated": {
        "time": 1411730470000,
        "isoDate": "2014-09-26"
      }
    }
  },
  "active": true
}
```

---

## Action Plan Recommendation

### URI Templates to automatically get an action plan recommendation

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/action-plan-recommendation?{parameters}` | Calculate a recommendation action plan in order to reach a target for an application. Users MUST have `QUALITY_MANAGER` Role. Returns an HTTP Error if the target is already reached. |
| GET | application/json | `{Domain}/applications/{ApplicationID}/action-plan-recommendation?{parameters}` | Calculate a recommendation action plan for a specific application. Users MUST have `QUALITY_MANAGER` Role. |
| PUT | application/json | `{Domain}/action-plan-recommendation` | Calculate a recommendation action plan and create automatically the action plan issues. Users MUST have `QUALITY_MANAGER` Role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan-recommendation` | Same for a specific application. |

**Parameters (GET as query params / PUT as payload)**

| Payload | URI Parameter | Description | Type | Default value |
|---|---|---|---|---|
| qualityIndicator | quality-indicator | A business criterion ID to improve. | Integer | 60017 (Total Quality Index) |
| scoreType | score-type | The score type to improve: `"grade"` (1.0–4.0) or `"compliance"` (0.0–1.0) | String | grade |
| goal | goal | The goal: `"reach-score-minimizing-violations"`, `"reach-score-minimizing-effort"`, `"reach-violations-count-maximizing-score"`, `"reach-total-effort-maximizing-score"` | String | reach-score-minimizing-violations |
| target | target | A target depending on the goal: grade (1.0–4.0), compliance (0.0–1.0), number of violations, or effort in minutes. | Decimal | 4.0 for a grade; 1.0 for compliance; 1 for violations; 480 for effort |
| application | application-name | Application name for the first URI template. | String | Default if single application |
| append | append | `false` = ignore current action plan; `true` = include current action plan. **WARNING**: PUT with `append=true` removes all current issues first. | Boolean | false |

**Payload Example**

```json
{
  "target": 3.4,
  "append": true
}
```

### JSON Representation of GET Response

| Properties | Description | Type | Occurs |
|---|---|---|---|
| title | Description added to the action plan issues | Integer | 0..1 |
| goal | The goal of this action plan | String | 0..1 |
| qualityIndicator | The quality indicator to improve | Structure | 1 |
| qualityIndicator.key | The ID of the quality indicator to improve | Integer | 1 |
| qualityIndicator.name | The name of the quality indicator to improve | String | 1 |
| qualityIndicator.shortName | The short name of the quality indicator to improve | String | 1 |
| qualityIndicator.href | Quality Indicator URI | String | 1 |
| scoreType | The score type to improve (grade or compliance) | String | 0..1 |
| score | The target score (compliance or grade) when violations will be fixed | Decimal | 1 |
| totalViolations | The number of violations to fix | Integer | 1 |
| totalEffort | The remediation effort to fix these violations | Decimal | 1 |
| actionPlan | The action plan as a list of remediations | Array | 1 |
| actionPlan[] | Details of a rule remediation | Structure | 0..* |
| actionPlan[].qualityRule | Quality Rule | Structure | 1 |
| actionPlan[].qualityRule.key | Quality Rule ID | Integer | 1 |
| actionPlan[].qualityRule.name | Quality Rule name | String | 1 |
| actionPlan[].qualityRule.href | Quality Rule URI | String | 1 |
| actionPlan[].id | The rule ID | Integer | 1 |
| actionPlan[].name | The rule name | String | 1 |
| actionPlan[].critical | True if the rule contributes to the technical criterion as a critical rule (relevant only when scoreType is "grade") | Boolean | 1 |
| actionPlan[].violationsCount | The number of violations to fix | Integer | 1 |
| actionPlan[].violationOccurrences | The number of violation occurrences to fix (number of findings) | Integer | 1 |
| actionPlan[].unitEffort | The effort to fix a violation occurrence (in minutes) | Integer | 1 |
| actionPlan[].effort | The total effort for this rule (in minutes) | Integer | 1 |
| actionPlan[].components | The list of components | Structure | 0..* |
| actionPlan[].components[].name | Full name of the component | String | 1 |
| actionPlan[].components[].shortName | Short name of the component | String | 1 |
| actionPlan[].components[].type | Type of the component | Structure | 1 |
| actionPlan[].components[].type.name | Type code name | String | 1 |
| actionPlan[].components[].type.label | Readable type name | String | 1 |

### URI Templates to inject recommended remediations from a client

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues/recommendations` | Add action plan issues from a payload of remediations. Domain must be an Engineering domain. Users MUST have `QUALITY_MANAGER` Role. |

### JSON Representation of Payload of Remediations

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern | Rule Pattern | Structure | 1 |
| remedialAction | Remedial Action Pattern | Structure | 1 |
| number | Number of violations to add to action plan | Integer | 1 |

### Example

```json
{
  "rulePattern": { "href": "AED13/rule-patterns/7438" },
  "remedialAction": { "tag": "high", "comment": "Recommended action plan for Total Quality Index score improvement from 2.67 to 2.98" },
  "number": 3
}
```

---

## Exclusion Request Detail

*No URI — an exclusion request detail is always embedded in an exclusion request or a violation.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | **added**: a newly created exclusion request; **processed**: an exclusion request that has been processed; **unbound**: an exclusion request has been defined but the violation does not exist any more | String | 1 |
| userName | User name who created/updated the exclusion request | String | 1 |
| comment | A comment (e.g. "False Positive") | String | 1 |
| dates.updated | Creation date or date of last update | Date | 1 |

### JSON Example

```json
"exclusionRequest": {
  "status": "added",
  "userName": "DCA",
  "comment": "False positive",
  "dates": {
    "updated": {
      "time": 1497279600000,
      "isoDate": "2017-06-12"
    }
  }
}
```

---

## Exclusion Request

An exclusion request represents the request to exclude a violation in the future (after reconsolidation or new snapshot).

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| POST | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Create a collection of Exclusion requests (comment is required). Users MUST have `EXCLUSION_MANAGER` Role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Update comments of a collection of Exclusion requests. Users MUST have `EXCLUSION_MANAGER` Role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Delete a collection of Exclusion requests. Users MUST have `EXCLUSION_MANAGER` Role. |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| component | Defective component snapshot violating a quality rule pattern | Structure | 1 |
| rulePattern.href | Quality Rule Pattern to comply | URI | 1 |
| exclusionRequest | Detail of the exclusion request | Structure | 1 |
| exclusionRequest.dates.updated | Creation date or date of last update | Date | 1 |
| exclusionRequest.comment | A comment (e.g. "False Positive") | String | 0..1 |
| exclusionRequest.userName | User name who created/updated the exclusion request | String | 1 |
| exclusionRequest.status | **added**: newly created; **processed**: has been processed; **unbound**: violation no longer exists | String | 1 |

### JSON GET Example

```json
{
  "component": {
    "href": "ENDTOEND83/components/19517/snapshots/15",
    "name": "com.castsoftware.util.data.Criterion",
    "shortName": "Criterion",
    "treeNodes": {
      "href": "ENDTOEND83/components/19517/snapshots/15/tree-nodes",
      "name": "Tree Nodes"
    }
  },
  "rulePattern": {
    "href": "ENDTOEND83/rule-patterns/4554",
    "name": "Avoid large Classes - too many Methods"
  },
  "exclusionRequest": {
    "status": "added",
    "userName": "DCA",
    "comment": "Num metric, Single AV, Violation in last snapshot (15)",
    "dates": {
      "updated": {
        "time": 1497279600000,
        "isoDate": "2017-06-12"
      }
    }
  }
}
```

### JSON POST/PUT/DELETE Input Example (Request Payload)

```json
[
  {
    "rulePattern": { "href": "ENDTOEND83/rule-patterns/4672" },
    "component": { "href": "ENDTOEND83/components/18568/snapshots/15" },
    "exclusionRequest": { "comment": "to exclude" }
  },
  {
    "rulePattern": { "href": "ENDTOEND83/rule-patterns/7846" },
    "component": { "href": "ENDTOEND83/components/21514/snapshots/15" },
    "exclusionRequest": { "comment": "false positive" }
  }
]
```

---

## Exclusion Summary

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/excluded-violations-summary` | Exclusion summary for this application at this snapshot |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| rule-pattern | Filter the excluded violations summary following the specified quality rules. The common syntax for this parameter can be found in the Violation section | a combination of integers and strings | All quality rules |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| excludedViolations.number | Number of excluded violations in this application snapshot | Integer | 1 |

### JSON Example

```json
{
  "href": "ENDTOEND83/applications/3/snapshots/15/excluded-violations-summary",
  "name": "Excluded Violations Summary",
  "excludedViolations": {
    "number": 25
  }
}
```

---

## Scheduled Exclusion

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/exclusions/scheduled{?Parameters}` | Scheduled exclusions for this application at this snapshot |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| status | Status of the scheduled exclusion: `to-add` = violations to exclude at the next snapshot; `to-remove` = exclusions to de-exclude; `$all` = all regardless of status | `to-add`, `to-remove`, `$all` | `$all` |
| rule-pattern | Filter the scheduled exclusions following the specified quality rules | a combination of integers and strings | All quality rules |
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| component | Defective component snapshot violating a quality rule pattern | Structure | 1 |
| rulePattern.href | Quality Rule Pattern to comply | URI | 1 |
| exclusionRequest | An exclusion request. This property is exclusive with "remedialAction" property | Structure | 0..1 |
| remedialAction | A remediation action request. This property is exclusive with "exclusion" property | Structure | 0..1 |

### CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Exclusion status | Status of the scheduled exclusion: "to add" or "to remove" | String | 1 |
| Exclusion requester | Name of the EXCLUSION_MANAGER user who requested to add the exclusion | String | 1 |
| Exclusion request comment | Comment associated to the exclusion request | String | 1 |
| Quality rule name | Name of the quality rule | String | 1 |
| Object name location | Full name of the object | String | 1 |
| Exclusion request last update | Date of last update of the exclusion request | String | 1 |
| Metric Id | Quality Rule Id | Integer | 1 |

---

## Scheduled Exclusions Summary

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/exclusions/scheduled-summary{?Parameters}` | Scheduled exclusions summary for this application at this snapshot |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| rule-pattern | Filter the scheduled exclusions following the specified quality rules | a combination of integers and strings | All quality rules |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| scheduledExclusionsToAdd.number | Number of scheduled exclusions to add in the next snapshot or reconsolidation | Integer | 1 |
| scheduledExclusionsToRemove.number | Number of scheduled exclusions to remove in the next snapshot or reconsolidation | Integer | 1 |
| scheduledExclusions.number | scheduledExclusionsToAdd.number + scheduledExclusionsToRemove.number | Integer | 1 |

### JSON Example

```json
{
  "href": "ENDTOEND83/applications/3/snapshots/15/exclusions/scheduled-summary",
  "name": "Scheduled Exclusions Summary",
  "scheduledExclusionsToAdd": { "number": 5 },
  "scheduledExclusionsToRemove": { "number": 1 },
  "scheduledExclusions": { "number": 6 }
}
```

---

## Violation

A violation identifies a component breaking a quality rule, with the following properties:

- a defective component
- a violated rule pattern
- a set of findings to pinpoint the statements or properties of the defective component violating the rule pattern
- a remedial action request or an exclusion request

Note: "findings" are also known as "associated values"

Note 2: "violation" is also known as "failed check"

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/violations{?Parameters}` | An empty array if there is no violation or an array with a single violation. |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations{?Parameters}` | Array of Violations raised for this application snapshot. |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations{?Parameters}` | Array of Violations raised for this module snapshot |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations{?Parameters}` | Array of Violations raised for this application snapshot in the scope of this tree node |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations{?Parameters}` | Array of Violations raised for this application snapshot in the scope of this transaction |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/excluded-violations{?Parameters}` | Array of Violations that are effectively excluded in this application snapshot |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations{?Parameters}` | Array of Violations removed between this snapshot and the previous snapshot (corrected, excluded, or object no longer exists). |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations{?Parameters}` | Array of violations between two layers |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | Specify first item (for JSON format only) | an integer | 1 |
| nbRows | Specify max number of items to return (for JSON format only) | an integer | 10 |
| rule-pattern | Specify the Quality Rules with a combination of syntaxes: `c:technicalCriterionId`, `cc:businessCriterionId`, `nc:businessCriterionId`, `bqi:businessCriterionId`, `critical-rules`, `quality-rules`, or a rule pattern id. NB: parameter not used for Excluded Violations | a combination of integers and strings | All quality rules |
| business-criterion | A quality indicator ID of a business criterion to add "Propagated Risk Index" property to components. NB: parameter not used for Excluded Violations and Removed Violations | an integer | None |
| technologies | A technology name to filter violations on. Available for ApplicationSnapshot and ModuleSnapshot violations. NB: parameter not used for Excluded Violations and Removed Violations | String | none |
| status | A violation status to filter violations on. NB: parameter not used for Excluded Violations and Removed Violations | String | none |
| caller-layer | The caller layer name — only for Architecture Models | String | none |
| callee-layer | The callee layer name — only for Architecture Models | String | none |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| component | Defective component snapshot violating a quality rule pattern | Structure | 1 |
| rulePattern.href | Quality Rule Pattern to comply. Includes the "critical" property for application/module violations and removed-violations templates. | URI | 1 |
| remedialAction | A remediation action request. This property is exclusive with "exclusionRequest" property | Structure | 0..1 |
| diagnosis | Diagnosis status and findings or bookmarks. This property is null if violation has been solved or excluded | Structure | 0..1 |
| exclusionRequest | An exclusion request. This property is exclusive with "remedialAction" property | Structure | 0..1 |

**Diagnosis structure**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | Diagnosis status: **"added"**: newly defective object; **"updated"**: some diagnosis results have changed; **"unchanged"**: compared to previous snapshot, diagnosis results are left unchanged | String | 1 |
| findings.ref | A reference to a diagnosis findings structure | URI | 0..1 |

### Excel/CSV Representation

| Columns | Description | Type | Occurs |
|---|---|---|---|
| Quality rule name | Full name of a rule pattern | String | 1 |
| Metric Id | Quality Rule Id | Integer | 1 |
| Critical | True if this has a critical contribution to a technical criterion. Defined only for application/module violations and removed-violations templates. | Boolean | 0..1 |
| Object name location | Full name of the defective component | String | 1 |
| Object status | Component status compared to the previous snapshot: "added", "unchanged", "updated" | String | 1 |
| Snapshot date | Input snapshot Date with YYYY-MM-DD format | String | 1 |
| Business Criterion | An optional input health factor to filter violations | String | 0..1 |
| Risk | Risk propagation assessment index according to an input health factor. | Integer | 0..1 |
| *Diagnosis Findings Name* | The associated values (with a comma separator in case of multiple values) | String | 1 |
| Violation status | Diagnosis status: "added" (newly defective), "updated" (diagnosis changed), "unchanged" | String | 1 |
| Action plan status | Status of a related issue: "added" (new in current snapshot), "pending" (not yet corrected), "solved" (corrected) | String | 1 |
| Action plan tag | A general text intended to replace priority | String | 1 |
| Exclusion request status | Exclusion request status | String | 1 |
| Exclusion requester | Name of the EXCLUSION_MANAGER user who requested the exclusion | String | 1 |

---

## Indexed Violation

This allows to filter violations with parameters, using a dedicated Lucene index.

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| PUT | application/json | `{Domain}/violations-index` | Create the Lucene index dedicated to violations search *(Admin only)* |
| GET | application/json | `{Domain}/violations-index` | Return information about the index |
| GET | application/json, text/csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/indexed-violations{?Parameters}` | Number and array of Violations resulting from filtering with Parameters for this application snapshot |

**Parameters** (all optional)

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| startRow | See Violation startRow | an integer | 1 |
| nbRows | See Violation nbRows | an integer | 10 |
| rule-pattern | See Violation rule-pattern. e.g. `rule-pattern=(bqi:60014)`, `rule-pattern=(c:61020)`, `rule-pattern=(7156)`, `rule-pattern=(bqi:60014,c:61020,7156)` | a combination of integers and strings | All quality rules |
| weight | A combination of integer values ranged from 1 to 9. e.g. `weight=(2)`, `weight=(2,5)` | Integer | All weights |
| critical | `true` or `false`. e.g. `critical=(true)` means all critical QRs | Boolean | All criticalities |
| business-criterion | See Violation business-criterion | an integer | None |
| status | See Violation status | String | none |
| object-status | The violated component status: `added`, `updated`, `unchanged` | String | none |
| technologies | See Violation technologies | String | none |
| modules | One or more module names | String | none |
| transactions | One or more transaction ids | Integer | none |
| object-fullname | A string to search in a component's name | String | none |
| order | Specify columns to be sorted server-side. Values: `action-exclusion-status`, `component-name`, `rule-pattern-name`, `diagnosis-status`. Use `asc(...)` or `desc(...)`. By default: PRI desc, then RULE_NAME asc, then OBJECT_FULL_NAME asc, then OBJECT_ID asc. | String | PRI desc, rule name asc, object name asc |

**NB.** Any combination of the parameters rule-pattern, weight and critical gives the QRs verifying all the constraints. For example: `rule-pattern=(bqi:60014)&weight=(2,5)&critical=(true)` means "all critical QRs of BC Efficiency with weight=2 or weight=5"

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| number | Total number of violations matching the filter | Integer | 1 |
| violations | Filtered violations | Array of Violations | 1 |

### JSON Example

```json
{
  "number": 4893,
  "violations": [
    {
      "component": {
        "href": "ENDTOEND833/components/1049/snapshots/13",
        "name": "AppliPubs.liens_serveurs.lien_table",
        "shortName": "lien_table",
        "status": "unchanged"
      },
      "diagnosis": {
        "status": "unchanged",
        "findings": {
          "href": "ENDTOEND833/components/1049/snapshots/13/findings/7344",
          "name": "Diagnosis Findings"
        }
      },
      "exclusionRequest": null,
      "rulePattern": {
        "href": "ENDTOEND833/rule-patterns/7344",
        "name": "Avoid \"SELECT *\" queries"
      },
      "remedialAction": null
    }
  ]
}
```

### Excel/CSV Representation

See Violation Excel/CSV Representation.

### Violations Summary

This represents the counts of total, added, updated or unchanged violations.

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations Summary for this application snapshot |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations-summary{?Parameters}` | Removed Violations Summary for this application snapshot |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations-summary{?Parameters}` | Violations Summary for this application snapshot and architecture model |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations Summary for this module snapshot |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations Summary for this treenode snapshot |
| GET | application/json | `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations Summary for this transaction snapshot |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| rule-pattern | A rule pattern ID | an integer or a string | None (Mandatory parameter) |
| technologies | A technology name to filter violations on. Available for ApplicationSnapshot and ModuleSnapshot only | String | none |
| status | A violation status to filter violations on: `added`, `updated`, `unchanged`. If not specified, response contains total count. If the corresponding status is not requested in the URL, the response contains `"xxxViolations": null`. | String | none |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Self reference | URI | 1 |
| name | Name | String | 1 |
| totalViolations.number | The number of total violations in the context, or null if not requested | Integer | 0..1 |
| addedViolations.number | The number of added violations in the context, or null if not requested | Integer | 0..1 |
| updatedViolations.number | The number of updated violations in the context, or null if not requested | Integer | 0..1 |
| unchangedViolations.number | The number of unchanged violations in the context, or null if not requested | Integer | 0..1 |

### JSON Representation For Architecture Models

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Self reference | URI | 1 |
| name | Name | String | 1 |
| callerLayer | The caller layer name | String | 1 |
| calleeLayer | The callee layer name | String | 1 |
| violations.href | The list of violations between the two layers | URI | 1 |
| violations.number | The number of violations between the two layers | Integer | 1 |

### JSON Representation For Transactions

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Self reference | URI | 1 |
| name | Name | String | 1 |
| shortName | Short name | String | 1 |
| status | Status | String | 1 |
| nbOfViolations | Number of violations | URI | 1 |
| omgTechnicalDebt | Technical Debt when the extension has been installed | Structure | 0..1 |
| omgTechnicalDebt.total | Total Remediation Effort | Integer | 1 |
| omgTechnicalDebt.numberOccurrences | Number of violation occurrences | Integer | 1 |
| omgTechnicalDebt.added | Technical Debt of added violations | Integer | 1 |
| omgTechnicalDebt.removed | Technical Debt of removed violations | Integer | 1 |

### JSON Representation For Removed Violations

| Properties | Description | Type | Occurs |
|---|---|---|---|
| removedViolations | The number of removed violations in the context | Integer | 1 |

### JSON Example

**Application Violations Summary**

```json
{
  "href": ".../violations-summary?rule-pattern=R&technologies=T&status=S",
  "name": "Violations count by rule, technology and status",
  "totalViolations": { "number": 4 },
  "addedViolations": { "number": 2 },
  "updatedViolations": { "number": 1 },
  "unchangedViolations": { "number": 1 }
}
```

**Architecture Model Violations Summary**

```json
{
  "callerLayer": "eCommerce Central",
  "calleeLayer": "eCommerce DataAccess",
  "violations": {
    "href": "ECOMMERCE/applications/3/snapshots/2/architecture-models/90000/violations?caller-layer=eCommerce+Central&callee-layer=eCommerce+DataAccess",
    "name": "Violations",
    "number": 15
  }
}
```

---

## OMG Technical Debt

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/omg-technical-debt` | Technical Debt details of a violation |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| total | Total Technical Debt is the Remediation Effort, designating the time required to remove an occurrence or a set of occurrences of a Technical Debt Item from the software. It covers the coding activity as well as unit/non-regression testing activities. | Integer | 1 |
| numberOccurrences | An occurrence (or Pattern Occurrence) designates a single instance of a Source Code Pattern representing a weakness implemented in the measured software. (ASCMM, ASCRM, ASCPEM, ASCSM) | Integer | 1 |
| complexity | The Complexity (or Effort Complexity) of the code elements implementing an Occurrence is qualification information measured according to the Effort Complexity definition from the Automated Enhancement Points (AEP) specification. | Integer | 1 |
| exposure | The Exposure of an Occurrence is qualification information that measures the level of connectedness of the Occurrence with the rest of the software, both directly and indirectly through call paths. | Integer | 1 |
| concentration | Concentration is qualification information that measures the number of Occurrences within any Code Element in the software. | Integer | 1 |
| technologicalDiversity | The Technological Diversity of an Occurrence is qualification information that measures the number of distinct programming languages in which the code elements included in a single occurrence are written. | Integer | 1 |
| gapSize | The extent of the gap between values and thresholds, measured as the difference between the values and the thresholds. | Integer | 1 |
| unadjustedEffort | Remediation effort as a digest of the complexity, exposure, concentration, technological diversity, gap size and the number of occurrences | Integer | 1 |
| added | Technical Debt of added occurrences | Integer | 1 |
| removed | Technical Debt of removed occurrences | Integer | 1 |
| adjustmentFactor | Adjustment Factor is a factor applied to compute the Remediation Effort from the Unadjusted Effort | Integer | 1 |

---

## Diagnosis Findings

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/findings/{RuleID}` | Findings of a violation |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| name | Name of findings items. If values is empty then no name is set. Name may be "null" if diagnosis of the rule pattern does not set any name. | String | 1 |
| type | Type of an item of 'values': **integer** (an integer, typically a threshold exceeded), **object** (wrapper of a component), **path** (execution path — array of wrappers of code fragment), **group** (array of wrappers of components), **percentage** (decimal number 0–100), **text** (string), **null** (no value) | String | 1 |
| values | An array of findings items | Array | 0..1 |
| values[] | An item (see type). For type "integer" and "percentage", this array contains a single value. | Any | 1..* |
| bookmarks | An array of array of bookmarks. This data is set only for the last snapshot of this application | Array | 0..1 |
| bookmarks[][] | A structure that pinpoints a violation. | Structure | 0..* |
| parameters | An array of parameters | Array | 0..1 |
| parameters[] | A parameter | Structure | 0..* |
| status[] | *Reserved* | String | 0..* |

**Findings item for type "object":**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| values[].component.codeSources | Component source codes. This data is set only for the last snapshot of this application | Structure | 0..1 |

**Values for type "path":**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| values[][].component | Component of the code fragment | Structure | 1 |
| values[][].codeFragment | A code fragment of an execution step. This data is set only for the last snapshot of this application | Structure | 0..1 |
| values[][].level | Execution code level of the code fragment, when this code fragment is part of an execution path | Integer | 0..1 |

**Values for type "group":**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| values[][].component.sourceCodes | Source codes of a group member. This data is set only for the last snapshot of this application | Structure | 0..1 |

**Bookmark item:**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| bookmarks[][].component | Component of the code fragment | Structure | 1 |
| bookmarks[][].codeFragment | A code fragment. This data is set only for the last snapshot of this application | Structure | 0..1 |

**Parameter item:**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| parameters[].name | Parameter name | String | 1 |
| parameters[].values[] | A parameter default value according to the technology | Any | 1..* |

### JSON Examples

**Integer type** (violation count with bookmarks)

```json
{
  "name": "Number of Violations Patterns",
  "type": "integer",
  "values": [2],
  "bookmarks": [
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} },
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ],
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} },
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ]
  ]
}
```

**Integer type** (sizing value without bookmarks)

```json
{
  "name": null,
  "type": "integer",
  "values": [35],
  "bookmarks": [
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ]
  ]
}
```

**Percentage type**

```json
{
  "name": "Comment/Code Ratio",
  "type": "percentage",
  "values": [15.0],
  "bookmarks": []
}
```

**Text type** (undocumented parameters)

```json
{
  "name": "Undocumented parameters",
  "type": "text",
  "values": ["param1", "param2"],
  "bookmarks": [
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ],
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ]
  ]
}
```

**Object type**

```json
{
  "name": "Name of the table having a misused composite index",
  "type": "object",
  "values": [
    { "component": { "name": "...", "sourceCodes": {} } },
    { "component": { "name": "...", "sourceCodes": {} } }
  ],
  "bookmarks": [
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ],
    [
      { "component": { "name": "...", "sourceCodes": {} }, "codeFragment": {} }
    ]
  ]
}
```

**Path type** (data-flow diagnosis)

```json
{
  "name": null,
  "type": "path",
  "values": [
    [
      { "component": {}, "codeFragment": {}, "level": 2 },
      { "component": {}, "codeFragment": {}, "level": 2 }
    ],
    [
      { "component": {}, "codeFragment": {}, "level": 2 },
      { "component": {}, "codeFragment": {}, "level": 2 }
    ]
  ],
  "bookmarks": null
}
```

---

## Search Results

> **Note:** This section covers search result resources. Please refer to the official documentation for full details on the Search Results API, including the `mode` parameter used in Indexed Violations.

---

## List of extensions

> **Note:** This section covers the list of installed extensions. Please refer to the official documentation for full details.

---

## Findings Report

> **Note:** This section covers the Findings Report resource. Please refer to the official documentation for full details.

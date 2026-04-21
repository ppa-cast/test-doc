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
- [Action Plan Recommendation #1](#action-plan-recommendation-1)
- [Action Plan Recommendation #2](#action-plan-recommendation-2)
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

### URI Templates 

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` 

  - *Description*:

    Array of Components of an application snapshot filtered by Health Factor

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` 

  - *Description*:

     Array of Components of a module snapshot filtered by Health Factor 

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{DistributionID}/{categoryRank}/{?parameters}`

  - *Description*:

     Array of Components of a distribution for an application snapshot by Health Factor

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components/{DistributionID}/{categoryRank}/{?parameters}`

  - *Description*:

     Array of Components of a distribution for module snapshot by Health Factor

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`
    
- **GET** `{Domain}/applications/{ApplicationID}/components/65005/{?parameters}` 

  - *Description*:

    Array of Components for Cost Complexity
    
  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/modules/{ModuleID}/components/65005/{?parameters}`

  - *Description*:

    Array of Components for Cost Complexity
    
  - *Media Type*:
    - `application/json`
    
- **GET** `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}`

  - *Description*:

    A component snapshot 
    
  - *Media Type*:
    - `application/json`
       
### URI Parameters
    
#### Common parameters

- **startRow**

    - *Description:* Specify first item 

    - *Values:* an integer

    - *Default value:* 1

- **nbRows**

    - *Description:* Specify max number of items to return

    - *Values:* an integer

    - *Default value:* 10

- **business-criterion**

    - *Description:* A health factor ID to compute propagated risk index 

    - *Values: a string

    - *Default value:* None

#### Additional parameters for `{HealthFactorID}` URIs

- **properties**

    - *Description:* 
        List of component properties which should be non-null to filter the components. e.g. `properties=(cyclomaticComplexity,fanOut)`.
        
        Available properties: 
        - `codeLines`, 
        - `commentedCodeLines`, 
        - `commentLines`, 
        - `coupling`, 
        - `fanIn`, 
        - `fanOut`, 
        - `cyclomaticComplexity`, 
        - `ratioCommentLinesCodeLines`, 
        - `halsteadProgramLength`, 
        - `halsteadProgramVocabulary`, 
        - `halsteadVolume`, 
        - `distinctOperators`, 
        - `distinctOperands`, 
        - `integrationComplexity`, 
        - `essentialComplexity`. 
            
        Their values are known only for the last snapshot.
        
    - *Values: a list of predefined strings 

    - *Default value:* None

- **order**

    - *Description:* 
        
        See Indexed Violation order
        
        By default, components are ordered by PRI desc (if any), component full name asc, component id asc.

        The sortable columns are all the above properties, plus: 
        - `pri`, 
        - `component-name`, 
        - `component-id`
        
    - *Values: a list of sort orders 

    - *Default value:* None
    

#### Only parameters applicable to Cost Complexity (`65005`) URIs

- **snapshot-ids**

    - *Description:* Two snapshot ids (snapshot range to consider). e.g. `snapshot-ids=(12,14)` 

    - *Values:* Two comma separated integers

    - *Default value:* None

- **status**

    - *Description:* Status of the artifacts between snapshots. Possible values: `added`, `updated`, `deleted` 

    - *Values:* String

    - *Default value:* None

- **technologies**

    - *Description:*  A technology name to filter artifacts

    - *Values:* String

    - *Default value:* None
    
#### Additional Parameters

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

```json
{
   "href":"ENDTOEND83/components/34364/snapshots/16",
   "name":"WASecurityForm._Default.GetRawInputGet",
   "shortName":"GetRawInputGet",
   "type":{
      "label":"C# Method",
      "name":"CAST_DotNet_MethodCSharp"
   },
   "treeNodes":{
      "href":"ENDTOEND83/components/34364/snapshots/16/tree-nodes",
      "name":"Tree Nodes"
   },
   "codeLines":1,
   "commentedCodeLines":null,
   "commentLines":0,
   "coupling":3,
   "fanIn":2,
   "fanOut":3,
   "cyclomaticComplexity":1,
   "ratioCommentLinesCodeLines":0,
   "halsteadProgramLength":8,
   "halsteadProgramVocabulary":8,
   "halsteadVolume":16.635532333438686,
   "distinctOperators":5,
   "distinctOperands":3,
   "integrationComplexity":1,
   "essentialComplexity":1
}
```

```json
[
   {
      "href":"ENDTOEND84/components/16879/snapshots/15",
      "name":"Pchit.Builder.AddListToList",
      "shortName":"AddListToList",
      "nbOfUpdates":1,
      "complexity":"moderate risk",
      "sqlComplexity":"low risk",
      "granularity":"moderate risk",
      "lackOfComments":"moderate risk",
      "coupling":"low risk",
      "treeNodes":{
         "href":"ENDTOEND84/components/16879/snapshots/15/tree-nodes",
         "name":"Tree Nodes"
      }
   },
]
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
| endColumn | end column of code fragment.<br/>Note: column number may be set to null in some cases.<br/>Note 2: this value is exclusive — code fragment ends at column (endColumn-1). This convention allows to locate a point in the text. | Integer | 1 |

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
| status | Filter the issues following the specified statuses.| Possible values: `added`, `pending`, `solved`<br/>e.g. `status=(added,pending)` | All statuses |
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
| status | This property is set only in the scope of an issue or a violation.<br/>Computed with internal dates compared to the current snapshot date:<br/>**"added"**: A remedial action has been requested at this violation snapshot date;<br/>**"pending"**: A remedial action has been requested prior to this snapshot, violation is still an issue;<br/>**"solved"**: This violation has been solved and does no longer exist in this snapshot | String | 1 |
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

### URI Templates

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}`

  - *Description*:

    Array of issues applicable until this application snapshot. "status" property of remedial action indicates the violation status regarding this action.

  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}`

  - *Description*:

    Array of issues applicable until this module snapshot. "status" property if remedial action indicates the violation status regarding this action.
    
  - *Media Type*:
    - `application/json`


- **POST** `{Domain}/applications/{ApplicationID}/action-plan/issues`

  - *Description*:

    Create a set of issues from last application snapshot. 

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    🛈 Payload is a collection of triplets: component, rule-pattern, and remedial action.

    ```json
    [{ "component": { "href": "D/components/C/snapshots/S" },
       "rulePattern": { "href": "D/rule-patterns/M" },
       "remedialAction": { "comment": "blablabla", "tag": "mytag" } }, ... ]
    ```

    🛈 For compatibility considerations with earlier versions, the following format is still accepted:

    "remedialAction": { "comment": "blablabla", "priority": "high" } }

    🛈 No active action must be already defined on the same component and the same rule pattern

    🛈 Remedial actions are created with last application snapshot date as internal 'start date'    

  - *Media Type*:
    - `application/json`
    
   
- **PUT** `{Domain}/applications/{ApplicationID}/action-plan/issues`

  - *Description*:

    Update a set of issues opened in last application snapshot for a rule pattern

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    (info) Payload is a collection of triplets: component, rule-pattern, and remedial action.

    ```json    
    [{ "component": { "href": "D/components/C/snapshots/S" },
       "rulePattern": { "href": "D/rule-patterns/M" },
       "remedialAction": { "comment": "blablabla", "tag": "mytag" } }, ... ]
    ```

    🛈 For compatibility considerations with earlier versions, the following format is still accepted:

    ```json
    "remedialAction": { "comment": "blablabla", "priority": "high" } }
    ```

    🛈 Related component must belong to the last application snapshot

    🛈 Remedial action must be active (i.e. not deleted with internal 'last date').

  - *Media Type*:
    - `application/json`


- **DELETE** `{Domain}/applications/{ApplicationID}/action-plan/issues`

  - *Description*:

    Remove a set of issues opened in last application snapshot for a rule pattern

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    🛈 Payload is a collection of issues, with rule-pattern href and component href:

    ```json
    [ { component: { "href": ....}, rulePattern: { "href": .... } }, ... ]
    ```

    🛈 Related component must belong to the last application snapshot

    🛈 Remedial actions must be active (i.e. internal 'end date' property not set).

  - *Media Type*:
    - `application/json`
    

### URI Parameters

- **startRow**

    - *Description:* Specify first item 

    - *Values:* an integer

    - *Default value:* 1
    

- **nbRows**

    - *Description:* Specify max number of items to return (for JSON format only)

    - *Values:* an integer

    - *Default value:* 10
    

- **rule-pattern**

    - *Description:* 
    
      Filter the issues following the specified quality rules
      
      The common syntax for this parameter can be found in the Violation section

    - *Values:* a combination of integers and strings, e.g. `rule-pattern=(7156,8294)`

    - *Default value:* All quality rules
    

- **status**

    - *Description:* Filter the issues following the specified statuses. Possible values: `added`, `pending`, `solved`

    - *Values:* 
    
      Possible values are `added`, `pending` or `solved`

      eg. `status=(added,pending)`

    - *Default value:*  All statuses 
    

- **tag**

    - *Description:* Filter the issues following the specified tags (an issue is returned if the tag equals the specified value)

    - *Values:* e.g. `tag=(low,high)`

    - *Default value:*  All tags
    

- **comment**

    - *Description:* Filter the issues following the specified comment (an issue is returned if the comment equals the specified value) 

    - *Values:*
    
      eg.
      - `comment=()` /* blank comment */
      - `comment=(test)`
      - `comment=(,test)` /* blank comment or test */
    
    - *Default value:*  All comments
    

- **object-fullname**

    - *Description:* Filter the issues following the specified object full name (an issue is returned if the object full name contains the specified value)

    - *Values:* a string

    - *Default value:*  All object full names
    

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
| Action plan status | This property is set only in the scope of an  issue or a  violation.<br/>This property is computed with internal dates properties compared to the current snapshot date:<br/><br/>"added": A remedial action has been requested at this violation snapshot date, because this violation has been identified as an issue <br/>"pending": A remedial action has been requested prior to this violation snapshot date; this violation is still an issue to solve, it has not been corrected for this snapshot<br/>"solved": This violation has been solved and does no longer exist in this snapshot  | String | 1 |
| Action plan comment | Remedial action comment | String | 1 |
| Quality rule name | Rule pattern name | String | 1 |
| Object name location | Full name of the defective component | String | 1 |
| Action plan last update | Date of last update in the action plan | Date | 1 |
| Metric Id | Quality Rule Id | Integer | 1 |

---

## Action Plan Trigger

An Action Plan Trigger is composed of a rule pattern and a remedial action. If active, all the violations of the corresponding rule will be automatically put in Action Plan in the next snapshot.


### URI Templates 

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/triggers{?Parameters}` 

  - *Description*:

    Array of action plan triggers applicable in the next snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **POST** `{Domain}/applications/{ApplicationID}/action-plan/triggers`  

  - *Description*:

    Create a set of action plan triggers. 
    
    Users MUST have `QUALITY_AUTOMATION_MANAGER` Role and MUST be granted to access to the related application

    (info) Payload is a collection of triplets: rule pattern, remedial action pattern, active.

    ```json
    [ 
        {
            "rulePattern": {"href" : "D/rule-patterns/M"},
            "remedialActionPattern": {"tag": "blabla bla", "comment": "to do asap"},
            "active": "true"
        }, 
        ... 
    ]
    ```

    (info) For compatibility considerations with earlier versions, the following format is still accepted:

    ```json
        "remedialActionPattern": {"priority": "extreme", "comment": "to do asap"}
    ```

  - *Media Type*:
    - `application/json`

- **PUT** `{Domain}/applications/{ApplicationID}/action-plan/triggers`  

  - *Description*:

    Update a set of action plan triggers

    Users MUST have `QUALITY_AUTOMATION_MANAGER` Role and MUST be granted to access to the related application

    (info) Payload is a collection of triplets: rule pattern, remedial action pattern, active.

    ```json
    [ 
        {
            "rulePattern": {"href" : "D/rule-patterns/M"},
            "remedialActionPattern": {"tag": "blabla bla", "comment": "to do asap"},
            "active": "true"
        }, 
        ... 
    ]
    ```
    
    (info) For compatibility considerations with earlier versions, the following format is still accepted:
    
    ```json
        "remedialActionPattern": {"priority": "low", "comment": "to do someday"}
    ```

  - *Media Type*:
    - `application/json`

- **DELETE** `{Domain}/applications/{ApplicationID}/action-plan/triggers`  

  - *Description*:

    Remove a set of action plan triggers

    Users MUST have `QUALITY_AUTOMATION_MANAGER` Role and MUST be granted to access to the related application

    (info) Payload is a collection of rule patterns:

    ```json
    [ 
        { 
            "rulePattern": {"href": "D/rule-patterns/M"}
        }, 
        ... 
    ]
    ```

  - *Media Type*:
    - `application/json`
    
### URI Parameters

- **rule-pattern**

    - *Description:* Filter the action plan triggers following the specified quality rules. The common syntax for this parameter can be found in the Violation section 

    - *Values:* a combination of integers and strings

    - *Default value:* All quality rules 
    
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

## Action Plan Recommendation #1

To automatically get an action plan recommendation

### URI Templates 

- **GET** `{Domain}/action-plan-recommendation?{parameters}` 

  - *Description*:

    Calculate a recommendation action plan in order to reach a target for an application. 

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    Return an HTTP Error if the target is already reached

  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/applications/{ApplicationID}/action-plan-recommendation?{parameters}` 

  - *Description*:

    Calculate a recommendation action plan in order to reach a target for an application. 

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    Return an HTTP Error if the target is already reached

  - *Media Type*:
    - `application/json`
    
- **PUT** `{Domain}/action-plan-recommendation`

  - *Description*:

    Calculate a recommendation action plan in order to reach a target for an application, and create automatically the action plan issues

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    Return an HTTP Error if the target is already reached

  - *Media Type*:
    - `application/json`
    
- **PUT** `{Domain}/applications/{ApplicationID}/action-plan-recommendation` 

  - *Description*:

    Calculate a recommendation action plan in order to reach a target for an application, and create automatically the action plan issues

    Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application

    Return an HTTP Error if the target is already reached

  - *Media Type*:
    - `application/json`
    
### URI Parameters

These parameters set the Action Plan Recommendation options, as query parameters for GET action, or as payload data for PUT action:

- **qualityIndicator/quality-indicator*

    - *Description:* A business criterion ID to improve.

    - *Type:* Integer

    - *Default value:* 60017 (Total Quality Index)

- **scoreType/score-type*

    - *Description:* 
    
        The score type to improve for the quality indicator:

        - "grade": score between 1.0 and 4.0
        - "compliance": compliance score between 0.0 and 1.0

    - *Type:* String

    - *Default value:* grade

- **goal/goal*

    - *Description:* 
    
        The goal of the action plan:

        - "reach-score-minimizing-violations": Reach a grade or a compliance score with the minimum violations to fix
        - "reach-score-minimizing-effort": Reach a grade or a compliance with the minimum effort
        - "reach-violations-count-maximizing-score": Fix violations to get the maximum grade or compliance score
        - "reach-total-effort-maximizing-score": Allocate effort to get the maximum grade of compliance score

    - *Type:* String

    - *Default value:* reach-score-minimizing-violations

- **target/target*

    - *Description:* 
    
        A target to each depending on the goal.

        This is a value between 1.0 and 4.0 for a grade, a value between 0.0 and 1.0 for a compliance score, a number of violations to fix, or an effort in minutes.

    - *Type:* Decimal

    - *Default value:* Default value is 4.0 for a grade; 1.0 or a compliance score, 1 for a number of violations to fix, 8*60 (1 man/day) for an effort.

- **application/application-name*

    - *Description:* 
    
       Application name for the first URI template. The user must be authorized to access this application

    - *Type:* String

    - *Default value:* When there is a single application, this application is set by default.

- **append/append*

    - *Description:* 
    
        If append=false ignore the current action plan, if append=true include the current action plan issues in the recommendation.

        WARNING: In case of a PUT action, if append =true, then the current action plan issues are all removed
        
    - *Type:* Boolean

    - *Default value:* false
    
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

## Action Plan Recommendation #2

To inject recommended remediations from a client

### URI Templates 

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues/recommendations` | Add action plan issues from a payload of remediations<br/>Domain must be an Engineering domain<br/>Users MUST have QUALITY_MANAGER Role and MUST be granted to access to the related application<br>Return an HTTP Error if the target is already reached |

### JSON Representation of Payload of Remediations

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern | Rule Pattern | Structure | 1 |
| remedialAction | Remedial Action Pattern<br/>NB. Whether the input has been "comment + priority" or "comment + tag", the JSON will contain {{"priority": "value", "tag": null}} or {{"priority": null, "tag": "value"}}. | Structure | 1 |
| number | Number of violations to add to action plan | Integer | 1 |

### Example

```json
{
  "rulePattern": { "href": "AED13/rule-patterns/7438" },
  "remedialAction": { "tag": "high", "comment": "Recommended action plan for Total Quality Index score improvement from 2.67 to 2.98" },
  "number": 3
},
{ 
  "rulePattern": { "href": "AED13/rule-patterns/7434" }, 
  "remedialAction": { "tag": "high", "comment": "Recommended action plan for Total Quality Index score improvement from 2.67 to 2.98" }, 
  "number": 1 
} 
```

---

## Exclusion Request Detail

*No URI — an exclusion request detail is always embedded in an exclusion request or a violation.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | **added**: a newly created exclusion request;<br/>**processed**: an exclusion request that has been processed;<br/>**unbound**: an exclusion request has been defined but the violation does not exist any more | String | 1 |
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
| status | Status of the scheduled exclusion:<br/>`to-add` = violations to exclude at the next snapshot;<br/>`to-remove` = exclusions to de-exclude;<br/>`$all` = all regardless of status | `to-add`, `to-remove`, `$all` | `$all` |
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

### JSON Example

```json
[
   {
      "component":{
         "href":"ENDTOEND83/components/19517/snapshots/15",
         "name":"com.castsoftware.util.data.Criterion",
         "shortName":"Criterion"
      },
      "rulePattern":{
         "href":"ENDTOEND83/rule-patterns/4554",
         "name":"Avoid large Classes - too many Methods"
      },
      "exclusionRequest":{
         "status":"added",
         "userName":"DCA",
         "comment":"Num metric, Single AV, Violation in last snapshot (15)",
         "dates":{
            "updated":{
               "time":1497279600000,
               "isoDate":"2017-06-12"
            }
         }
      },
      "remedialAction":null
   },
   {
      "component":{
         "href":"ENDTOEND83/components/17172/snapshots/15",
         "name":"O11JNK.LINDBERGH_CENTRAL.PROPATTR",
         "shortName":"PROPATTR"
      },
      "rulePattern":{
         "href":"ENDTOEND83/rule-patterns/1596",
         "name":"Avoid using \"nullable\" Columns except in the last position in a Table"
      },
      "exclusionRequest":null,
      "remedialAction":null
   }"..."
]
```

### CSV/EXCEL Example

| Exclusion status | Exclusion requester | Exclusion request comment | Quality rule name                                                     | Metric Id | Object name location                          | Exclusion request last update |
| -----------------|---------------------|---------------------------|-----------------------------------------------------------------------|-----------|-----------------------------------------------|-------------------------------|
| to add           | DCA                 | Comment 1                 | Avoid large Classes - too many Methods                                | 4554      | com.castsoftware.util.data.Criterion          | 2017-06-12
| to remove        |                     |                           | Avoid using "nullable" Columns except in the last position in a Table | 1596      | O11JNK.LINDBERGH_CENTRAL.PROPATTR             | 
| to add           | DCA                 | Comment 2                 | Avoid using Fields (non static final) from other Classes              | 46028     | <Default Package>.ButtonReset.actionPerformed | 2017-06-12
| to add           | ExclusionMan        | Comment 3                 | Avoid using Global Variables                                          | 588       | <none>                                        | 2017-06-12
| to add           | ExclusionMan        | Comment 4                 | Avoid using Inner Classes                                             | 7308      | com.castsoftware.util.string.XMLFileInterpreterImpl.flushSection | 2017-06-12
| to add           | DCA | Comment 5     |                           | Methods must have appropriate JavaDoc @param tags                     | 4672      | com.castsoftware.util.data.retriever.RowReaderConstant.close | 2017-06-12


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

For example, consider these 2 quality rules:
- 1. "Methods must have appropriate JavaDoc @param tags"
- 2. "Avoid calling the same paragraph with PERFORM and GO TO statements"  

For Quality Rule #1, diagnosis results are list of parameter names without documentation.

For Quality Rule #2; diagnosis results are list of "GO TO" statements. For each diagnosis result, a pair of diagnosis bookmarks locates the GO TO and the PERFORM statement.

Note: "findings" are also known as "associated values"

Note 2: "violation" is also known as "failed check" 


### URI Templates 

- **GET** `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/violations{?Parameters}`

  - *Description*:

    An empty array if there is no violation or an array with a single violation.

  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations{?Parameters}`

  - *Description*:

    Array of Violations that have been raised for this application snapshot.

  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations{?Parameters}`

  - *Description*:

    Array of Violations that have been raised for this module snapshot.

  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations{?Parameters}`

  - *Description*:

    Array of Violations raised for this application snapshot in the scope of this tree node

  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations{?Parameters}` 

  - *Description*:

    Array of Violations raised for this application snapshot in the scope of this transaction 
    
  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/excluded-violations{?Parameters}` 

  - *Description*:

     Array of Violations that are effectively excluded in this application snapshot
    
  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations{?Parameters}` 

  - *Description*:

     Array of Violations removed between this snapshot and the previous snapshot (corrected, excluded, or object no longer exists).
    
  - *Media Type*:
    - `application/json`
    - `text/csv` 
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` 

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations{?Parameters}` 

  - *Description*:

     Array of violations between two layers
    
  - *Media Type*:
    - `application/json`

    
### Parameters

- **startRow**

    - *Description:* Specify first item (for JSON format only)

    - *Values:* an integer

    - *Default value:* 1

- **nbRows**

    - *Description:* Specify max number of items to return (for JSON format only)

    - *Values:* an integer

    - *Default value:* 10

- **rule-pattern**

    - *Description:* 
    
        Specify the Quality Rules with a combination of the following syntaxes, separated by commas:

        - `c:` technicalCriterionId
        - `cc:` businessCriterionId
        - `nc:` businessCriterionId
        - `bqi:` businessCriterionId (Base Quality Indicators, i.e. all quality indicators from a Business Criterion)
        - `critical-rules` keyword (i.e. identified as critical regarding a business criterion)
        - a metric type, eg "quality-rules" keyword
        - a rule pattern id
        
        NB: parameter not used for Excluded Violations

    - *Values:* a combination of integers and strings
       
    - *Default Value*: All quality rules

- **business-criterion**

    - *Description:*
    
        A quality indicator ID of a business criterion (optional parameter) to a dd "Propagated Risk Index" property to components.

        If not specified or not a health factor, components will contain no PRI

        It's up to the requester to set a business criterion consistent with the rule-pattern.

        NB: parameter not used for Excluded Violations and Removed Violations    

    - *Values:* an integer

    - *Default value:* none

- **technologies**

    - *Description:*
    
        A technology name to filter violations on

        This parameter is available for ApplicationSnapshot violations and ModuleSnapshot violations, in any Media Type

        NB: parameter not used for Excluded Violations and Removed Violations

    - *Values:* a string

    - *Default value:* none
    
- **status**

    - *Description:* 

        A violation status to filter violations on
        NB: parameter not used for Excluded Violations and Removed Violations

    - *Values:* string

    - *Default value:* none

- **caller-layer**

    - *Description:* The caller layer name — only for Architecture Models 

    - *Values:* string

    - *Default value:* none    

- **callee-layer**

    - *Description:* The callee layer name — only for Architecture Models 

    - *Values:* string

    - *Default value:* none    


### Excel/CSV Representation


| Columns | Description | Type | Occurs |
|---|---|---|---|
| Quality rule name | Full name of a rule pattern | String | 1 |
| Metric Id | Quality Rule Id | Integer | 1 |
| Critical | True if this has a critical contribution to a technical criterion. Defined only for application/module violations and removed-violations templates. | Boolean | 0..1 |
| Object name location | Full name of the defective component | String | 1 |
| Object status | Component status on this component compared to the previous snapshot<br/>"added": new component<br/>"unchanged": unchanged component<br/>"updated": updated component<br/> | String | 1 |
| Snapshot date | Input snapshot Date with YYYY-MM-DD format | String | 1 |
| Business Criterion | An optional input health factor to filter violations. If this business criterion is a health factor, then a Propagated Risk Propagation Assessment Index is calculated for this Health Factor
 | String | 0..1 |
| Risk | Risk propagation assessment index according to an input health factor. | Integer | 0..1 |
| *Diagnosis Findings Name* | The associated values (with a comma separator in case of multiple values) | String | 1 |
| Violation status | Diagnosis status on this component compared to the previous snapshot<br/>"added": this component is a newly defective object regarding this quality rule<br/>"updated": some diagnosis results have been changed<br/>"unchanged": compared to the previous snapshot, diagnosis results are left unchanged | String | 1 |
| Action plan status | Status of a related issue regarding the snapshot of the container (application or module)<br/>"added": newly created issue in the current snapshot<br/>"pending": pending issue, violation is not yet corrected for this snapshot<br/>"solved": issue solved for this snapshot | String | 1 |
| Action plan tag | A general text intended to replace priority | String | 1 |
| Exclusion request status | Exclusion request status | String | 1 |
| Exclusion requester | Name of the EXCLUSION_MANAGER user who requested the exclusion | String | 1 |


### CSV Example

Diagnosis with bookmarks:

| Quality rule name                            | Metric Id | Business Criterion | Object name location                     | Object Status | Snapshot Date | Propagation Risk Index | Status    | Number of violations | Action Plan Status | Action Plan Priority |
|----------------------------------------------|-----------|--------------------|------------------------------------------|---------------|---------------|------------------------|-----------|----------------------|--------------------|------------- |
| Avoid missing WHEN OTHERS in CASE statements | 8028      | Transferability    | F:\USERS\ABD\CAST-TESTS\.../KCD_FTAB_GET | unchanged     | 2014-12-31    | 1                      | unchanged | 3                    | pendingIssues      |              |


Diagnosis without bookmarks:

| Quality rule name                               | Metric Id | Business Criterion | Object name location                                        | Object Status    | Snapshot Date | Propagation Risk Index | Status    | Number of violations | Undocumented parameters | Action Plan Status | Action Plan Priority |
|-------------------------------------------------|-----------|--------------------|-------------------------------------------------------------|------------------|---------------|------------------------|-----------|----------------------|--------------------|------------- |------------- |
Methods_must_have_appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.Monster.Monster           | unchanged        | 2014-12-31    | 1                      | unchanged | 3|_description,in_city,in_pricePerHour | pending  | |
Methods must have appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.MonsterEvent.addMonster   | unchanged        | 2014-12-31    | 1                      | unchanged | 3 | a,b,c | pending | |
Methods must have appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.Customer.Customer         | unchanged        | 2014-12-31    | 1                      | unchanged | 3 | a,b,c | pending | |
Methods must have appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.MonsterEvent.addOption    | unchanged        | 2014-12-31    | 1                      | unchanged | 3 | a,b,c | pending | |
Methods must have appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.MonsterEvent.MonsterEvent | unchanged        | 2014-12-31    | 1                      | added     | 3 | a,b,c | pending | |
Methods must have appropriate JavaDoc @param tags | 4672      | Transferability    | com.cast.monster_event.base.model.SpookyPlace.SpookyPlace   | unchanged        | 2014-12-31    | 1                      | added     | 3 | a,b,c | pending | 


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
| status | Diagnosis status component compared to the previous snapshot : **"added"**: newly defective object; **"updated"**: some diagnosis results have changed; **"unchanged"**: compared to previous snapshot, diagnosis results are left unchanged | String | 1 |
| findings.ref | A reference to a diagnosis findings structure | URI | 0..1 |

### JSON Example

```json
[
   { 
        "component" : {
            "href": "CENTRAL/components/4/snapshots/5",
            "name": "com.cast.monster_event.base.model.Monster.Monster", 
            "propagationRiskIndex" : ....,
            "status": "unchanged",
            "sourceCodes": {
                "href": "CENTRAL/components/4/snapshots/5/source-codes"
                "name": "Source codes",
            }
        },
        "rulePattern": { 
            "href": "CENTRAL/rule-patterns/4672",
            "name": "Methods must have appropriate JavaDoc @param tags"
        }, 
        "remedialAction" : { 
            "dates": {
                "updated": {"time":...},
            },
            "comment": "...",
            "priority": "high",
            "status": "added",
            "tag": "high"
        },
        "exclusionRequest": null,
        "diagnosis": {
            "status": "unchanged",
            "findings": {
                "href": "CENTRAL/components/4/snapshots/5/findings/4672"
                "name": "Diagnosis Findings"
            }
        }
    }
]
```

```json
[
    {
        "component": {
            "href": "ENDTOEND83/components/15700/snapshots/15",
            "name": "com.castsoftware.util.string.XMLFileInterpreterImpl.flushSection",
            "shortName": "flushSection",
            "status": "unchanged",
            "sourceCodes": {
                "href": "ENDTOEND83/components/15700/snapshots/15/source-codes",
                "name": "Source Codes"
            },
            "treeNodes": {
                "href": "ENDTOEND83/components/15700/snapshots/15/tree-nodes",
                "name": "Tree Nodes"
            }
        },
        "diagnosis": {
            "status": "unchanged",
            "findings": {
                "href": "ENDTOEND83/components/15700/snapshots/15/findings/7308",
                "name": "Diagnosis Findings"
            }
        },
        "exclusionRequest": {
            "status": "added",
            "userName": "ExclusionMan",
            "comment": "Meme metrique mais violation added",
            "dates": {
                "updated": {
                    "time": 1497259800000,
                    "isoDate": "2017-06-12"
                }
            }
        },
        "rulePattern": {
            "href": "ENDTOEND83/rule-patterns/7308",
            "name": "Avoid using Inner Classes"
        },
        "remedialAction": null
    }
]
```

ENDTOEND83/applications/3/snapshots/15/excluded-violations
```json
[
    {
        "rulePattern": {
            "href": "ENDTOEND83/rule-patterns/1596",
            "name": "Avoid using \"nullable\" Columns except in the last position in a Table"
        },
        "component": {
            "href": "ENDTOEND83/components/17172/snapshots/15",
            "name": "O11JNK.LINDBERGH_CENTRAL.PROPATTR",
            "shortName": "PROPATTR"
        },
        "exclusionRequest": {
            "status": "processed",
            "userName": "ExclusionMan",
            "comment": "Char metric, Multiple AV, Processed",
            "dates": {
                "updated": {
                    "time": 1497259800000,
                    "isoDate": "2017-06-12"
                }
            }
        },
        "remedialAction": null
    }
]
```

---

## Indexed Violation

This allows to filter violations with parameters, using a dedicated Lucene index.

### URI Templates 

- **PUT** `{Domain}/violations-index`

  - *Description*:

    Create the Lucene index dedicated to violations search *(Admin only)* 

  - *Media Type*:
    - `application/json`

- **GET** `{Domain}/violations-index`

  - *Description*:

     Return information about the index 

  - *Media Type*:
    - `application/json`
    
- **GET**  `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/indexed-violations{?Parameters}`

  - *Description*:

    Number and array of Violations resulting from filtering with Parameters for this application snapshot

  - *Media Type*:
    - `application/json`
    - `text/csv`
    - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

### URI Parameters

All parameters are optional. If mentioned, they will filter the violations accordingly.

- **startRow**

    - *Description:* Specify first item 

    - *Values:* an integer

    - *Default value:* 1

- **nbRows**

    - *Description:* Specify max number of items to return

    - *Values:* an integer

    - *Default value:* 10

- **rule-pattern**

    - *Description:* 
    
        See Violation rule-pattern

        A combination of bqi:BCID, c:TCID, RuleID
        (bqi means Base Quality Indicator, c means Contributor)

        eg.
        - `rule-pattern=(bqi:60014)` means all QRs of BC “Efficiency”
        - `rule-pattern=(c:61020)` means all QRs of TC “Programming Practices - Modularity and OO Encapsulation Conformity”
        - `rule-pattern=(7156)` means the QR “Avoid Too Many Copy Pasted Artifacts”
        - `rule-pattern=(bqi:60014,c:61020,7156)` means the union of all above QRs

    - *Values:* a combination of integers and strings
       
    - *Default Value*: All quality rules

- **weight**

    - *Description:* 
            
        A combination of integer values ranged from 1 to 9

        eg.
        - `weight=(2)` means all QRs with a weight=2
        - `weight=(2,5)` means all QRs with a weight=2 or a weight=5

    - *Values:* an integer
       
    - *Default Value*: All weights

- **critical**

    - *Description:* 
            
       true or false

       eg.
       - `critical=(true)` means all critical QRs
       - `critical=(false)` means all non critical QRs

    - *Values:* a boolean
       
    - *Default Value*: All criticalities

- **business-criterion**

    - *Description:* See Violation business-criterion
    
    - *Values:* an integer

    - *Default value:* none

- **status**

    - *Description:* See Violation status
    
    - *Values:* a string

    - *Default value:* none
    
- **object-status**

    - *Description:* 
    
      The violated component status

      Takes one or more values among `added`, `updated`, `unchanged`
    
    - *Values:* a string

    - *Default value:* none
    
- **technologies**

    - *Description:* See Violation technologies
    
    - *Values:* a string

    - *Default value:* none

- **modules**

    - *Description:* One or more module names
    
    - *Values:* a string

    - *Default value:* none
    
- **transactions**

    - *Description:* One or more transaction ids
    
    - *Values:* a string

    - *Default value:* none
        
- **mode**

    - *Description:* See Search Results mode
    
    - *Values:* a string

    - *Default value:* none
    
- **object-fullname**

    - *Description:* A string to search in a component's name

    - *Values:* a string

    - *Default value:* none

- **order**

    - *Description:* 

      Specify columns to be sorted on server side
   
      asc means ascending order, desc means descending order.
      Each sortable column is represented by a parameter value:
      - Actions or Exclusions: `action-exclusion-status`
      - Object name location: `component-name`
      - Rule: `rule-pattern-name`
      - Status: `diagnosis-status`
   
      The use of parameter order is optional. Also, if you use this parameter, you can mention from one to four values in it.
      Any not mentioned column value takes the default value. By default, violations are sorted by: PRI desc (if any), then by RULE_NAME asc, then by OBJECT_FULL_NAME asc, then by OBJECT_ID asc
   
      Examples:
      - `order=(asc(action-exclusion-status))` sorts the violations by ascending "Action/Exclusion": violations with remedial action, followed by violations with exclusion request, followed by all other violations
      - `order=(desc(diagnosis-status))` sorts the violations by descending "Violation Status": "updated" violations, followed by "unchanged" violations, followed by "added" violations
      - `order=(asc(action-exclusion-status),asc(component-name))` sorts the violations by ascending "Action/Exclusion", then by ascending "Object name"
      - `order=(asc(action-exclusion-status),asc(rule-pattern-name),desc(component-name),asc(diagnosis-status))`
   
      NB Sorting works also with pagination. You can combine it with the `startRow` and `nbRows` parameters

    - *Values:* an expression
       
    - *Default Value*: None

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

## Violations Summary

### URI Templates 

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations-summary{?Parameters}`

  - *Description*: Violations Summary for this application snapshot

  - *Media Type*: `application/json`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations-summary{?Parameters}`

  - *Description*: Removed Violations Summary for this application snapshot
  
  - *Media Type*: `application/json`

- **GET** `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations-summary{?Parameters}` 

  - *Description*: Violations Summary for this application snapshot and architecture model
  
  - *Media Type*: `application/json`

- **GET** `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` 

  - *Description*: Violations Summary for this module snapshot
  
  - *Media Type*: `application/json`

- **GET** `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations-summary{?Parameters}`  

  - *Description*: Violations Summary for this treenode snapshot
  
  - *Media Type*: `application/json`

- **GET** `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations-summary{?Parameters}`  

  - *Description*: Violations Summary for this transaction snapshot
  
  - *Media Type*: `application/json`
  

### URI Parameters

- **rule-pattern**

    - *Description:* A rule pattern ID

    - *Values:* an integer or a string
       
    - *Default Value*: None (Mandatory parameter)

- **technologies**

    - *Description:* A rule pattern ID
    
      A technology name to filter violations on

      This parameter is available for ApplicationSnapshot violations and ModuleSnapshot violations only

    - *Values:* a string

    - *Default Value*: None

- **status**

    - *Description:* A rule pattern ID
    
        A violation status to filter violations on

        ```status``` can have one of the following values: ```added```, ```updated```, ```unchanged```

        If you want to get the total count of violations (corresponding to "status=all"), simply do not mention any status in the URL.

        If the corresponding "status" has not been requested in the URL, then the response contains
        ```"xxxViolations": null```
        instead of
        ```"xxxViolations": { "number": N }```

    - *Values:* a string

    - *Default Value*: None

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
| gapSize | In the context of patterns which rely on roles that model values and threshold values that are not to be exceeded, the gap between these values must be closed to remediate this weakness; the Occurrence Gap Size is the extent of the gap, measured as the difference between the values and the thresholds. | Integer | 1 |
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
| type | Type of an item of 'values':<br/>**integer** (an integer, typically a threshold exceeded),<br/>**object** (wrapper of a component),<br/>**path** (execution path — array of wrappers of code fragment),<br/>**group** (array of wrappers of components),<br/>**percentage** (decimal number 0–100),<br/>**text** (string),<br/>**null** (no value) | String | 1 |
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

**Integer type** (sizing value with or without bookmarks)

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

Values is a list of execution steps (case of a data-flow diagnosis)
- Ex: 7954: Avoid indirect String concatenation inside loops
- Ex: 7150: Favor PreparedStatement or CallableStatement over Statement (Note: 7150 is a contributor of "66062: Secure Coding - Input Validation")

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

**Group type**

Specific case for "7156: Avoid Too Many Copy Pasted Artifacts"

```json
{
  "name": null,
  "type": "group",
  "values": [
    [
      { "component": { ... } },
      { "component": { ... } }
    ],
    [
      { "component": { ... } },
      { "component": { ... } }
    ]
  ],
  "bookmarks": null
}
```

**No value type**

Values is empty, but bookmarks are set.

```json
{
  "name": null,
  "type": null,
  "values": [],
  "bookmarks": [
    [
      { "component": { "name": "...", "sourceCodes": {...} }, "codeFragment": {...} },
      { "component": { "name": "...", "sourceCodes": {...} }, "codeFragment": {...} }
    ],
    [
      { "component": { "name": "...", "sourceCodes": {...} }, "codeFragment": {...} },
      { "component": { "name": "...", "sourceCodes": {...} }, "codeFragment": {...} }
    ]
  ]
}
```

---

## Search Results

Search results are items indexed by Lucene. As the construction of these indexes is still optional, we deliver these feature in a separate URI.

### URI Templates 

| HTTP Action | Media Type | URI Templates | Description |
| --- | --- | --- | --- |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/search-results{?Parameters}` | Search results on this application snapshot |

### URI Parameters 

| URI Parameter | Description | Values | Default value |
| --- | --- | --- | --- |
| items | Type of items to search on | "components" | none |
| mode | Search mode<br/>"prefix" retrieves objects starting with "word"<br/>"term" retrieves objects containing "word" | "prefix" or "term" | none |
| word | The word to search | a string | none |
| startRow | Specify first item | an integer | 1 |
| nbRows | Specify max number of items to return | an integer | 10 |

### JSON Representation

| Properties | Description | Type | Occurs |
| --- | --- | --- | --- |
| number | Total number of results matching the filter | Integer | 1 |
| components | Search results, ordered by alphabetical order (case-unsensitive) and object_id | Structure | 0..* |

### JSON Example

```json
{
   "number":7277,
   "components":[
      {
         "id":284004,
         "name":"[S:\\CAIP\\JAVA\\Applications\\Dashboard\\adg\\adg-war\\src\\main\\webapp\\javascript\\isomorphic\\system\\modules\\ISC_Core.js].$be",
         "shortName":"$be",
         "type":{
            "label":"Javascript Client Side Method",
            "name":"CAST_Javascript_ClientSide_Method"
         }
      },
      {
         "id":290125,
         "name":"[S:\\CAIP\\JAVA\\Applications\\Dashboard\\adg\\adg-war\\src\\main\\webapp\\javascript\\isomorphic\\system\\development\\ISC_Core.js].$be",
         "shortName":"$be",
         "type":{
            "label":"Javascript Client Side Method",
            "name":"CAST_Javascript_ClientSide_Method"
         }
      },
      ...
   }
}
```   
---

## List of extensions

### URI Templates 

| HTTP Action | Media Type | URI Templates | Description |
| --- | --- | --- | --- |
| GET | application/json | `{Domain}/extensions` | List of extensions that are installed on Application |


### JSON example

```json
[
    {
        "name": "/com.castsoftware.omg-ascqm-index",
        "version": "20200602.0.0-funcrel"
    },
    {
        "name": "/com.castsoftware.asp",
        "version": "1.0.0"
    },
    {
        "name": "/com.castsoftware.angularjs",
        "version": "2.0.12-funcrel"
    },
    {
        "name": "/com.castsoftware.jquery",
        "version": "2.2.0-funcrel"
    },
    {
        "name": "/com.castsoftware.mips-redux-index",
        "version": "20200602.0.0-funcrel"
    },
    {
        "name": "/com.castsoftware.nodejs",
        "version": "2.3.0-beta3"
    }
]
```

---

## Findings Report

# Findings Report

Create a report of violation findings including rules descriptions.

The `accept-language` HTTP header parameter can be set in order to get the translation of rules descriptions.

## URI Templates

| HTTP Action | Media Type | URI Templates | Description |
| --- | --- | --- | --- |
| GET | `application/json` | `/{Domain}/applications/{ApplicationID}/findings-report?{parameters}` | Create a findings report with a JSON format |
| GET | `application/xml` | `/{Domain}/applications/{ApplicationID}/findings-report?{parameters}` | Create a findings report with an XML format |
| GET | `text/html` | `/{Domain}/applications/{ApplicationID}/findings-report?{parameters}` | Create a findings report with an HTML format for a preview |

### Parameters

| URI Parameter | Description | Default value |
| --- | --- | --- |
| `business-criteria` | Fetch all rules attached to a business criteria list. Example for an ISO-5055 Index Report, when the ISO-5055 Index extension is installed:<br>`business-criteria=1061001,1061002,1061003,1061004`<br>Note: the structure of the report will be: Business Criteria/Rules | `60017` (TQI) |
| `quality-standards` | Fetch rules attached to a quality standard when the Quality Standard extension is installed.<br>`quality-standards=ISO-5055`<br>Note: the structure of the report will be: CWE/Rules | |
| `quality-categories` | Fetch rules attached to quality categories when the Quality Standard extension is installed.<br>`quality-categories=ISO-5055-Maintainability,ISO-5055-Performance-Efficiency,ISO-5055-Reliability,ISO-5055-Security`<br>Note: the structure of the report will be: Criteria/Rules. This is the same as the `business-criteria` example, except that the installation of ISO-5055 index is not required | |
| `select` | Control the report output:<br>`select=code-fragment` will add code fragment of each violation | |
| `findings-limit` | This is the number of reported findings per rule. Ex of a finding: a bookmark (including secondary bookmarks), a full call path (dataflow), an associated value.<br>Set `999999` for unlimited value (it should be avoided for large applications). This parameter aims to control the output report size. | `3` |
| `surrounding-lines` | This parameter adds N lines before/after a bookmark when the code fragment is required. | `0` |

---

## JSON Representation

| Property | Description | Type | Occurs |
| --- | --- | --- | --- |
| `name` | A top-level rules category name: a business criterion name, or a quality standard name, or a quality category name | String | 1 |
| `subCategories` | The list of sub categories | Array | 1 |
| `subCategories[]` | A sub category | Structure | 1..* |
| `subCategories[].name` | A sub category name | String | 1 |
| `subCategories[].rules` | The list of violated quality rules for this category | Array | 1 |
| `subCategories[].rules[]` | A violated quality rule | Structure | 1..* |
| `...rules[].id` | A quality rule ID | Integer | 1 |
| `...rules[].name` | A quality rule name | String | 1 |
| `...rules[].rationale` | A quality rule rationale | String | 1 |
| `...rules[].description` | A quality rule description | String | 1 |
| `...rules[].remediation` | A quality rule remediation | String | 1 |
| `...rules[].critical` | Critical rule in case of a report based on business criteria | Boolean | 0..1 |
| `...rules[].maxWeight` | Maximum weight contribution in case of a report based on business criteria | Integer | 0..1 |
| `...rules[].violations` | The list of violations for a quality rule | Array | 1 |
| `...rules[].violations[]` | A violation | Structure | 1..* |
| `...violations[].objectFullName` | The component full name in violation | String | 1 |
| `...violations[].objectType` | The type of the component in violation | String | 1 |
| `...violations[].findingName` | A finding name. For simple value findings this is the associated value name | String | 1 |
| `...violations[].findingType` | A finding type among: `number`, `percentage`, `text`, `integer`, `no-value`, `object`, `path`, `group`, `bookmark` | String | 1 |
| `....violations[].findings` | A list of findings for this violation | Array | 1 |
| `....violations[].findings[]` | A finding of this violation | Structure | 1..* |
| `...findings[].findingKey` | An identifier of the finding | Integer | 1 |
| `...findings[].findingValues` | A list of findings values when the finding type is a simple value: `number`, `percentage`, `text`, `integer` | Array | 1 |
| `...findings[].findingValues[]` | A finding value | any | 1..* |
| `...findings[].locations` | The source code locations of the finding | Array | 1 |
| `...findings[].locations[]` | A source code location | Structure | 1 |
| `...locations[].rank` | The rank order of the location | Integer | 1 |
| `...locations[].sourcePath` | The source code path | String | 0..1 |
| `...locations[].startLine` | The start line of the objects in violation (for finding type: `integer`, `text`), the start line of the bookmark (for finding type `bookmark`), or the start line of the call path (for finding type `path`) | Integer | 0..1 |
| `...locations[].endLine` | The end line of the objects in violation (for finding type: `integer`, `text`), the end line of the bookmark (for finding type `bookmark`), or the end line of the call path (for finding type `path`) | Integer | 0..1 |
| `...locations[].codeFragment` | The code fragment | String | 0..1 |
| `...locations[].fragmentStartLine` | The start line of the bookmark (for finding type `bookmark`) relatively to the code fragment | Integer | 0..1 |
| `...locations[].fragmentEndLine` | The end line of the bookmark (for finding type `bookmark`) relatively to the code fragment | Integer | 0..1 |

---

## XML Representation


## XML Representation

Structure overview:
```xml
/categories
└── /category
    ├── @name
    └── /subCategories
        └── /subCategory
            ├── @name
            └── /rules
                └── /rule
                    ├── @id, @name, @rationale, @description, @remediation
                    ├── @critical, @maxWeight
                    └── /violations
                        └── /violation
                            ├── @objectFullName, @objectType
                            ├── @findingName, @findingType
                            └── /findings
                                └── /finding
                                    ├── @key
                                    ├── /values
                                    │   └── /value
                                    └── /locations
                                        └── /location
                                            ├── @rank, @sourcePath
                                            ├── @startLine, @endLine
                                            └── /codeFragment
                                                ├── @startLine
                                                └── @endLine
```

| Node | Description | Type | Occurs |
| --- | --- | --- | --- |
| `/categories` | The list of top-level rule categories | | |
| `category` | A top-level rule category | | |
| `category@name` | A top-level rules category name: a business criterion name, a quality standard name, or a quality category name | String | 1 |
| `subCategories` | The list of sub categories | Array | 1 |
| `subCategory` | A sub category | Structure | 1..* |
| `subCategory@name` | A sub category name | String | 1 |
| `rules` | The list of violated quality rules for this category | Array | 1 |
| `rule` | A violated quality rule | Structure | 1..* |
| `rule@id` | A quality rule ID | Integer | 1 |
| `rule@name` | A quality rule name | String | 1 |
| `rule@rationale` | A quality rule rationale | String | 1 |
| `rule@description` | A quality rule description | String | 1 |
| `rule@remediation` | A quality rule remediation | String | 1 |
| `rule@critical` | Critical rule in case of a report based on business criteria | Boolean | 0..1 |
| `rule@maxWeight` | Maximum weight contribution in case of a report based on business criteria | Integer | 0..1 |
| `violations` | The list of violations for a quality rule | Array | 1 |
| `violation` | A violation | Structure | 1..* |
| `violation@objectFullName` | The component full name in violation | String | 1 |
| `violation@objectType` | The type of the component in violation | String | 1 |
| `violation@findingName` | A finding name. For simple value findings this is the associated value name | String | 1 |
| `violation@findingType` | A finding type among: `number`, `percentage`, `text`, `integer`, `no-value`, `object`, `path`, `group`, `bookmark` | String | 1 |
| `findings` | A list of findings for this violation | Array | 1 |
| `finding` | A finding of this violation | Structure | 1..* |
| `finding@key` | An identifier of the finding | Integer | 1 |
| `values` | A list of findings values when the finding type is a simple value: `number`, `percentage`, `text`, `integer` | Array | 1 |
| `value` | A finding value | any | 1..* |
| `locations` | The source code locations of the finding | Array | 1 |
| `location` | A source code location | Structure | 1 |
| `location@rank` | The rank order of the location | Integer | 1 |
| `location@sourcePath` | The source code path | String | 0..1 |
| `location@startLine` | The start line of the objects in violation (for finding type: `integer`, `text`), of the bookmark (for `bookmark`), or of the call path (for `path`) | Integer | 0..1 |
| `location@endLine` | The end line of the objects in violation (for finding type: `integer`, `text`), of the bookmark (for `bookmark`), or of the call path (for `path`) | Integer | 0..1 |
| `codeFragment` | The code fragment | String | 0..1 |
| `codeFragment@startLine` | The start line of the bookmark (for finding type `bookmark`) relatively to the code fragment | Integer | 0..1 |
| `codeFragment@endLine` | The end line of the bookmark (for finding type `bookmark`) relatively to the code fragment | Integer | 0..1 |


 
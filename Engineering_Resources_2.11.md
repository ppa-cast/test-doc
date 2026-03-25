# Engineering Resources - 2.11

*Created by N Padmavathi on Jan 31, 2023*

> **Dashboard Service database only**

---

## On this page

- [Glossary](#glossary)
- [OMG Data Functions and Transactions](#omg-data-functions-and-transactions)
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

---

## Glossary

| Term | Definition |
|---|---|
| Action Plan | A set of issues (violations requiring remedial action) |
| Code Fragment | Identifies a file contents extract |
| Component Snapshot | A source code item (class, method, etc.). A defective component violates a quality rule pattern. |
| Defects Summary | Statistics of defects impacting a business criterion for components in a tree node scope |
| Diagnosis Findings | Pinpoint statements or properties of the defective component violating a rule pattern (also known as "associated values") |
| File Contents | Raw text of a source file |
| OMG Functions | Data Functions or Transactions from Function Point analysis |
| Issue | Reports a remedial action in the context of an application snapshot |
| Remedial Action | A user input request to correct a component regarding a quality rule-pattern |
| Transaction | A set of components involved in transaction processing |
| Tree Node | Hierarchy relation of a component (language-dependent). **WARNING: a component may be reached from several tree nodes.** |
| Violation | Identifies a defective component breaking a quality rule pattern. For a given component and rule, there is 0 or 1 violation. Also known as "failed check". |

---

## OMG Data Functions and Transactions

> **Even in case of a restricted license, `authorizations.xml` configuration is applied for these URLs.**

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions` | Array of Functions |
| GET | text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/ifpug-functions-evolution` | Array of Functions (evolution) |
| GET | text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/omg-functions-functional-evolution{?parameters}` | Array of Functions |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| sheet | Sheet to retrieve (CSV only): `context`, `functional`, or `technical` | String | None |

### CSV Representation

| Columns | Description | Type |
|---|---|---|
| Element Type | Data Function or Transaction | String |
| Object Name | Full name | String |
| Nb of FPs | Number of Function Points | Integer |
| FP details | Function Points details | String |
| Object Type | Type of this Function | String |
| Module name | Functional module name | String |
| Technology | Technology | String |
| Transaction Name | Full name of the transaction | String |
| Risk | Risk propagation assessment index | Integer |

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
| security.href | Transactions by risk propagation index for Security | URI | 1 |
| efficiency.href | Transactions by risk propagation index for Efficiency | URI | 1 |
| robustness.href | Transactions by risk propagation index for Robustness | URI | 1 |

---

## Transaction

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/transactions/{BusinessCriterionId}/{?parameters}` | Array of Transactions |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/transactions{?parameters}` | Array of Transactions |
| GET | application/json | `{ApplicationId}/transactions/{TransactionID}/snapshots/{snapshotID}/components/{HealthFactorID}/violations-summary{?parameters}` | Array of Objects |
| GET | application/json | `{ApplicationId}/transactions/{TransactionID}/snapshots/{snapshotID}/components/{HealthFactorID}/object-violations-count{?parameters}` | Array of Objects |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| startRow | First item | Integer | 1 |
| nbRows | Max items | Integer | 10 |
| business-criterion | Business criterion to compute TRI (tree-node transactions only) | 60013, 60014, 60016 | 60017 |
| status | Filter by status | `added`, `updated`, `unchanged` | All |
| critical | Filter by criticality | Boolean | All |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Reference to a transaction | URI | 1 |
| name | Transaction full name | String | 1 |
| shortName | Transaction short name | String | 1 |
| transactionRiskIndex | Risk assessment according to health factor | int | 0..1 |
| nbOfViolations | Number of violations | int | 0..1 |
| treeNodes.href | All tree nodes linked to this component | URI | 0..1 |
| totalObjects / addedObjects / updatedObjects / unchangedObjects | Object counts | String or int | 0..1 |

---

## Tree Node

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/tree-node` | Root tree-node for this application |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/tree-node` | Tree-node for this module |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}` | Tree-node detail |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/children/{?parameters}` | Direct children (ordered by name) |
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/ancestors` | All ancestors (descending node levels) |
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/tree-nodes` | All tree-nodes for this component |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| component | Related component (absent for app/module root) | Structure | 0..1 |
| component.type.name | Component type name (usable as PNG image filename) | String | 1 |
| component.type.label | Component type label (e.g. `"C++ Class"`, `"Java Method"`) | String | 1 |
| defects-summary.href | URI to get defect statistics | URI | 1 |
| children.href | URI to get direct children (undefined for leaf nodes) | URI | 0..1 |
| ancestors.href | URI to get all parents (undefined for root) | URI | 0..1 |

---

## Defects Summary

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/defects-summary{?parameters}` | Summary of defects |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| business-criterion | Business Criterion for statistics | Integer | 60017 (TQI) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| violations.number | Number of violations impacting the business criterion | Integer | 1 |
| defectiveComponents.number | Number of defective components | Integer | 1 |
| violatedRulePatterns.href | URI to list violated rule patterns | URI | 1 |
| violatedRulePatterns.number | Number of violated rule patterns | Integer | 1 |
| criticalViolations.number | Number of critical violations | Integer | 1 |
| defectiveComponentsToCriticalRules.number | Defective components impacting critical rules | Integer | 1 |
| violatedCriticalRulePatterns.number | Number of critical violated rule patterns | Integer | 1 |

---

## Violated Rule Pattern

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violated-rule-patterns{?parameters}` | Array of violated rule patterns ordered by violation count |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| business-criterion | Business criterion to select rule patterns | Integer | 60017 (TQI) |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern.href | Auto reference | URI | 1 |
| violations.name | Name | String | 1 |
| violations.href | Reference to list of violations | URI | 1 |
| violations.number | Number of violations/defective components | Integer | 1 |

---

## Components Directory

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components` | Components directory for an application |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components` | Components directory for a module |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| security.href | Components by risk propagation index for Security | URI | 1 |
| efficiency.href | Components by risk propagation index for Efficiency | URI | 1 |
| robustness.href | Components by risk propagation index for Robustness | URI | 1 |
| transferability.href | Components by risk propagation index for Transferability | URI | 1 |
| changeability.href | Components by risk propagation index for Changeability | URI | 1 |

---

## Component Snapshot

### URI Templates & Parameters

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` | Array of Components filtered by Health Factor |
| GET | application/json, text/csv, xlsx | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/components/{HealthFactorID}/{?parameters}` | Array of Components for a module snapshot |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/components/{DistributionID}/{categoryRank}/{?parameters}` | Components of a distribution for an application snapshot |
| GET | application/json | `{Domain}/applications/{ApplicationID}/components/65005/{?parameters}` | Components for Cost Complexity |
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}` | A single component snapshot |

**Common parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| startRow | First item | Integer | 1 |
| nbRows | Max items | Integer | 10 |
| business-criterion | Health factor ID to compute propagated risk index | String | None |

**Additional parameters for `{HealthFactorID}` URLs**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| properties | Filter components having non-null values for listed properties (e.g. `properties=(cyclomaticComplexity,fanOut)`) | String | None |
| order | Sort order. Sortable columns: `pri`, `component-name`, `component-id`, and all property names | String | PRI desc, then name asc |

**Parameters for Cost Complexity (`65005`)**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| snapshot-ids | Two snapshot IDs for range (e.g. `snapshot-ids=(12,14)`) | Two comma-separated integers | None |
| status | Artifact status between snapshots | `added`, `updated`, `deleted` | None |
| technologies | Technology name to filter | String | None |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Full component name | String | 1 |
| shortName | Short name | String | 1 |
| propagationRiskIndex | Risk propagation index (only with business-criterion) | Integer | 0..1 |
| status | `"added"`, `"updated"`, or `"unchanged"` | String | 0..1 |
| sourceCodes.href | Source code fragments (last snapshot only) | URI | 0..1 |
| type.label | Component type label | String | 0..1 |
| type.name | Component type name (usable as PNG filename) | String | 0..1 |
| treeNodes.href | Tree nodes for this component (last snapshot only) | URI | 0..1 |

**Additional properties (single ComponentSnapshot or tree-node only)**

`codeLines`, `commentedCodeLines`, `commentLines`, `coupling`, `fanIn`, `fanOut`, `cyclomaticComplexity`, `ratioCommentLinesCodeLines`, `halsteadProgramLength`, `halsteadProgramVocabulary`, `halsteadVolume`, `distinctOperators`, `distinctOperands`, `integrationComplexity`, `essentialComplexity`

---

## Code Fragment

*No URI — always embedded in a parent structure.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| file.href | URI to get source file contents | URI | 1 |
| file.name | Path/name of the file | String | 1 |
| file.size | Size of the file (octets) | Integer | 1 |
| startLine | Start line of code fragment | Integer | 1 |
| startColumn | Start column (may be null for "path" findings) | Integer | 1 |
| endLine | End line | Integer | 1 |
| endColumn | End column (exclusive) | Integer | 1 |

---

## File Contents

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | text/plain | `{Domain}/local-sites/{SiteID}/file-contents/{ContentsID}/?{Parameters}` | Source file contents |

**Parameters**

| URI Parameter | Description | Default |
|---|---|---|
| start-line | First line to extract | 1 |
| end-line | Last line to extract | Last line |

---

## Action Plan Summary

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/summary/{?parameters}` | Action plan summary for an application snapshot |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/action-plan/summary/{?parameters}` | Action plan summary for a module snapshot |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| objectSamples | Number of sample objects to display | Integer | 5 |
| rule-pattern | Filter by quality rules | Combination of integers and strings | All |
| status | Filter by status | `added`, `pending`, `solved` | All |
| tag | Filter by tag | String | All |
| comment | Filter by comment | String | All |
| object-fullname | Filter by object full name (contains) | String | All |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern.href | Reference to the rule pattern | URI | 1 |
| objectSamples | Sample defective components | Array | 1 |
| addedIssues | Issues added to action plan in this snapshot | Integer | 1 |
| pendingIssues | Issues selected before this snapshot still pending | Integer | 1 |
| solvedIssues | Issues corrected in the meantime | Integer | 1 |

---

## Remedial Action

*No URI — always embedded in an issue or violation.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | `"added"`, `"pending"`, or `"solved"` | String | 1 |
| dates.updated | Date of last change | Date | 1 |
| dates.solved | Date when solved | Date | 0..1 |
| comment | Short comment from requester | String | 1 |
| priority | `extreme`, `high`, `moderate`, `low` | String | 1 |
| tag | General text intended to replace priority | String | 1 |

---

## Issue

An issue represents a remedial action for a rule pattern and a component in the context of an application snapshot.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}` | Array of issues until this snapshot |
| GET | application/json | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/action-plan/issues/{?Parameters}` | Array of issues for a module |
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Create issues from last application snapshot. Requires `QUALITY_MANAGER` role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Update issues in last application snapshot. Requires `QUALITY_MANAGER` role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/action-plan/issues` | Remove issues. Requires `QUALITY_MANAGER` role. |

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

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| startRow | First item (JSON only) | Integer | 1 |
| nbRows | Max items (JSON only) | Integer | 10 |
| rule-pattern | Filter by quality rules | Combination | All |
| status | Filter by status | `added`, `pending`, `solved` | All |
| tag | Filter by tag | String | All |
| comment | Filter by comment | String | All |
| object-fullname | Filter by object full name | String | All |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Reference to an issue | URI | 1 |
| component | Defective component snapshot | Structure | 1 |
| rulePattern.href | Quality Rule Pattern | URI | 1 |
| remedialAction | Remedial Action | Structure | 0..1 |

---

## Action Plan Trigger

An Action Plan Trigger is composed of a rule pattern and a remedial action. If active, all violations of the corresponding rule will be automatically added to the Action Plan in the next snapshot.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/action-plan/triggers{?Parameters}` | Array of action plan triggers |
| POST | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Create triggers. Requires `QUALITY_AUTOMATION_MANAGER` role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Update triggers. Requires `QUALITY_AUTOMATION_MANAGER` role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/action-plan/triggers` | Remove triggers. Requires `QUALITY_AUTOMATION_MANAGER` role. |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| rulePattern | Rule Pattern | Structure | 1 |
| remedialActionPattern | Remedial Action Pattern. Contains either `priority` or `tag` (the other is null). | Structure | 1 |
| active | Whether the trigger is currently active | Boolean | 1 |

---

## Action Plan Recommendation

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/action-plan-recommendation?{parameters}` | Calculate recommendation to reach a target |
| GET | application/json | `{Domain}/applications/{ApplicationID}/action-plan-recommendation?{parameters}` | Calculate recommendation for a specific application |
| PUT | application/json | `{Domain}/action-plan-recommendation` | Calculate recommendation and create action plan issues automatically |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/action-plan-recommendation` | Same for a specific application |

**Parameters (GET as query params / PUT as payload)**

| Parameter | Description | Type | Default |
|---|---|---|---|
| qualityIndicator / quality-indicator | Business criterion ID to improve | Integer | 60017 (TQI) |
| scoreType / score-type | `"grade"` (1.0–4.0) or `"compliance"` (0.0–1.0) | String | grade |
| goal | `"reach-score-minimizing-violations"`, `"reach-score-minimizing-effort"`, `"reach-violations-count-maximizing-score"`, `"reach-total-effort-maximizing-score"` | String | reach-score-minimizing-violations |
| target | Grade, compliance, violation count, or effort (minutes) | Decimal | 4.0 for grade |
| application / application-name | Application name (first URI template) | String | Default if single app |
| append | `false` = ignore current plan; `true` = include it. **Warning**: PUT with `append=true` removes all current issues first. | Boolean | false |

### JSON Representation of GET Response

| Properties | Description | Type | Occurs |
|---|---|---|---|
| goal | The goal of this action plan | String | 0..1 |
| qualityIndicator | Quality indicator to improve | Structure | 1 |
| scoreType | `grade` or `compliance` | String | 0..1 |
| score | Target score when violations are fixed | Decimal | 1 |
| totalViolations | Number of violations to fix | Integer | 1 |
| totalEffort | Remediation effort (minutes) | Decimal | 1 |
| actionPlan | List of rule remediations | Array | 1 |
| actionPlan[].qualityRule | Quality Rule | Structure | 1 |
| actionPlan[].critical | True if rule is critical | Boolean | 1 |
| actionPlan[].violationsCount | Violations to fix | Integer | 1 |
| actionPlan[].unitEffort | Effort per occurrence (minutes) | Integer | 1 |
| actionPlan[].effort | Total effort for this rule (minutes) | Integer | 1 |
| actionPlan[].components | List of components | Structure | 0..* |

---

## Exclusion Request Detail

*No URI — always embedded in an exclusion request or a violation.*

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| status | `"added"`, `"processed"`, or `"unbound"` | String | 1 |
| userName | User who created/updated the request | String | 1 |
| comment | Comment (e.g. "False Positive") | String | 1 |
| dates.updated | Creation date or date of last update | Date | 1 |

---

## Exclusion Request

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| POST | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Create exclusion requests (comment required). Requires `EXCLUSION_MANAGER` role. |
| PUT | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Update exclusion request comments. Requires `EXCLUSION_MANAGER` role. |
| DELETE | application/json | `{Domain}/applications/{ApplicationID}/exclusions/requests` | Delete exclusion requests. Requires `EXCLUSION_MANAGER` role. |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| component | Defective component snapshot | Structure | 1 |
| rulePattern.href | Quality Rule Pattern | URI | 1 |
| exclusionRequest.status | `"added"`, `"processed"`, or `"unbound"` | String | 1 |
| exclusionRequest.userName | Requester user name | String | 1 |
| exclusionRequest.comment | Comment | String | 0..1 |
| exclusionRequest.dates.updated | Date of last update | Date | 1 |

---

## Exclusion Summary

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/excluded-violations-summary` | Exclusion summary for this snapshot |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Name | String | 1 |
| excludedViolations.number | Number of excluded violations | Integer | 1 |

---

## Scheduled Exclusion

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/exclusions/scheduled{?Parameters}` | Scheduled exclusions for this snapshot |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| status | `to-add`, `to-remove`, or `$all` | String | `$all` |
| rule-pattern | Filter by quality rules | Combination | All |
| startRow | First item (JSON only) | Integer | 1 |
| nbRows | Max items (JSON only) | Integer | 10 |

### CSV Representation

| Columns | Description | Type |
|---|---|---|
| Exclusion status | `"to add"` or `"to remove"` | String |
| Exclusion requester | EXCLUSION_MANAGER user name | String |
| Exclusion request comment | Comment | String |
| Quality rule name | Name of the quality rule | String |
| Object name location | Full name of the object | String |
| Exclusion request last update | Date of last update | String |
| Metric Id | Quality Rule ID | Integer |

---

## Scheduled Exclusions Summary

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/exclusions/scheduled-summary{?Parameters}` | Scheduled exclusions summary |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| scheduledExclusionsToAdd.number | Exclusions to add in next snapshot | Integer | 1 |
| scheduledExclusionsToRemove.number | Exclusions to remove in next snapshot | Integer | 1 |
| scheduledExclusions.number | Total scheduled exclusions | Integer | 1 |

---

## Violation

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json, text/csv, xlsx | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/violations{?Parameters}` | 0 or 1 violation for this component |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations{?Parameters}` | All violations for this application snapshot |
| GET | application/json, text/csv, xlsx | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations{?Parameters}` | All violations for this module snapshot |
| GET | application/json, text/csv, xlsx | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations{?Parameters}` | Violations in scope of this tree node |
| GET | application/json, text/csv, xlsx | `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations{?Parameters}` | Violations in scope of this transaction |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/excluded-violations{?Parameters}` | Effectively excluded violations |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations{?Parameters}` | Violations removed between this and previous snapshot |
| GET | application/json | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations{?Parameters}` | Violations between two layers |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| startRow | First item (JSON only) | Integer | 1 |
| nbRows | Max items (JSON only) | Integer | 10 |
| rule-pattern | Quality Rules filter. Syntaxes: `c:TCID`, `cc:BCID`, `nc:BCID`, `bqi:BCID`, `critical-rules`, `quality-rules`, rule pattern ID | Combination | All |
| business-criterion | Health factor ID for PRI computation | Integer | None |
| technologies | Technology filter | String | None |
| status | Status filter (`added`, `updated`, `unchanged`) | String | None |
| caller-layer / callee-layer | Layer names for Architecture Models | String | None |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| component | Defective component snapshot | Structure | 1 |
| rulePattern.href | Quality Rule Pattern (includes `critical` for app/module violations) | URI | 1 |
| remedialAction | Remediation action request (exclusive with `exclusionRequest`) | Structure | 0..1 |
| diagnosis.status | `"added"`, `"updated"`, or `"unchanged"` | String | 0..1 |
| diagnosis.findings.ref | Reference to diagnosis findings | URI | 0..1 |
| exclusionRequest | Exclusion request (exclusive with `remedialAction`) | Structure | 0..1 |

---

## Indexed Violation

Allows filtering violations with parameters using a dedicated Lucene index.

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| PUT | application/json | `{Domain}/violations-index` | Create the Lucene violations index *(Admin only)* |
| GET | application/json | `{Domain}/violations-index` | Get index information |
| GET | application/json, text/csv, xlsx | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/indexed-violations{?Parameters}` | Violations filtered by Parameters |

**Parameters** (all optional)

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| startRow / nbRows | Pagination | Integer | 1 / 10 |
| rule-pattern | See Violation rule-pattern | Combination | All |
| weight | Filter by weight (1–9) | Integer | All |
| critical | Filter by criticality | Boolean | All |
| business-criterion | Health factor for PRI | Integer | None |
| status | Violation status | String | None |
| object-status | Component status (`added`, `updated`, `unchanged`) | String | None |
| technologies / modules / transactions | Filter by technology, module, or transaction | String/Integer | None |
| object-fullname | Search in component name | String | None |
| order | Sort order. Values: `action-exclusion-status`, `component-name`, `rule-pattern-name`, `diagnosis-status`. Use `asc(...)` or `desc(...)`. | String | PRI desc, rule name asc, object name asc |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| number | Total violations matching the filter | Integer | 1 |
| violations | Filtered violations | Array | 1 |

### Violations Summary

| HTTP Action | URI Templates | Description |
|---|---|---|
| GET | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations summary for an application snapshot |
| GET | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/removed-violations-summary{?Parameters}` | Removed violations summary |
| GET | `{Domain}/applications/{ApplicationID}/snapshots/{SnapshotID}/architecture-models/{ModelID}/violations-summary{?Parameters}` | Violations summary for architecture model |
| GET | `{Domain}/modules/{ModuleID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations summary for module |
| GET | `{Domain}/tree-nodes/{Level}-{LowerID}-{UpperID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations summary for tree node |
| GET | `{Domain}/transactions/{TransactionID}/snapshots/{SnapshotID}/violations-summary{?Parameters}` | Violations summary for transaction |

**Parameters**

| URI Parameter | Description | Values | Default |
|---|---|---|---|
| rule-pattern | Rule pattern ID | Integer or string | None (mandatory) |
| technologies | Technology filter | String | None |
| status | Status filter (`added`, `updated`, `unchanged`). Omit for total count. | String | None |

---

## OMG Technical Debt

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/omg-technical-debt` | Technical Debt details of a violation |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| total | Total Remediation Effort (time to remove occurrences, including coding + testing) | Integer | 1 |
| numberOccurrences | Number of pattern occurrences | Integer | 1 |
| complexity | Effort Complexity (AEP specification) | Integer | 1 |
| exposure | Connectedness of the occurrence with the rest of the software | Integer | 1 |
| concentration | Number of occurrences within any code element | Integer | 1 |
| technologicalDiversity | Number of distinct programming languages in one occurrence | Integer | 1 |
| gapSize | Extent of the gap between values and thresholds | Integer | 1 |
| unadjustedEffort | Effort digest before adjustment factor | Integer | 1 |
| added | Technical Debt of added occurrences | Integer | 1 |
| removed | Technical Debt of removed occurrences | Integer | 1 |
| adjustmentFactor | Factor applied to compute Remediation Effort from Unadjusted Effort | Integer | 1 |

---

## Diagnosis Findings

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/components/{ComponentID}/snapshots/{SnapshotID}/findings/{RuleID}` | Findings of a violation |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| name | Name of findings items | String | 1 |
| type | Item type: `integer`, `object`, `path`, `group`, `percentage`, `text`, or `null` | String | 1 |
| values | Array of findings items. For `integer` and `percentage`, contains a single value. | Array | 0..1 |
| bookmarks | Array of bookmark arrays (last snapshot only) | Array | 0..1 |
| bookmarks[][].component | Component of the code fragment | Structure | 1 |
| bookmarks[][].codeFragment | Code fragment (last snapshot only) | Structure | 0..1 |
| parameters | Array of parameters | Array | 0..1 |
| parameters[].name | Parameter name | String | 1 |
| parameters[].values[] | Parameter default value | Any | 1..* |

**Type-specific properties:**

- **`object`**: `values[].component.codeSources` — source codes (last snapshot only)
- **`path`**: `values[][].component`, `values[][].codeFragment`, `values[][].level` — execution code level
- **`group`**: `values[][].component.sourceCodes` — source codes of group member (last snapshot only)

### JSON Examples

**Integer type** (violation count with bookmarks)

```json
{
  "name": "Number of Violations Patterns",
  "type": "integer",
  "values": [2],
  "bookmarks": [
    [{ "component": { "name": "..." }, "codeFragment": {} }],
    [{ "component": { "name": "..." }, "codeFragment": {} }]
  ]
}
```

**Text type** (undocumented parameters)

```json
{
  "name": "Undocumented parameters",
  "type": "text",
  "values": ["param1", "param2"],
  "bookmarks": []
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
    ]
  ],
  "bookmarks": []
}
```

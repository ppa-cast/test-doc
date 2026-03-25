# Quality and Sizing Model Resources - 2.11

*Created by N Padmavathi on Jan 31, 2023*

---

## On this page

- [Glossary](#glossary)
- [Configuration Snapshot](#configuration-snapshot)
- [Remediation Efforts](#remediation-efforts)
- [Configuration Models](#configuration-models)
- [Collection of Configuration Items](#collection-of-configuration-items)
- [Business Criterion](#business-criterion)
- [Technical Criterion](#technical-criterion)
- [Indirect Grade Contributors](#indirect-grade-contributors)
- [Quality Rule](#quality-rule)
- [Quality Distribution](#quality-distribution)
- [Quality Measure](#quality-measure)
- [Sizing Measure](#sizing-measure)
- [Background Fact](#background-fact)
- [Rule Pattern](#rule-pattern)
- [Distribution Pattern](#distribution-pattern)
- [Measure Pattern](#measure-pattern)

---

## Glossary

| Term | Definition |
|---|---|
| Background Fact | An external information that can be related to Quality Indicators or Sizing Measures |
| Business Criterion | A Quality Criterion graded based on contributing technical criteria and critical violations |
| Category | A Quality Distribution slot. 4 categories defined per Quality Distribution. |
| Configuration Item | A Quality Indicator, Sizing Measure, or Background Fact implemented as a calculation rule |
| Distribution Pattern | Definition of the object property used to assign objects to distribution categories, with documentation |
| Grade Aggregator | An impacted Quality Indicator (see Grade Contributor) |
| Grade Contributor | A Quality Indicator whose measure is used to compute another Quality Indicator |
| Indirect Contributing Quality Indicator | A Quality Rule, Distribution, or Measure contributing to a Business Criterion |
| Measure Pattern | Definition of the measure to perform on each module, with documentation |
| Quality Indicator | A grade between 1.0 (very high risk) and 4.0 (low risk) to assess source code quality |
| Quality Distribution | A Quality Criterion based on a distribution pattern |
| Quality Rule | A Quality Criterion assessing compliance with a Rule Pattern |
| Quality Measure | A Quality Criterion based on a measure pattern |
| Rule Pattern | The pattern searched for in analysis results to pinpoint violations, with documentation |
| Sizing Measure | A quantitative measure |
| Technical Criterion | A Quality Indicator graded based on contributing Quality Rules, Distributions, and Measures |

---

## Configuration Snapshot

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/snapshots` | Array of configuration snapshots |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}` | A configuration snapshot content |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Configuration name | String | 1 |
| number | Snapshot order number | Integer | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | `"EFP"` or `"AEP"` | String | 1 |
| qualityIndicators.href | Reference to get a group of indicators | URI | 1 |
| sizingMeasures.href | Reference to get a group of sizing measures | URI | 1 |
| backgroundFacts.href | Reference to get background facts | URI | 1 |

### JSON Example

**GET DEMO/configuration/snapshots/5**

```json
{
  "href": "DEMO/configuration/snapshots/5",
  "name": "Configuration Snapshot #1",
  "number": 1,
  "annotation": {
    "version": "1.0",
    "date": { "time": 1338328800000 },
    "description": null,
    "name": "GM_DRAS_201206180516",
    "enhancementMeasure": "EFP"
  },
  "qualityIndicators": {
    "href": "DEMO/configuration/snapshots/5/quality-indicators",
    "name": "All Quality Indicators"
  },
  "sizingMeasures": {
    "href": "DEMO/configuration/snapshots/5/sizing-measures",
    "name": "All Sizing Measures"
  },
  "backgroundFacts": {
    "href": "DEMO/configuration/snapshots/5/background-facts",
    "name": "All Background Facts"
  }
}
```

---

## Remediation Efforts

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/remediation-efforts` | Array of Remediation Efforts. Returns 400 if not found, 404 if schema is prior to AIP 8.3.33. |
| PUT | application/json | `{Domain}/configuration/remediation-efforts` | Replace the remediation effort for some rules. Payload must contain at least `key` and `remediationEffort`. |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| key | Rule key | String | 1 |
| rulePattern.href | URI to the rule pattern | String | 1 |
| rulePattern.name | Rule name | String | 1 |
| remediationEffort | Workload in Man x Minutes for one occurrence. Priority: specific effort > CISQ default (extension) > CISQ default (tagging) > AIP default | Number | 1 |
| applicable | True if the rule is applicable for the latest snapshot | Boolean | 1 |

---

## Configuration Models

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-indicators` | Quality Model |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/sizing-measures` | Sizing Model |

### JSON Representation

**Quality Model**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| businessCriteria.href | All Business Criteria | URI | 1 |
| technicalCriteria.href | All Technical Criteria | URI | 1 |
| qualityRules.href | All Quality Rules | URI | 1 |
| qualityDistributions.href | All Quality Distributions | URI | 1 |
| qualityMeasures.href | All Quality Measures | URI | 1 |

**Sizing Model**

| Properties | Description | Type | Occurs |
|---|---|---|---|
| technicalSizeMeasures.href | All Technical Size Measures | URI | 1 |
| functionalWeightMeasures.href | All Functional Weight Measures | URI | 1 |
| criticalViolationStatistics.href | All Critical Violation Statistics | URI | 1 |
| violationStatistics.href | All Violation Statistics | URI | 1 |
| technicalDebtStatistics.href | All Technical Debt Statistics | URI | 1 |
| runtimeStatistics.href | All Run-time Statistics | URI | 1 |

---

## Collection of Configuration Items

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/business-criteria` | Array of Business Criteria |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-criteria` | Array of Technical Criteria |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-rules` | Array of Quality Rules |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-distributions` | Array of Quality Distributions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-measures` | Array of Quality Measures |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-size-measures` | Array of Technical Size Measures |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/functional-weight-measures` | Array of Functional Weight Measures |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/critical-violation-statistics` | Array of Critical Violation Statistics |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/violation-statistics` | Array of Violation Statistics |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-debt-statistics` | Array of Technical Debt Statistics |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/run-time-statistics` | Array of Run-time Statistics |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/background-facts` | Array of Background Facts |

---

## Business Criterion

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}` | A snapshoted Quality Indicator definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| key | Metric ID | String | 1 |
| name | Business Criterion name | String | 1 |
| description | Metric description | String | 0..1 |
| type | Type | String | 1 |
| annotation | User annotation | Structure | 1 |
| number | Snapshot order number | Integer | 1 |
| gradeAggregators | Empty array | Array | 1 |
| gradeContributors | Array of contributing Technical Criteria | Array | 1 |
| gradeContributors[].href | Contributing Technical Criterion | URI | 1 |
| baseQualityIndicators.href | Reference to indirect contributing quality indicators | URI | 1 |

### JSON Example

```json
{
  "href": "DEMO/quality-indicators/66032/snapshots/5",
  "key": "66032",
  "name": "Architectural Design",
  "description": "Architectural Design",
  "type": "business-criteria",
  "annotation": {
    "version": "1.0",
    "date": { "time": 1338328800000 },
    "name": "GM_DRAS_201206180516",
    "enhancementMeasure": "EFP"
  },
  "number": 1,
  "gradeAggregators": [],
  "gradeContributors": [
    {
      "href": "DEMO/quality-indicators/66070/snapshots/5",
      "key": "66070",
      "name": "Architecture - Architecture Models Automated Checks",
      "weight": 1,
      "critical": false
    }
  ],
  "baseQualityIndicators": {
    "href": "DEMO/quality-indicators/66032/snapshots/5/base-quality-indicators",
    "name": "All indirect grade contributors"
  }
}
```

---

## Technical Criterion

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}` | A snapshoted Quality Indicator definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Quality indicator name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| gradeAggregators[].href | Grade Aggregator reference | URI | 1 |
| gradeAggregators[].name | Grade Aggregator name | String | 1 |
| gradeAggregators[].key | Grade Aggregator key | String | 1 |
| gradeContributors.href | Reference to contributing Quality Indicators | URI | 1 |
| rationale | Text | String | 0..1 |
| description | Text | String | 0..1 |
| snapshots.href | Reference to history | URI | 1 |
| annotation | User annotation | Structure | 1 |

---

## Indirect Grade Contributors

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}/base-quality-indicators` | Array of Indirect Contributing Quality Indicators |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Quality Indicator reference | URI | 1 |
| key | Quality Indicator key | String | 1 |
| name | Quality Indicator name | String | 1 |
| compoundedWeight | Sum of all contributing path weights from the QI to the Business Criterion | Integer | 1 |
| compoundedWeightFormula | Explicit formula, e.g. `"(8x4)+(4x5)"` | String | 1 |
| critical | True if the QI is critical for at least one technical criterion | Boolean | 1 |

---

## Quality Rule

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}` | A snapshoted Quality Indicator definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Quality rule name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| rulePattern | Pattern | URI | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| gradeSettings.thresholds | 4 percentage thresholds for grade formula | Array | 1 |
| snapshots.href | Reference to history | URI | 1 |
| annotation | User annotation | Structure | 1 |
| parameters[].name | Parameter name (threshold to distribute objects) | String | 1 |
| parameters[].technology | Applicable technology | String | 1 |
| parameters[].value | Threshold value | Number or String | 1 |

---

## Quality Distribution

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}` | A snapshoted Quality Indicator definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Distribution name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| distributionPattern | Pattern | URI | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| gradeSettings.categories | 4 ordered categories | Array | 1 |
| gradeSettings.categories[].name | Category name | String | 1 |
| gradeSettings.categories[].thresholds | 4 thresholds for grade formula | Array | 1 |
| snapshots.href | Reference to history | URI | 1 |
| annotation | User annotation | Structure | 1 |
| parameters[].name | Parameter name | String | 1 |
| parameters[].technology | Applicable technology | String | 1 |
| parameters[].value | Threshold value | Number | 1 |

---

## Quality Measure

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/quality-indicators/{metricID}/snapshots/{snapshotID}` | A snapshoted Quality Indicator definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Quality measure name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| measurePattern | Pattern | URI | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| snapshots.href | Reference to history | URI | 1 |
| annotation | User annotation | Structure | 1 |

---

## Sizing Measure

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/sizing-measures/{metricID}/snapshots/{snapshotID}` | A snapshoted Sizing Measure definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Sizing measure name | String | 1 |
| description | Text | String | 1 |
| type | Type | String | 1 |
| annotation | User annotation | Structure | 1 |
| number | Snapshot order number | Integer | 1 |

---

## Background Fact

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/background-facts/{metricID}/snapshots/{snapshotID}` | A snapshoted Background Fact definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Background fact name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Always `"background-facts"` | String | 1 |
| description | Text | String | 0..1 |
| snapshots.href | Reference to history | URI | 1 |
| annotation | User annotation | Structure | 1 |

---

## Rule Pattern

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/rule-patterns/{metricID}` | A Rule Pattern definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Pattern name | String | 1 |
| key | Pattern public identifier | String | 1 |
| rationale | Text | String | 1 |
| description | Text | String | 1 |
| remediation | Text | String | 1 |
| reference | External reference | String | 0..1 |
| sample | Code snippet of a bad practice | String | 0..1 |
| remediationSample | Code snippet of a remediation | String | 0..1 |
| output | Diagnosis findings report | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| total | Definition of the tested components | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |
| qualityStandards[].standard | Quality Standard name | String | 1 |
| qualityStandards[].id | Quality Standard reference ID | String | 1 |
| qualityStandards[].reference | URL to external documentation | String | 1 |

### JSON Example

```json
{
  "href": "DEMO/rule-patterns/7298",
  "key": "7298",
  "name": "A class that has pointer data members must provide a copy constructor",
  "description": "The report list all...",
  "technologies": ["C++"],
  "rationale": "If you don't define a copy constructor...",
  "reference": null,
  "remediation": "Define a copy constructor to properly manage pointer data members.",
  "sample": "class MyClass {\n   char * apointermember;\n};",
  "qualityStandards": [
    {
      "standard": "CISQ",
      "id": "ASCMM-MNT-15",
      "name": "Public Member Element",
      "reference": null,
      "description": null
    }
  ]
}
```

---

## Distribution Pattern

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/distribution-patterns/{metricID}` | A Distribution Pattern definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Pattern name | String | 1 |
| key | Pattern public identifier | String | 1 |
| rationale | Text | String | 1 |
| description | Text | String | 1 |
| remediation | Text | String | 1 |
| reference | External reference | String | 0..1 |
| output | Diagnosis findings report | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |

---

## Measure Pattern

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/measure-patterns/{metricID}` | A Measure Pattern definition |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Pattern name | String | 1 |
| key | Pattern public identifier | String | 1 |
| rationale | Text | String | 1 |
| description | Text | String | 1 |
| remediation | Text | String | 1 |
| reference | External reference | String | 0..1 |
| output | Deprecated | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| total | Definition of the tested components | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |

### JSON Example

```json
{
  "href": "DEMO/measure-patterns/66067",
  "key": "66067",
  "name": "Avoid High Volume of Copy Pasted Code",
  "description": "This metric is based on the ratio...",
  "technologies": [],
  "rationale": "A program with a lot of duplication...",
  "reference": null,
  "remediation": null,
  "output": null,
  "associatedValueName": null,
  "total": "Artifacts larger than 10 lines of code (default value of the CODELINE parameter)"
}
```

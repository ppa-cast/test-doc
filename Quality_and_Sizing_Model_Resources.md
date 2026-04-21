# Quality and Sizing Model Resources - 2.12

*Created by James Hurrell on Apr 4, 2024*

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
| Business Criterion | A Quality Criterion which grade is based upon contributing technical criteria grades and critical violations |
| Category | A Quality Distribution slot. There are 4 categories defined for each Quality Distribution. A category measure is an input value to compute the Quality Distribution grade. |
| Configuration Item | A Quality Indicator, Sizing Measure of Background Fact, implemented as calculation rule to measure an application or a module. |
| Distribution Pattern | The Distribution Pattern is the definition of the Object property to use to decide in which distribution category to assign Objects. It also includes the associated documentation: description, rationale, reference, remediation, … |
| Grade Aggregator | An impacted Quality Indicator (see Grade Contributor) |
| Grade Contributor | A Quality Indicator which measure is used to compute another Quality Indicator (see Grade Aggregator) |
| Indirect Contributing Quality Indicator | A Quality Rule, Quality Distribution or Quality Measure contributing to a Business Criterion |
| Measure Pattern | The Measure Pattern is the definition of the measure to perform on each Module. It also includes the associated documentation: description, rationale, reference, remediation, … |
| Quality Indicator | An calculation measure which unit is a grade between 1.0 (very high risk) and 4.0 (low risk) to assess a source code quality. |
| Quality Distribution | A Quality Criterion based on a distribution pattern |
| Quality Rule | A Quality Criterion to assess compliance of a source code with a Rule Pattern. |
| Quality Measure | A Quality Criterion based on a measure pattern |
| Rule Pattern | The Rule Pattern is the pattern that is searched for in the analysis results (source code, cartography, etc.) to pinpoint Violations. It also includes the associated documentation: description, rationale, reference, remediation, … |
| Sizing Measure | A quantitative measure |
| Technical Criterion | A Quality Indicator which grade is based upon contributing Quality Rule , Quality Distribution and Quality Measures grades. |

---

## Configuration Snapshot

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/snapshots` | Array of Configurations snapshots |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}` | A configuration snapshot content |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| href | Auto reference | URI | 1 |
| name | Configuration name | String | 1 |
| number | Snapshot order number. Snapshots are ordered according to the annotation.date | Integer | 1 |
| annotation | User annotations describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |
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
		"date": {
			"time": 1338328800000
		},
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
| GET | application/json | `{Domain}/configuration/remediation-efforts` | Array of Remediation Efforts. If the web service does not exist the response status is 400. If the web service targets a schema prior to AIP 8.3.33, the response status is 404. |
| PUT | application/json | `{Domain}/configuration/remediation-efforts` | Replace the remediation effort for some rules. Payload must contain at least the rule `"key"` and the new `"remediationEffort"` value. |

### JSON Representation

| Properties | Description | Type | Occurs |
|---|---|---|---|
| key | Rule Key | String | 1 |
| rulePattern | Reference to the rule pattern | URI | 1 |
| rulePattern.href | URI to the rule pattern | String | 1 |
| rulePattern.name | Rule Name | String | 1 |
| remediationEffort | A workload in Man x Minutes of a violation occurrence remediation.<br/><br/>This value is selected as follow with a priority order:<br/>- a specific remediation effort for this rule<br/>- a CISQ Default effort, deduced from the attachment of the rule to the CISQ Assessment model extension<br/>- a CISQ Default effort deduced from the CISQ tagging of the rule by the Quality Standard Mapping extension<br/>- a AIP Default effort deduced from attachment of the rule to a technical criterion. | Number | 1 |
| applicable | True if the rule is applicable for the latest snapshot of this domain. | Boolean | 1 |

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
| businessCriteria.href | All Business criteria | URI | 1 |
| technicalCriteria.href | All Technical criteria. | URI | 1 |
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

### JSON Example

**GET DEMO/configuration/snapshots/5/quality-indicators**

```json
{
	"businessCriteria": {
		"href": "DEMO/configuration/snapshots/5/business-criteria",
		"name": "All Business Criteria"
	},
	"technicalCriteria": {
		"href": "DEMO/configuration/snapshots/5/technical-criteria",
		"name": "All Technical Criteria"
	},
	"qualityRules": {
		"href": "DEMO/configuration/snapshots/5/quality-rules",
		"name": "All Quality Rules"
	},
	"qualityDistributions": {
		"href": "DEMO/configuration/snapshots/5/quality-distributions",
		"name": "All Quality Distributions"
	},
	"qualityMeasures": {
		"href": "DEMO/configuration/snapshots/5/quality-measures",
		"name": "All Quality Measures"
	}
}
```

---

## Collection of Configuration Items

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/business-criteria` | Array of references to Quality Indicators definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-criteria` | Array of references to Quality Indicators definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-rules` | Array of references to Quality Indicators definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-distributions` | Array of references to Quality Indicators definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/quality-measures` | Array of references to Quality Indicators definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-size-measures` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/functional-weight-measures` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/critical-violation-statistics` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/violation-statistics` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/technical-debt-statistics` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/run-time-statistics` | Array of references to Sizing Measures definitions |
| GET | application/json | `{Domain}/configuration/snapshots/{snapshotID}/background-facts` | Array of references to Background Facts definitions |

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
| key | Metric Id | String | 1 |
| name | Business Criterion name | String | 1 |
| description | Metric description | String | 0..1 |
| type | Type | String | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |
| number | Snapshot order number | Integer | 1 |
| gradeAggregators | An empty array | Array | 1 |
| gradeContributors | An array of contributing Technical Criteria | Array | 1 |
| gradeContributors[].href | A contributing technical criteria | URI | 1 |
| baseQualityIndicators.href | Reference to get an array of indirect contributing quality indicators | URI | 1 |

### JSON Example

**GET DEMO/quality-indicators/66032/snapshots/5 (Business Criterion)**

```json
{
	"href": "DEMO/quality-indicators/66032/snapshots/5",
	"key": "66032",
	"name": "Architectural Design",
	"description": "Architectural Design",
	"type": "business-criteria",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
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
		},...
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
| gradeContributors.href | Reference to get an array of contributing Quality Indicators | URI | 1 |
| rationale | Text | String | 0..1 |
| description | Text | String | 0..1 |
| snapshots.href | Reference to get history of this item | URI | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |

### JSON Example

**GET DEMO/quality-indicators/61029/snapshots/5 (Technical Criterion)**

```json
{
	"href": "DEMO/quality-indicators/61029/snapshots/5",
	"key": "61029",
	"name": "Complexity - Dynamic Instantiation",
	"description": "Respect of practices regarding dynamic instantiation",
	"type": "technical-criteria",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
	"gradeAggregators": [
		{
			"href": "DEMO/quality-indicators/60012/snapshots/5",
			"key": "60012",
			"name": "Changeability"
		},
		{
			"href": "DEMO/quality-indicators/60014/snapshots/5",
			"key": "60014",
			"name": "Efficiency "
		},...
	],
	"gradeContributors": [
		{
			"href": "DEMO/quality-indicators/2572/snapshots/5",
			"key": "2572",
			"name": "Avoid declaring VB Variables without typing them"
		},...
	]
}
```

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
| compoundedWeight | A contributing weight of a quality indicator is the sum of all contributing path weights from the quality indicator to the business criterion. A contributing path weight is the product: contributing weight from the quality indicator to the technical criterion x contributing weight of the technical criterion to the business criterion | Integer | 1 |
| compoundedWeightFormula | Explicit formula used to calculate the compounded weight Ex: `"(8x4)+(4x5)"` | String | 1 |
| critical | Boolean if the quality indicator is critical for at least one technical criterion | Boolean | 1 |

### JSON Example

**GET DEMO/quality-indicators/66032/snapshots/5/base-quality-indicators**

```json
[
	{
		"href": "DEMO/quality-indicators/7140/snapshots/5",
		"key": "7140",
		"name": "Action artifacts should not directly call a JSP page",
		"compoundedWeight": 3,
		"compoundedWeightFormula": "(1x3)",
		"critical": true
	},
	{
		"href": "DEMO/quality-indicators/7144/snapshots/5",
		"key": "7144",
		"name": "Action Artifacts should not directly use database objects",
		"compoundedWeight": 7,
		"compoundedWeightFormula": "(1x7)",
		"critical": true
	},...
]
```

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
| gradeAggregators[].href | Grade Aggregator reference | URI | 1 |
| gradeAggregators[].name | Grade Aggregator name | String | 1 |
| gradeAggregators[].key | Grade Aggregator key | String | 1 |
| gradeSettings.thresholds | Thresholds used in grade formula to transform a quality rule compliance ratio into a grade. | Array | 1 |
| gradeSettings.thresholds[] | A percentage threshold | Integer | 4 |
| snapshots.href | Reference to get history of this item | URI | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |
| parameters[] | | Array | 1 |
| parameters[].name | Parameter name; a threshold to distribute objects into categories | String | 1 |
| parameters[].technology | Applicable technology | String | 1 |
| parameters[].value | The threshold value | Number or String | 1 |

### JSON Example

**GET DEMO/quality-indicators/7298/snapshots/5 (Quality Rule)**

```json
{
	"href": "DEMO/quality-indicators/7298/snapshots/5",
	"key": "7298",
	"name": "A class that has pointer data members must provide a copy constructor",
	"description": "The report list...",
	"type": "quality-rules",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
	"gradeAggregators": [
		{
			"href": "DEMO/quality-indicators/66069/snapshots/5",
			"key": "66069",
			"name": "Programming Practices - Unexpected Behavior"
		}
	],
	"gradeContributors": [],
	"thresholds": [
		98,
		99,
		99.5,
		99.99
	],
	"rulePattern": {
		"href": "DEMO/rule-patterns/7298",
		"name": "A class that has pointer data members must provide a copy constructor"
	},
	"parameters": [
		{
			"name": "Maximum Line Count",
			"technology": "HTML5",
			"value": 100
		},
		{
			"name": "Maximum Line Count",
			"technology": "JEE",
			"value": 100
		},
		{
			"name": "Maximum Line Count",
			"technology": "PL/SQL",
			"value": 50
		}
	]
}
```

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
| name | Quality rule name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| distributionPattern | Pattern | URI | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| gradeAggregators[].href | Grade Aggregator reference | URI | 1 |
| gradeAggregators[].name | Grade Aggregator name | String | 1 |
| gradeAggregators[].key | Grade Aggregator key | String | 1 |
| gradeSettings.categories | 4 ordered categories | Array | 1 |
| gradeSettings.categories[] | A category (a distribution slot) | Structure | 0..* |
| gradeSettings.categories[].name | Category name | String | 1 |
| gradeSettings.categories[].thresholds | 4 thresholds | Array | 1 |
| gradeSettings.categories[].thresholds[] | A threshold used in grade formula to transform a category measure into a category grade | Double | 0..4 |
| snapshots.href | Reference to get history of this item | URI | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |
| parameters[] | - | Array | 1 |
| parameters[].name | Parameter name; a threshold to distribute objects into categories | String | 1 |
| parameters[].technology | Applicable technology | String | 1 |
| parameters[].value | The threshold value | Number | 1 |

### JSON Example

**GET DEMO/quality-indicators/65601/snapshots/5 (Quality Distribution)**

```json
{
	"href": "DEMO/quality-indicators/65601/snapshots/5",
	"key": "65601",
	"name": "4GL Complexity Distribution",
	"description": "Distribution of Forms regarding their complexity",
	"type": "quality-distributions",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
	"gradeAggregators": [
		{
			"href": "DEMO/quality-indicators/61009/snapshots/5",
			"key": "61009",
			"name": "Complexity - Algorithmic and Control Structure Complexity"
		},
		{
			"href": "DEMO/quality-indicators/61026/snapshots/5",
			"key": "61026",
			"name": "Complexity - Technical Complexity"
		}
	],
	"gradeContributors": [],
	"categories": [
		{
			"name": "Very High 4GL Complexity Forms",
			"thresholds": [
				5,
				4,
				2,
				0
			]
		},...
	],
	"distributionPattern": {
		"href": "DEMO/distribution-patterns/65601",
		"name": "4GL Complexity Distribution"
	},
	"parameters": [
		{
			"name": "Heavy Forms threshold",
			"technology": ".Net",
			"value": 20
		},
		{
			"name": "Heavy Forms threshold",
			"technology": "C#",
			"value": 20
		},
		...
	]
}
```

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
| name | Quality rule name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Type | String | 1 |
| measurePattern | Pattern | URI | 1 |
| gradeAggregators | Indicators depending on this indicator | Array | 0..1 |
| gradeAggregators[].href | Grade Aggregator reference | URI | 1 |
| gradeAggregators[].name | Grade Aggregator name | String | 1 |
| gradeAggregators[].key | Grade Aggregator key | String | 1 |
| snapshots.href | Reference to get history of this item | URI | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |

### JSON Example

**DEMO/quality-indicators/66067/snapshots/5 (Quality Measure)**

```json
{
	"href": "DEMO/quality-indicators/66067/snapshots/5",
	"key": "66067",
	"name": "Avoid High Volume of Copy Pasted Code",
	"description": "This metric is based...",
	"type": "quality-measures",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
	"gradeAggregators": [
		{
			"href": "DEMO/quality-indicators/66009/snapshots/5",
			"key": "66009",
			"name": "Architecture - Reuse"
		}
	],
	"gradeContributors": [],
	"measurePattern": {
		"href": "DEMO/measure-patterns/66067",
		"name": "Avoid High Volume of Copy Pasted Code"
	}
}
```

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
| name | Quality rule name | String | 1 |
| description | Text | String | 1 |
| type | Type | String | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.date | Snapshot date | Date | 1 |
| annotation.version | Assessment version number | String | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |
| number | Snapshot order number | Integer | 1 |

### JSON Example

**GET DEMO/sizing-measures/68001/snapshots/5**

```json
{
	"href": "DEMO/sizing-measures/68001/snapshots/5",
	"key": "68001",
	"name": "Technical Debt",
	"description": "Technical Debt estimates...",
	"type": "technical-debt-statistics",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
}
```

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
| name | Quality rule name | String | 1 |
| shortName | Abbreviation | String | 0..1 |
| number | Snapshot order number | Integer | 1 |
| type | Always `"background-facts"` | String | 1 |
| description | Text | String | 0..1 |
| snapshots.href | Reference to get history of this item | URI | 1 |
| annotation | User annotation describing this snapshot | Structure | 1 |
| annotation.version | Assessment point version number | String | 1 |
| annotation.date | Application date | Date | 1 |
| annotation.description | Snapshot description | String | 1 |
| annotation.name | Snapshot name | String | 1 |
| annotation.enhancementMeasure | Computation mode of enhancement data. It has one of the following values : `"EFP"` or `"AEP"` | String | 1 |

### JSON Example

**GET DEMO/background-facts/66061/snapshots/5**

```json
{
	"href": "DEMO/background-facts/66061/snapshots/5",
	"key": "66061",
	"name": "Business Value",
	"description": "description",
	"type": "background-facts",
	"annotation": {
		"version": "1.0",
		"date": {
			"time": 1338328800000
		},
		"description": null,
		"name": "GM_DRAS_201206180516",
		"enhancementMeasure": "EFP"
	},
	"number": 1,
}
```

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
| reference | Text: External reference | String | 0..1 |
| sample | Text: Code snippet of a bad practice | String | 0..1 |
| remediationSample | Text: Code snippet of a remediation | String | 0..1 |
| output | Diagnosis findings report | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| total | Text: definition of the tested components | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |
| technologies[] | An applicable technology | String | 1..* |
| qualityStandards | Quality Standard References | Array | 1 |
| qualityStandards[] | A Quality Standard Reference | Struct | 0..1 |
| qualityStandards[].standard | Quality Standard name | String | 1 |
| qualityStandards[].id | Quality Standard reference ID | String | 1 |
| qualityStandards[].reference | URL to get an external documentation | String | 1 |
| qualityStandards[].description | RESERVED | String | 1 |

### JSON Example

**GET DEMO/rule-patterns/7298**

```json
{
	"href": "DEMO/rule-patterns/7298",
	"key": "7298",
	"name": "A class that has pointer data members must provide a copy constructor",
	"description": "The report list all...",
	"technologies": [
		"C++"
	],
	"rationale": "If you don't define a copy constructor...",
	"reference": null,
	"remediation": "Define a copy constructor to properly manage pointer data members.",
	"output": null,
	"associatedValueName": null,
	"total": null,
	"sample": "class MyClass {\n   char * apointermember;\n};",
	"remediationSample": "class MyClass...",
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
| reference | Text: External reference | String | 0..1 |
| output | Diagnosis findings report | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |
| technologies[] | An applicable technology | String | 1..* |

### JSON Example

**GET DEMO/distribution-patterns/65601**

```json
{
	"href": "DEMO/distribution-patterns/65601",
	"key": "65601",
	"name": "4GL Complexity Distribution",
	"description": "Distribution of Forms regarding their complexity",
	"technologies": [],
	"rationale": null,
	"reference": null,
	"remediation": null,
	"output": null,
	"associatedValueName": null
}
```

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
| reference | Text: External reference | String | 0..1 |
| output | deprecated | String | 0..1 |
| associatedValueName | Diagnosis findings name | String | 0..1 |
| total | Text: definition of the tested components | String | 0..1 |
| technologies | Applicable technologies | Array | 1 |
| technologies[] | An applicable technology | String | 1..* |

### JSON Example

**GET DEMO/measure-patterns/66067**

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
	"total": "Artifacts larger than 10 lines of code ( default value of the CODELINE parameter ) "
}
```

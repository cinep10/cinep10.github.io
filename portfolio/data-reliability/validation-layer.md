# Validation Layer (Technical Deep Dive)

A core layer for ensuring data reliability

---

## 1. Overview

The Validation Layer is a core component of the data reliability architecture.

This layer validates collected data in terms of:

* completeness
* validity
* consistency

Based on these checks, it generates **data quality signals**.

Its primary role is to separate and formalize data quality issues before the ML stage,
thereby improving the interpretability and stability of the overall platform.

---

## 2. Objective

The Validation Layer has two primary objectives:

* detect data quality issues
* protect ML input data from garbage-in problems

In other words, this layer determines whether data is in a trustworthy state
before it is used for analysis or modeling.

---

## 3. Architecture

```text
raw / aggregated data
        ↓
validation_layer_runner_v2
        ↓
validation_result (row-level)
        ↓
validation_summary_day (aggregation)
        ↓
risk scoring / ML feature input
```

---

## 4. validation_layer_runner_v2

### Role

* execute validation rules at the dataset or metric level
* generate validation results for each record or aggregated value
* support both rule-based and statistical checks

---

### Input Data

* `data_aggregation_day`
* `metric_statistics_day`
* optional baseline reference tables

---

### Output Data

* `validation_result` (row-level)

---

## 5. Validation Types

The Validation Layer is composed of five major types of checks.

---

### 5.1 Completeness Check (Null / Missing)

```sql id="mniejt"
CASE 
  WHEN value IS NULL THEN 'NULL_VIOLATION'
  WHEN value = 0 AND metric_type = 'count' THEN 'ZERO_SUSPICIOUS'
END
```

Purpose:

* detect missing data collection
* detect ETL failures
* detect API failures

---

### 5.2 Validity Check (Expected Range)

```sql id="ewz42k"
CASE
  WHEN value < expected_min THEN 'BELOW_RANGE'
  WHEN value > expected_max THEN 'ABOVE_RANGE'
END
```

Criteria:

* predefined business rules
* or dynamic ranges based on baseline statistics

Examples:

* `conversion_rate`: 0 ~ 1
* `latency`: 0 ~ 5000 ms

---

### 5.3 Statistical Anomaly (Z-score)

```text id="m92uvx"
z_score = (value - mean) / stddev
```

```sql id="ud0ldo"
CASE
  WHEN ABS(z_score) > 3 THEN 'STAT_ANOMALY_HIGH'
END
```

Purpose:

* detect abnormal fluctuations within otherwise valid ranges
* detect spikes and drift-like changes

---

### 5.4 Rule Violation (Business Logic)

Examples:

* `page_view < session_count`
* `purchase > add_to_cart`

```sql id="xmr2t5"
CASE
  WHEN purchase_count > add_to_cart THEN 'FUNNEL_BREAK'
END
```

Purpose:

* detect violations of service logic
* detect data pipeline errors

---

### 5.5 Time-series Continuity Check

```sql id="6e5q56"
LAG(value) OVER (PARTITION BY metric ORDER BY dt)
```

Checks include:

* sudden drop to zero
* unexpected gaps in time-series continuity

---

## 6. validation_result (Row-Level)

### Example Schema

| Column             | Description                |
| ------------------ | -------------------------- |
| dt                 | date                       |
| profile_id         | domain / profile           |
| metric_name        | metric name                |
| value              | observed value             |
| validation_type    | type of validation         |
| validation_status  | PASS / FAIL                |
| violation_type     | NULL / RANGE / RULE / STAT |
| severity           | LOW / MEDIUM / HIGH        |
| z_score            | statistical score          |
| expected_min / max | expected bounds            |
| created_at         | creation timestamp         |

---

### Design Characteristics

* all validation results are stored as normalized events
* multiple validation rules can be applied to the same row
* results can later be aggregated and scored in downstream layers

---

## 7. validation_summary_day (Aggregation Layer)

### Role

* aggregate row-level validation results by day
* provide summarized input to the Risk Layer

---

### Aggregation Logic

```sql id="qnhr2p"
SELECT
  dt,
  profile_id,
  COUNT(*) AS total_checks,
  SUM(CASE WHEN validation_status = 'FAIL' THEN 1 ELSE 0 END) AS fail_count,
  SUM(CASE WHEN violation_type = 'NULL' THEN 1 ELSE 0 END) AS null_issues,
  SUM(CASE WHEN violation_type = 'RANGE' THEN 1 ELSE 0 END) AS range_issues,
  SUM(CASE WHEN violation_type = 'RULE' THEN 1 ELSE 0 END) AS rule_issues,
  SUM(CASE WHEN violation_type = 'STAT' THEN 1 ELSE 0 END) AS stat_issues,
  AVG(z_score) AS avg_z_score,
  MAX(z_score) AS max_z_score
FROM validation_result
GROUP BY dt, profile_id
```

---

### Derived Metrics

* `fail_ratio = fail_count / total_checks`
* `critical_issue_flag`
* `dominant_issue_type`

---

## 8. Data Quality Signal

The key output of the Validation Layer is not simply a fail count,
but a **Data Quality Signal**.

### Signal Definitions

| Signal          | Condition                            |
| --------------- | ------------------------------------ |
| QUALITY_WARNING | fail_ratio > 5%                      |
| QUALITY_ALERT   | fail_ratio > 15%                     |
| CRITICAL_BREAK  | critical rule violation exists       |
| DATA_MISSING    | null-related issues increase sharply |

---

### Example

```sql id="6xyotc"
CASE
  WHEN fail_ratio > 0.15 THEN 'ALERT'
  WHEN fail_ratio > 0.05 THEN 'WARNING'
  ELSE 'NORMAL'
END
```

---

## 9. Connection to the Risk Layer

Validation results are consumed as follows:

```text id="th834h"
validation_summary_day
        ↓
data_risk_score_day
```

Usage includes:

* feature input (such as `fail_ratio`)
* direct penalty factors
* supporting signals for scenario interpretation

---

## 10. Key Design Principles

### 10.1 Validation is not Anomaly Detection

* Validation = incorrect or invalid data
* Drift / ML = unusual patterns in otherwise valid data

These must be clearly separated.

---

### 10.2 Preventing Baseline Contamination

Validation should always remain:

* based on raw or canonical input
* independent of scenario assumptions

---

### 10.3 Explainability

Every validation result must include:

* the reason (`violation_type`)
* the rule or threshold used (`expected range` or `business rule`)

This enables drill-down analysis in Grafana.

---

## 11. Grafana Integration

Recommended dashboard panels:

* daily fail ratio trend
* violation type distribution
* top failing metrics
* z-score distribution
* critical violation timeline

---

## 12. Summary

The Validation Layer ensures that:

* data exists when it should (completeness)
* data makes logical sense (validity / rule consistency)
* data behaves within expected statistical boundaries

Based on this, it transforms data quality into
an independent and quantifiable risk signal.

---

## One-line Definition

Validation Layer =
the layer that validates data quality and converts it into quantitative signals

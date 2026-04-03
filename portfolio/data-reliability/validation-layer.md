# Validation Layer (Technical Deep Dive)

A core layer for ensuring data reliability

⸻

1. Overview

The Validation Layer is a core component of the data reliability architecture.

This layer validates collected data in terms of:
	•	completeness
	•	validity
	•	consistency

Based on these checks, it generates data quality signals.

Its primary role is to separate and formalize data quality issues before the ML stage,
thereby improving the interpretability and stability of the overall platform.

⸻

2. Objective

The Validation Layer has two primary objectives:
	•	detect data quality issues
	•	protect ML input data from garbage-in problems

In other words, this layer determines whether data is in a trustworthy state
before it is used for analysis or modeling.

⸻

3. Architecture

raw / aggregated data
        ↓
validation_layer_runner_v2
        ↓
validation_result (row-level)
        ↓
validation_summary_day (aggregation)
        ↓
risk scoring / ML feature input


⸻

4. validation_layer_runner_v2

Role
	•	execute validation rules at the dataset or metric level
	•	generate validation results for each record or aggregated value
	•	support both rule-based and statistical checks

Input Data
	•	data_aggregation_day
	•	metric_statistics_day
	•	optional baseline reference tables

Output Data
	•	validation_result (row-level)

⸻

5. Validation Types

5.1 Completeness Check (Null / Missing)

CASE 
  WHEN value IS NULL THEN 'NULL_VIOLATION'
  WHEN value = 0 AND metric_type = 'count' THEN 'ZERO_SUSPICIOUS'
END

Purpose:
	•	detect missing data collection
	•	detect ETL failures
	•	detect API failures

⸻

5.2 Validity Check (Expected Range)

CASE
  WHEN value < expected_min THEN 'BELOW_RANGE'
  WHEN value > expected_max THEN 'ABOVE_RANGE'
END

Examples:
	•	conversion_rate: 0 ~ 1
	•	latency: 0 ~ 5000 ms

⸻

5.3 Statistical Anomaly (Z-score)

z_score = (value - mean) / stddev

CASE
  WHEN ABS(z_score) > 3 THEN 'STAT_ANOMALY_HIGH'
END

Purpose:
	•	detect abnormal fluctuations within valid ranges
	•	detect spikes and drift-like changes

⸻

5.4 Rule Violation (Business Logic)

CASE
  WHEN purchase_count > add_to_cart THEN 'FUNNEL_BREAK'
END

Examples:
	•	page_view < session_count
	•	purchase > add_to_cart

⸻

5.5 Time-series Continuity Check

LAG(value) OVER (PARTITION BY metric ORDER BY dt)

Checks:
	•	sudden drop to zero
	•	unexpected gaps

⸻

6. validation_result (Row-Level)

Example schema:
	•	dt
	•	profile_id
	•	metric_name
	•	value
	•	validation_type
	•	validation_status (PASS / FAIL)
	•	violation_type (NULL / RANGE / RULE / STAT)
	•	severity (LOW / MEDIUM / HIGH)
	•	z_score
	•	expected_min / max
	•	created_at

Design 특징
	•	validation 결과를 이벤트 형태로 저장
	•	하나의 row에 multiple validation 가능
	•	downstream aggregation / scoring 가능

⸻

7. validation_summary_day

Role
	•	row-level 결과를 일 단위로 집계
	•	Risk Layer 입력으로 사용

Aggregation

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

Derived Metrics
	•	fail_ratio
	•	critical_issue_flag
	•	dominant_issue_type

⸻

8. Data Quality Signal

Validation Layer의 핵심 출력은 Data Quality Signal

Signal Definition
	•	QUALITY_WARNING → fail_ratio > 5%
	•	QUALITY_ALERT → fail_ratio > 15%
	•	CRITICAL_BREAK → rule violation 존재
	•	DATA_MISSING → null 증가

CASE
  WHEN fail_ratio > 0.15 THEN 'ALERT'
  WHEN fail_ratio > 0.05 THEN 'WARNING'
  ELSE 'NORMAL'
END


⸻

9. Connection to Risk Layer

validation_summary_day
        ↓
data_risk_score_day

Usage:
	•	feature input (fail_ratio 등)
	•	risk penalty factor
	•	scenario 판단 보조

⸻

10. Key Design Principles

Validation ≠ Anomaly Detection
	•	Validation → incorrect data
	•	Drift / ML → unusual patterns

⸻

Prevent Baseline Contamination

Validation must be:
	•	raw-based
	•	scenario-independent

⸻

Explainability

Each validation must include:
	•	violation_type
	•	threshold / rule

→ Grafana drill-down 가능

⸻

11. Grafana Integration

Recommended panels:
	•	daily fail ratio trend
	•	violation type distribution
	•	top failing metrics
	•	z-score distribution
	•	critical violation timeline

⸻

12. Summary

Validation Layer ensures:
	•	data exists (completeness)
	•	data is logically valid (validity / rules)
	•	data follows expected patterns (statistical behavior)

And converts data quality into a quantifiable risk signal

⸻

One-line Definition

Validation Layer =
the layer that validates data quality and converts it into quantitative signals

⸻
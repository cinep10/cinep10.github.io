
---

# Drift Layer

A layer for detecting how data behavior deviates from its normal state

---

## 1. Overview

If the Validation Layer answers
**“Is the data correct?”**,
the Drift Layer answers
**“Has the data changed from its normal behavior?”**

The purpose of this layer is to quantitatively measure how much data at a given point in time deviates from its baseline, and determine whether the change is a simple fluctuation or a structural anomaly.

In other words, the Drift Layer answers the following questions:

* How much have today’s metrics changed compared to normal?
* Is this change temporary or structural?
* Which metric dimensions show concentrated changes?
* Has not only the value changed, but also the pattern and relationships?

This layer goes beyond simple distribution comparison and acts as a dynamic anomaly detection layer that considers both temporal patterns and inter-metric relationships.

---

## 2. Architecture

```
metric_value_day
    ↓
metric_drift_analysis_db_v8.R
    ↓
metric_drift_result_r
    ↓
risk scoring / root cause / dashboard
```

Structural anomalies are calculated separately:

```
metric_value_day
    ↓
time_pattern_anomaly_runner
    ↓
metric_time_anomaly_day

metric_value_day
    ↓
correlation_anomaly_runner
    ↓
metric_correlation_anomaly_day
```

The Drift Layer consists of two perspectives:

* Drift Detection: changes in values
* Structural Anomaly Detection: changes in patterns and relationships

---

## 3. Input Data

The primary input is `metric_value_day`.

Examples:

* daily_active_users
* page_view_count
* auth_attempt_count
* auth_success_rate
* loan_apply_submit_count
* card_apply_submit_rate
* mapping_coverage_auth
* estimated_missing_rate

Drift compares two windows:

**Current Window**

* metric values for the target date

**Baseline Window**

* historical values from previous N days or a reference period

The Drift Layer is essentially a
**current vs baseline comparison engine**

---

## 4. metric_drift_analysis_db_v8.R

This is an R-based drift computation engine.

Main roles:

* extract metric time series from the database
* compute baseline statistics
* compare current vs baseline
* store drift results

Baseline statistics include:

* mean
* standard deviation
* quantiles

Comparison metrics include:

* z-score
* ratio change
* PSI
* funnel change

Output:

* `metric_drift_result_r`

---

## 5. Drift Computation Methods

### 5.1 Z-score Deviation

```
z_score = (current_value - baseline_mean) / baseline_std
```

* |z| < 2 → normal
* |z| ≥ 2 → warn
* |z| ≥ 3 → alert

---

### 5.2 Distribution Drift (PSI)

```
PSI = Σ (actual_i - expected_i) * ln(actual_i / expected_i)
```

* PSI < 0.10 → stable
* 0.10 ~ 0.25 → moderate
* ≥ 0.25 → significant

---

### 5.3 Ratio Drift

```
ratio_diff = current / baseline_mean
ratio_diff_pct = (current - baseline_mean) / baseline_mean
```

Used for:

* conversion rates
* success ratios
* coverage metrics

---

### 5.4 Funnel Change

Example:

```
start → process → submit
```

Checks:

* step-wise volume changes
* conversion rate deviation
* funnel distortion

This captures behavioral flow changes, not just volume changes.

---

## 6. metric_drift_result_r

Key fields:

* profile_id
* dt
* metric_name
* baseline_mean
* baseline_std
* current_value
* z_score
* drift_score
* drift_status (normal / warn / alert)
* severity

Most commonly used:

* drift_score
* drift_status

---

## 7. Drift Decision Logic

```
current_value
    ↓
baseline calculation
    ↓
z_score
    ↓
ratio / psi / funnel
    ↓
drift_score
    ↓
status classification
```

Example:

```python
if abs(z_score) >= 3 or psi >= 0.25:
    drift_status = "alert"
elif abs(z_score) >= 2 or psi >= 0.10:
    drift_status = "warn"
else:
    drift_status = "normal"
```

Thresholds should vary depending on metric type:

* conversion metrics → more sensitive
* traffic metrics → less sensitive
* campaign-driven metrics → exceptions or annotations

---

## 8. Structural Anomaly Detection

Drift measures value change.
Structural Anomaly measures pattern and relationship change.

It detects:

* time-series pattern shifts
* metric relationship breakdown
* funnel dependency issues

---

### 8.1 Time Pattern Anomaly

```
rolling_avg_7d = mean(value[t-7:t-1])
rolling_std_7d = std(value[t-7:t-1])
zscore_7d = (value[t] - rolling_avg_7d) / rolling_std_7d
```

Detects:

* sudden spikes
* sudden drops
* trend breaks

---

### 8.2 Correlation Anomaly

```
baseline_ratio = mean(left_metric / right_metric)
observed_ratio = left_metric_today / right_metric_today
ratio_diff_pct = (observed - baseline) / baseline
```

Detects:

* funnel break
* dependency break
* relationship distortion

---

## 9. Drift vs Structural

* Drift → value change
* Structural → relationship / pattern change

Example:

* start is stable
* submit slightly drops

Drift may miss it,
but conversion ratio reveals a real issue.

---

## 10. Connection to Risk Layer

Used as inputs:

* drift_alert_count
* drift_warn_count
* avg_drift_score

Structural signals:

```
time anomaly + correlation anomaly → structural risk
```

---

## 11. Connection to Root Cause

Example:

* submit metric drift
* start vs submit correlation break

→ interpreted as funnel break

This improves:

* root cause analysis
* action prioritization
* explainability

---

## 12. Grafana Integration

Drift:

* drift alert trend
* top drifting metrics
* drift timeline

Time anomaly:

* anomaly counts
* rolling vs observed trends

Correlation anomaly:

* broken metric pairs
* funnel ratio deviation

---

## 13. Summary

The Drift Layer measures how much current metrics deviate from baseline behavior.

Core functions:

* z-score
* PSI
* ratio change
* funnel analysis

Structural Anomaly detects:

* time pattern changes
* metric relationship breakdown
* structural behavior changes

---

## One-line Definition

Drift Layer =
a layer that measures how data values, patterns, and relationships deviate from their normal state

---

같은 톤으로 이어드릴게요.

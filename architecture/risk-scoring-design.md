# Risk Score Design(v4)

A Unified Risk Scoring Framework for Data Reliability

---

## 1. Overview

Risk Score in this system is not a simple anomaly metric.

It is a unified layer that integrates outputs from Validation, Drift, Structural Anomaly, and Mapping into a single operational risk indicator.

Each upstream layer answers a different question:

* Validation → Is the data correct?
* Drift → How much has it changed from baseline?
* Structural → Has the relationship structure broken?
* Mapping → Is the data interpretable?

Risk Score consolidates these signals into a single answer:

**“How risky is the system state right now?”**

---

## 2. Design Evolution (v4)

### 2.1 From Anomaly Detection to Risk Integration

Previous versions:

* Validation failures
* Drift signals

v4 introduces:

* Validation
* Drift
* Time anomaly
* Correlation anomaly
* Mapping coverage

This shifts the design from:

**isolated anomaly detection → integrated risk modeling**

---

### 2.2 From Metric-level to Day-level Decision

Previous:

* Metric-level analysis

v4:

* Metric aggregation → day-level risk

The system is designed for operational questions:

**“Is today safe or not?”**

---

### 2.3 Explainability-Centric Design

Risk Score is not just a number.

It is directly connected to:

* Root Cause Layer
* Action Engine
* ML Feature Layer

This makes Risk Score the central axis of system interpretability.

---

## 3. Architecture

```text
validation_result
metric_drift_result
metric_time_anomaly_day
metric_correlation_anomaly_day
mapping_coverage_day
        ↓
risk_score_day_v4_runner
        ↓
data_risk_score_day_v3
```

---

## 4. Risk Components

Risk Score v4 is composed of five core signals.

---

### 4.1 Validation Score

Measures data quality issues.

Inputs:

* null / missing values
* range violations
* rule violations

Pseudo logic:

```text
validation_score = weighted_sum(fail_count / total_count)
```

Characteristics:

* Fundamental signal
* May generate false positives

---

### 4.2 Drift Score

Measures deviation from baseline behavior.

Signals:

* z-score
* PSI
* distribution shift
* funnel change

```text
drift_score = normalize(z_score, psi, funnel_change)
```

Characteristics:

* Captures change, not necessarily errors
* Requires interpretation (normal vs abnormal change)

---

### 4.3 Time Pattern Anomaly

Detects temporal deviations.

Examples:

* sudden spikes or drops
* pattern breakdown
* unexpected time behavior

```text
time_anomaly_score = deviation_from_time_pattern
```

Characteristics:

* Critical for detecting bursts, fraud-like behavior, campaign effects
* Complementary to drift

---

### 4.4 Correlation Anomaly

Detects breakdown in metric relationships.

Examples:

* view ↑ but purchase ↓
* apply ↑ but submit ↓

```text
corr_anomaly_score = correlation_break_score
```

Characteristics:

* Core signal for funnel integrity
* Strong indicator of structural issues

---

### 4.5 Mapping Coverage Score

Measures interpretability of data.

Examples:

* increase in unmapped events
* classification failures

```text
mapping_score = 1 - mapping_coverage_ratio
```

Characteristics:

* Reflects semantic data quality
* Directly impacts ML and AI downstream

---

## 5. Final Risk Score Calculation

Risk Score is computed as a weighted combination:

```text
final_risk_score =
    w1 * validation_score +
    w2 * drift_score +
    w3 * time_anomaly_score +
    w4 * corr_anomaly_score +
    w5 * mapping_score
```

Design intention:

* Validation → data correctness
* Drift → behavioral change
* Structural → relationship integrity
* Mapping → interpretability

This results in:

**Data + Structure + Meaning integration**

---

## 6. Risk Grade

Risk Score is translated into operational states:

```text
if score >= 0.7:
    grade = "alert"
elif score >= 0.3:
    grade = "warning"
else:
    grade = "normal"
```

---

## 7. Scenario Validation

### 7.1 Baseline

* score ≈ 0.01 ~ 0.02
* grade = normal

Indicates stable system behavior.

---

### 7.2 Funnel Break

* score ≥ 0.7
* grade = alert

Driven by:

* correlation anomaly
* drift increase

---

### 7.3 Interpretation

* Validation alone cannot detect this
* Drift + Structural signals are essential

This validates the design approach.

---

## 8. Integration with Root Cause and Action

Risk Score feeds downstream systems:

```text
risk_score
    ↓
root_cause_result
    ↓
data_reliability_action_day
```

This enables:

* identification of problematic metrics
* explanation of causes
* actionable next steps

---

## 9. Integration with ML

Risk Score is a core feature for ML.

```text
ml_feature_vector_day:
    - final_risk_score
    - risk_grade
    - drift_score
    - anomaly_score
```

ML acts as:

**a risk state classifier**

---

## 10. Integration with AI

AI operates on top of Risk Score.

Inputs:

* risk_score
* ML prediction
* root cause

Outputs:

* incident summary
* action recommendation

Thus:

**Risk Score is the anchor of AI reasoning**

---

## 11. Limitations

### 11.1 Static Weights

* Currently fixed
* Requires adaptive weighting

---

### 11.2 Domain Dependency

* Designed for web logs
* Needs redesign for financial domains

---

### 11.3 Structural Sensitivity

* Some scenarios underrepresented
* Needs tuning

---

## 12. Future Directions

### 12.1 Domain Expansion (Finance)

* amount-based risk
* state consistency (balance, approval)
* stronger funnel validation

---

### 12.2 Weight Optimization

```text
weight = learned_weight (ML-driven)
```

---

### 12.3 Explainability Enhancement

* metric contribution scoring
* causal graph modeling

---

## 13. Summary

Risk Score v4 is not an anomaly metric.

It is a unified operational risk indicator that integrates:

* data quality
* behavioral change
* structural integrity
* semantic interpretability

It serves as:

* input to ML
* reasoning base for AI
* decision signal for operators

---

## One-line Definition

Risk Score =
A unified metric that transforms data quality, change, structure, and meaning into operational risk

---

## Final Insight

The core of this system is not ML.

It is the Risk Score.

# Explainable Data Behavior: Implementation

## 1. Pipeline Overview

The ML pipeline is structured as follows:

```text
Feature Vector → Train → Predict → Importance → Drift
```

---

## 2. Feature Engineering

### 2.1 Feature Generation

```text
metric_value_day → aggregation → ml_feature_vector_day
```

All features are derived from metric data.

---

### 2.2 Feature Categories

- User behavior metrics
- Authentication metrics
- Funnel metrics
- Data quality metrics

Key characteristics:

- Daily aggregation
- Null values normalized to 0
- Schema consistency enforced

---

## 3. Model Training

### 3.1 Model Selection

- Logistic Regression

Reason:

- High interpretability
- Suitable for feature importance analysis

---

### 3.2 Limitation

Initial dataset:

```text
All labels = normal
```

→ Single-class problem

---

### 3.3 Fallback Strategy

To address this:

```text
If ML is not trainable → use rule-based risk scoring
```

---

## 4. Prediction Logic

```text
1. If supervised learning is possible → use ML
2. Otherwise → fallback to rule-based logic
```

---

### Fallback Rules

```text
risk_score ≥ 0.7 → alert
risk_score ≥ 0.4 → warning
else → normal
```

---

## 5. Feature Importance

- Extracted from model coefficients
- Stored for explainability

---

## 6. Feature Drift

- Baseline vs observed comparison
- Drift score calculated per feature

---

## 7. Execution Pipeline

```text
run_scenario_test_pipeline.sh
```

Steps:

1. Scenario injection
2. Drift calculation
3. Risk scoring
4. Root cause analysis
5. Feature generation
6. Model training
7. Prediction
8. Importance extraction
9. Feature drift analysis

---

## 8. Key Implementation Challenges

- Schema mismatch handling
- Data type normalization
- Stable numeric conversion
- Fallback design

---

## 9. Conclusion

The most important lesson:

> Data pipeline reliability is more critical than ML itself

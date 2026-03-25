# ML Input Data Reliability Monitoring

**Monitoring ML Input Reliability**

---

## 1. Problem
ML assumes:
```text
Input distribution is stable
```
Reality:
- user behavior changes
- traffic shifts
- feature relationships break

---

## 2. Architecture
```text
Metric Layer
    ↓
ML Feature Drift Analysis
    ↓
ml_feature_drift_result
    ↓
Risk Score Integration
    ↓
data_risk_score_day_v3
```

---

## 3. Drift Detection
Based on:
```text
metric_value_hh
```

Methods:
- PSI-like drift
- Z-score anomaly

---

## 4. Risk Composition

```text
Risk Score =
    Validation Contribution
  + Drift Contribution
  + ML Feature Contribution
```

---

## 5. Key Insight

```text
Most risk is NOT from ML,
but from data issues.
```

- distribution shifts
- structural inconsistencies
- traffic variation

---

## 6. Why This Matters

```text
Do not trust ML blindly.
Trust the data feeding it.
```

---

## 7. Outcome
- ML input monitoring
- drift-aware ML system
- explainable predictions

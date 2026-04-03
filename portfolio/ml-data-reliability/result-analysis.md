# Explainable Data Behavior: Result Analysis

## 1. Test Scenarios

- funnel_break
- auth_failure
- mixed_incident

---

## 2. Summary of Results

### Risk Score

- Overall low levels observed

### ML Prediction

- Mostly classified as normal
- Very low alert probability

---

## 3. Key Findings

### 3.1 ML is not effective yet

Reasons:

- Lack of labeled data
- Low risk signal strength

---

### 3.2 Root Cause Analysis is accurate

Example:

- Decrease in loan_apply_submit_count
- Correctly identified as funnel_break

---

### 3.3 Drift Detection works properly

- Baseline comparison is stable
- Distribution changes are detected

---

## 4. System Evaluation

```text
Data Reliability: Complete
Explainability: Achieved
ML Performance: Limited
```

---

## 5. Core Issue

The problem is not ML.

> The real issue is lack of labels

---

## 6. Improvements

### 6.1 Label Generation

```text
Generate labels based on risk_score
```

---

### 6.2 Risk Model Enhancement

- Scenario-aware weighting
- Adaptive scoring

---

### 6.3 ML Retraining

- Multi-class classification
- Handle class imbalance

---

## 7. Conclusion

The key achievement is not ML.

> The system successfully explains data behavior

---

## 8. Future Work

- risk_score_v4
- label generation
- ML retraining
- scenario-based evaluation

# Risk Scoring Design

## Converting Data Issues into Actionable Signals

---

## 1. Problem

Validation and Drift detect issues.

However, they do not answer:

> How critical is this problem?

---

### Real-world Situation

- many validation warnings → important?  
- drift alerts → meaningful impact?  

---

### Result

> Issues exist, but prioritization is unclear

---

## 2. Design Goal

The Risk Layer is designed to:

> Aggregate data issues into a single, interpretable score

---

### Objectives

- unify multiple signals  
- apply weighted importance  
- ensure explainability  
- support trend analysis  

---

## 3. Architecture


Validation + Drift + ML Feature
↓
Signal Generation
↓
Weighted Aggregation
↓
Risk Score


---

## 4. Core Design 1: Signal-based Structure

---

Each issue is converted into a signal.

---

### Examples

- validation_fail_count  
- validation_warn_count  
- drift_alert_count  
- drift_warn_count  
- ml_feature_alert_count  

---

### Purpose

Standardize different problems into a common format.

---

## 5. Core Design 2: Contribution Decomposition

---

Risk is not a single number.

---


Risk =
Validation Contribution

Drift Contribution
ML Feature Contribution

---

### Benefit

Explains why risk increased.

---

## 6. Core Design 3: Weighted Aggregation

---

Not all signals are equal.

---

### Examples

- validation_fail > validation_warn  
- drift_alert > drift_warn  

---

### Concept


Risk = Σ (signal × weight)


---

## 7. Core Design 4: Explainability

---

Risk must be decomposable.

---

### Example


Risk = 87

Drift: 60%
Validation: 35%
ML Feature: 5%

---

### Meaning

Not just a score, but an explanation.

---

## 8. Core Design 5: Time-based Risk

---

Risk must be interpreted over time.

---

### Usage

- spike → event-driven anomaly  
- steady increase → structural issue  

---

## 9. Output Structure

---

### Table


data_risk_score_day_v3


---

### Key Fields

- dt  
- risk_score  
- risk_status  
- validation_fail_count  
- drift_alert_count  
- ml_feature_alert_count  
- total_signal_count  

---

## 10. Key Design Summary

---

### Risk is a summary of problems

Multiple issues are compressed into one value.

---

### Explainability is essential

Scores without context are meaningless.

---

### Trend matters more than snapshot

Patterns over time define real risk.

---

### Risk precedes ML

Without risk, ML results are not interpretable.

---

## One-line Summary

Risk Scoring = Converting data issues into decision-ready values

---

## Portfolio Statement

Validation and Drift signals were unified into an explainable risk scoring system,  
enabling prioritization and decision-making based on data reliability.

# Risk Layer

## From Data Signals to Business Impact

---

## 1. Problem

Modern data systems can detect multiple issues:

- validation failures  
- distribution changes (drift)  
- feature instability  

However, detection alone is not sufficient.

---

### Limitation

When multiple signals occur:

- it is unclear which issue is critical  
- prioritization becomes difficult  
- operational decisions depend on manual judgment  

---

### Core Issue

> Data systems detect problems,  
> but cannot determine their impact

---

## 2. Why Risk Matters

Validation answers:

- Is the data correct?

Drift answers:

- Has the data changed?

---

But neither answers:

> Does this change matter?

---

### Example

- validation warnings present  
- drift detected  

---

Without integration:

- no prioritization  
- no clear action  

---

### Result

> Systems produce signals, but not decisions

---

## 3. Design Goal

The Risk Layer is designed to:

> Convert data signals into a unified representation of impact

---

### Objectives

- integrate multiple signals  
- quantify impact  
- enable prioritization  
- support decision-making  

---

## 4. Architecture

```text
Validation + Drift + Feature Signals → Risk Score
```

---

### Input

- validation_summary_day  
- metric_drift_result  
- ml_feature_drift_result  

---

### Output

```text
data_risk_score_day
```

---

Risk operates as an aggregation layer.

---

## 5. Risk Model

---

### Core Structure

```text
Risk Score =
  Validation Contribution
  + Drift Contribution
  + Feature Contribution
```

---

Each component represents a different dimension of reliability.

---

### 5.1 Validation Contribution

Represents structural correctness.

- validation_fail_count  
- validation_warn_count  

---

### 5.2 Drift Contribution

Represents behavioral change.

- drift_alert_count  
- drift_severity  

---

### 5.3 Feature Contribution

Represents ML input stability.

- feature_alert_count  
- feature_drift_score  


---

## 6. Design Principles

---

### 6.1 Signal-based Aggregation

All inputs are treated as signals.

---

Example:

- validation_fail → high impact  
- drift_warn → medium impact  

---

Risk is computed from aggregated signals.

---

### 6.2 Weighted Model

Not all signals are equal.

---

Typical priority:

- Validation > Drift > Feature  

---

Because:

- structural issues invalidate all downstream data  

---

### 6.3 Severity-aware Scoring

Different levels contribute differently:

- WARN  
- FAIL  

---

Example:

- FAIL contributes more than WARN  
- multiple WARNs can accumulate  

---

### 6.4 Explainability

Risk must be decomposable.

---

### Example

```text
Risk = 85

Validation: 50 (row drop)
Drift: 25 (traffic spike)
Feature: 10 (conversion instability)
```

---

Each component must be traceable.

---

### 6.5 Time-based Analysis

Risk must be interpreted over time.

---

### Example Patterns

- stable → increasing → critical  
- spike → recovery  

---

Trend analysis enables early detection.

---

## 7. Relationship with Other Layers

---

### Full Flow

```text
Validation → Drift → Risk → Root Cause → Decision
```

---

### Interpretation

- Validation → correctness  
- Drift → change  
- Risk → impact  
- Root Cause → explanation  
- Decision → action  

---

### Key Role

> Risk is the bridge between detection and decision

---

## 8. Role in Real Systems

---

### Observed Pattern

- validation warnings are persistent  
- drift alerts occur during events  
- risk increases when both overlap  

---

### Interpretation

> Risk emerges from the interaction of structure and change

---

### Typical High-Risk Scenarios

- validation failure + drift spike  
- funnel distortion + traffic surge  
- feature instability under load  

---

## 9. Key Insights

---

### Detection is not prioritization

Multiple issues require a unified view.

---

### Risk is not a simple sum

It reflects:

- structure  
- change  
- input stability  

---

### Risk enables decision-making

Without risk, signals remain analytical.

---

### Risk is a business abstraction

It translates technical signals into operational impact.

---

## 10. Conclusion

Risk Layer is not just a scoring system.

It is:

> A mechanism that converts data conditions into actionable impact

---

## One-line Summary

Risk = A quantified representation of data reliability and its impact

# Drift Layer

## Detecting Changes in Data Meaning

---

## 1. Problem

Validation determines whether data is correct.

However, correctness alone is not sufficient.

Even valid data can become unreliable when its behavior changes.

---

### What Actually Changes

- traffic increases or decreases  
- user behavior shifts  
- event patterns evolve  
- seasonal effects appear  

---

### Core Issue

> Data can be structurally correct,  
> but its meaning can change over time

---

## 2. Why Drift Matters

A system that only validates correctness cannot detect change.

---

### Example

- page_view remains stable  
- conversion rate drops significantly  

---

The data is valid, but the underlying behavior has changed.

---

### Result

> Decisions are made on outdated assumptions

---

## 3. Design Goal

The Drift Layer is designed to:

> Detect and quantify changes in data distribution and structure

---

### Objectives

- detect changes (detection)  
- measure change intensity (severity)  
- identify structural changes  
- integrate with Risk  

---

## 4. Architecture

```text
Metric → Drift → Drift Result
```

---

### Output Tables

- metric_drift_result  
- ml_feature_drift_result  

---

Drift operates on metrics, not raw data.

---

## 5. Types of Drift

---

### 5.1 Distribution Drift

Changes in value distribution.

---

#### Examples

- increase in user_count  
- decrease in session_count  

---

#### Interpretation

The scale of activity has changed.


---

### 5.2 Structural Drift

Changes in relationships between metrics.

---

#### Examples

- page_view stable but submit decreases  
- login → success conversion drops  

---

#### Interpretation

> Funnel structure is distorted

---

This is the most critical form of drift.


---

### 5.3 Time Pattern Drift

Changes in temporal behavior.

---

#### Examples

- abnormal traffic spikes at specific hours  
- deviation from typical daily patterns  

---

#### Interpretation

System behavior differs from historical patterns.

---

## 6. Detection Methods

---

### 6.1 Baseline Comparison

Comparison against historical baseline:

- weekday  
- hour  

---

### 6.2 Z-score

Measures deviation from expected value.

---

```text
z = (current - mean) / std
```

---

### 6.3 PSI-like Metrics

Measures distribution difference.

---

Used for:

- feature distribution comparison  
- population shift detection  

---

## 7. Design Principles

---

### 7.1 Drift Requires a Baseline

Drift cannot exist without a reference.

---

### 7.2 Patterns Matter More Than Values

Single values are less meaningful than changes over time.

---

### 7.3 Structure Over Individual Metrics

The system focuses on relationships, not isolated values.

---

### Key Insight

> Drift represents a change in data meaning

---

## 8. Relationship with Validation and Risk

---

### Core Flow

```text
Validation → Drift → Risk
```

---

### Interpretation

- Validation → Is the data correct?  
- Drift → Has the data changed?  
- Risk → Does the change matter?  

---

### Important Distinction

- Validation detects structural issues  
- Drift detects environmental or behavioral change  

---

Both are required for reliable interpretation.

---

## 9. Role in Real Systems

---

### Observed Pattern

- validation warnings occur continuously  
- drift alerts spike during specific events  

---

### Interpretation

> External changes amplify existing structural weaknesses

---

### Typical Drift Signals

- traffic spikes  
- funnel conversion drop  
- correlation breakdown  

---

## 10. Key Insights

---

### Drift is inevitable

All real-world data changes over time.

---

### Drift is context-dependent

The same change can be normal or abnormal depending on baseline.

---

### Drift without validation is dangerous

Incorrect data makes drift analysis meaningless.

---

### Drift is the first signal of behavioral change

It often precedes visible failures.

---

## 11. Conclusion

Drift Layer is not just about detecting anomalies.

It is about:

> Understanding how data behavior evolves over time

---

## One-line Summary

Drift = A change in the meaning of data over time

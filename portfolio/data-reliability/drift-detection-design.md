# Drift Detection Design

## Detecting Changes in Data Behavior and Meaning

---

## 1. Problem

Validation determines whether data is correct.

However, it cannot answer a critical question:

> Has the data changed compared to the past?

---

### Real-world Changes

- traffic spikes or drops  
- user behavior shifts  
- event distribution changes  
- feature usage increases  

---

### Risk

> Even correct data can become misleading when its behavior changes

---

### Example

- user_count increases → normal (traffic growth)  
- conversion_rate decreases → problem  

---

## 2. Design Goal

The Drift Detection Layer is designed to:

> Quantitatively detect distribution and structural changes in data

---

### Objectives

- detect changes (detection)  
- measure intensity (severity)  
- identify structural shifts  
- integrate with Risk  

---

## 3. Architecture


Metric → Baseline → Drift Calculation → Drift Result


---

### Input

- metric_value_hh  
- metric_value_day  

---

### Output

- metric_drift_result  
- ml_feature_drift_result  

---

## 4. Core Design 1: Baseline Definition

Drift cannot exist without a reference.

---

### Selected Baseline


weekday + hour baseline


---

### Why Not Global Average?

❌ Problem:

- ignores time patterns  
- ignores weekday/weekend differences  

---

### Selected Approach


Compare Monday 10AM with previous Mondays 10AM


---

### Benefit

- captures time patterns  
- reflects seasonality  
- reduces false positives  

---

---

## 5. Core Design 2: Drift Calculation

---

### 5.1 Z-score Drift


z = (current - mean) / std


---

### Interpretation

- |z| < 2 → normal  
- 2 ≤ |z| < 3 → warn  
- |z| ≥ 3 → alert  

---

---

### 5.2 PSI-like Drift

Used to measure distribution differences.

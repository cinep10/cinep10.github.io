# Root Cause Layer

## From Detection to Explanation: Making Data Issues Understandable

---

## 1. Problem

Modern data systems can detect issues.

- Validation detects correctness problems  
- Drift detects behavioral changes  
- Risk quantifies impact  

---

However, one critical question remains:

> Why did this problem occur?

---

### Limitation

Without root cause analysis:

- operators must manually investigate  
- interpretation is inconsistent  
- response time increases  

---

### Core Issue

> Detection without explanation does not lead to action

---

## 2. Why Root Cause Matters

Detection answers *what happened*.

Root Cause answers *why it happened*.

---

### Example

- Drift detected  
- Risk increased  

---

But:

- Was it traffic?  
- Was it a mapping issue?  
- Was it a funnel problem?  

---

Without explanation:

> The system is not actionable

---

## 3. Design Goal

The Root Cause Layer is designed to:

> Automatically generate interpretable explanations for data issues

---

### Objectives

- convert signals into explanations  
- reduce manual analysis  
- enable consistent interpretation  
- connect to decision-making  

---

## 4. Architecture

```text
Risk Signals → Rule Engine → Root Cause
```

---

### Input

- validation_summary_day  
- metric_drift_result  
- ml_feature_drift_result  

---

### Output

```text
data_risk_root_cause_day
```

---

## 5. Root Cause Model

Root Cause is derived from combinations of signals.

---

### Key Idea

> A single signal is not meaningful  
> Multiple signals define a cause

---

## 6. Rule-based Interpretation

---

### Example Rule 1: Traffic Spike

```text
IF drift_alert_count ↑
AND user_count ↑
THEN traffic_spike
```

---

### Example Rule 2: Funnel Distortion

```text
IF submit ↓
AND click stable
THEN funnel_distortion
```

---

### Example Rule 3: Mapping Issue

```text
IF validation_fail ↑
THEN mapping_issue
```

---

These rules transform signals into semantic explanations.

---

## 7. Output Structure

---

### Key Fields

- cause_type  
- description  
- confidence  

---

### Example

```text
cause_type: funnel_distortion
description: conversion dropped while upstream activity remained stable
confidence: 0.82
```

---

## 8. Design Principles

---

### 8.1 Signal Combination

Root Cause is not derived from a single metric.

It requires:

- validation signals  
- drift signals  
- contextual metrics  

---

### 8.2 Explainability

Output must be human-readable.

---

Bad:

- "drift detected"

---

Good:

- "conversion dropped while traffic remained stable"

---

### 8.3 Deterministic Rules

Rules must be:

- reproducible  
- consistent  
- testable  

---

### 8.4 Integration with Risk

Root Cause is not independent.

```text
Risk → Root Cause → Decision
```

---

Root Cause explains *why* risk increased.

---

## 9. Role in the System

---

### Full Flow

```text
Data
→ Metric
→ Validation
→ Drift
→ Risk
→ Root Cause
```

---

### Interpretation

- Validation → structural correctness  
- Drift → behavioral change  
- Risk → impact  
- Root Cause → explanation  

---

## 10. Key Insights

---

### Detection is not enough

Systems must explain, not just detect.

---

### Root Cause enables action

Without explanation, response is delayed.

---

### Meaning comes from relationships

Causes are defined by interactions between signals.

---

### This is where systems become intelligent

Root Cause transforms monitoring into reasoning.

---

## 11. Conclusion

Root Cause Layer is not an analysis tool.

It is:

> A system that translates data issues into human-understandable explanations

---

## One-line Summary

Root Cause = Converting data signals into interpretable explanations

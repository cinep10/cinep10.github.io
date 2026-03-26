# Validation Layer

## Ensuring That Measured Data Can Be Trusted

---

## 1. Problem

Even if metrics are well-defined,  
they are not always reliable.

---

### What Happens in Real Systems

- missing logs → certain metrics become zero  
- mapping errors → submit > click  
- identity issues → abnormal user_count spikes  
- session errors → distorted session metrics  
- delayed or partial data ingestion  

---

### Key Insight

> Metrics are always subject to error

---

### Result

> Data may appear valid, but be structurally incorrect

This leads to:

- incorrect analysis  
- misleading conclusions  
- unreliable decisions  

---

## 2. Design Goal

The Validation Layer is designed to:

> Quantitatively determine whether metrics are trustworthy

---

### Objectives

- validate metric reliability  
- detect structural inconsistencies  
- continuously measure data quality  
- generate signals for Risk Layer  

---

## 3. Architecture

```text
metric_value_hh / metric_value_day
↓
validation rules
↓
validation_result
↓
aggregation
↓
validation_summary_day
```

---

## 4. Input

The Validation Layer operates exclusively on metrics:

```text
metric_value_hh
metric_value_day
```

---

### Design Principle

> Validation does not inspect raw data  
> It validates interpreted data (metrics)

---

### Reason

- raw validation belongs to ETL  
- validation focuses on semantic correctness  

---

## 5. Validation Design

Validation is structured into four categories.

---

### 5.1 Completeness Validation

---

#### Purpose

Does the data exist?

---

#### Examples

- page_view = 0  
- sudden drop in user_count  

---

#### Logic

```text
IF metric_value = 0
THEN validation_status = 'warn'
```

---

#### Interpretation

Indicates ingestion or parsing issues.

---

### 5.2 Structural Validation

---

#### Purpose

Are relationships between metrics valid?

---

#### Rules

- session_count ≥ user_count  
- click_count ≤ page_view  
- submit_count ≤ click_count  

---

#### Logic

```text
IF submit_count > click_count
THEN validation_status = 'fail'
```

---

#### Key Insight

> This validates behavior structure, not just values

---

#### Interpretation

Detects funnel distortion.

---

### 5.3 Mapping Validation

---

#### Purpose

Are metrics correctly derived from raw data?

---

#### Examples

- missing page_type  
- incorrect event mapping  
- critical metrics = 0  

---

#### Logic

```text
IF login_success_count = 0
THEN validation_status = 'warn'
```

---

#### Interpretation

Detects ETL / parsing / mapping issues.

---

### 5.4 Ratio / Range Validation

---

#### Purpose

Are values within valid ranges?

---

#### Examples

- conversion_rate > 1  
- auth_success_rate > 1  
- negative latency  

---

#### Logic

```text
IF conversion_rate > 1
THEN validation_status = 'fail'
```

---

## 6. Output Design

---

### 6.1 validation_result

Stores individual validation outcomes.

---

#### Key Fields

- dt  
- metric_name  
- validation_type  
- validation_status (normal / warn / fail)  
- metric_value  
- threshold  
- message  

---

### Design Insight

> Validation results are stored as structured data, not logs

---

### 6.2 validation_summary_day

Aggregated daily validation results.

---

#### Key Fields

- validation_fail_count  
- validation_warn_count  
- total_validation_count  

---

#### Meaning

> A quantitative measure of daily data quality

---

## 7. Design Principles

---

### 7.1 From Rules to Signals

---

Traditional approach:

```text
if error → log
```

---

This system:

```text
validation → signal → aggregation → risk
```

---

### Key Insight

Validation outputs are inputs to Risk.

---

### 7.2 Quantification

---

Instead of:

- “there is a problem”

We measure:

- how many problems exist  

---

### 7.3 Persistence

---

Validation is continuous:

- executed daily  
- supports trend analysis  

---

### 7.4 Explainability

---

Each validation must clearly explain:

- why it failed  
- why it triggered a warning  

---

## 8. Relationship with Other Layers

---

### Core Flow

```text
Metric → Validation → Drift → Risk
```

---

### Interpretation

- Metric → what is measured  
- Validation → is it correct  
- Drift → has it changed  

---

### Critical Insight

> Drift analysis is meaningless if validation fails

---

### Conceptual Model

- Validation = baseline integrity  
- Drift = change detection  

---

## 9. Observations in Real Systems

---

### Typical Pattern

- validation warnings occur continuously  
- drift alerts spike during specific events  

---

### Interpretation

> Structural weaknesses amplified by external changes

---

### Common Issues Detected

- incomplete mapping  
- weak funnel definition  
- inconsistent event structure  

---

## 10. Key Insights

---

### Validation always fails

Perfect data does not exist.

---

### Validation precedes Drift

It defines the baseline.

---

### Validation checks structure

It validates relationships, not just values.

---

### Validation drives Risk

In many systems, it contributes the majority of risk signals.

---

## 11. Conclusion

Validation Layer is not an error detection system.

It is:

> A system that determines whether data can be trusted

---

## One-line Summary

Validation = Making measured data trustworthy

---

## Portfolio Statement

Validation was designed not as a simple rule check,  
but as a continuously measurable reliability signal  
that integrates directly into the Risk Layer.

# Validation Layer

## Defining Data Reliability Through Systematic Validation

---

## 1. Problem

Most data pipelines follow a simple structure:


Raw Data → ETL → Metric → Dashboard


However, this structure assumes that the data is already correct.

In practice, this assumption does not hold.

---

### What Actually Happens

- Missing data  
- Incorrect event mapping  
- Broken user/session relationships  
- Funnel structure collapse  
- Inconsistent metrics  

---

### Result

> Data appears correct, but is fundamentally wrong

This leads to:

- incorrect analysis  
- unreliable decision-making  

---

## 2. Why Validation Matters

Validation is not just about detecting errors.

It is about answering a fundamental question:

> Can this data be trusted?

---

### Design Objectives

- Quantify data reliability  
- Detect structural issues  
- Enable continuous monitoring  
- Integrate with Risk layer  

---

## 3. Role in Architecture


Data → Metric → Validation → Drift → Risk


---

Validation acts as:

> A mandatory reliability filter before data is interpreted

---

## 4. Layer Structure

---

### 4.1 Input

- metric_value_day / hh  
- user, session, event metrics  
- funnel-based metrics  

---

### 4.2 Output


validation_result
validation_summary_day


---

---

### 4.3 validation_result

Stores individual validation outcomes.

---

#### Key Fields

- dt  
- metric_name  
- validation_type  
- validation_status (normal / warn / fail)  
- value  
- threshold  
- message  

---

---

### 4.4 validation_summary_day

Aggregates validation results at daily level.

---

#### Key Fields

- validation_fail_count  
- validation_warn_count  
- total_validation_count  

---

---

## 5. Validation Types

---

### 5.1 Completeness Validation

Checks whether data exists.

---

#### Examples

- page_view = 0  
- session_count sudden drop  

---

#### Interpretation

Indicates data pipeline issues.

---

---

### 5.2 Structural Validation

Checks relationships between metrics.

---

#### Examples

- session ≥ user  
- submit ≤ click  
- click ≤ view  

---

#### Interpretation

Indicates funnel structure breakdown.

---

---

### 5.3 Mapping Validation

Validates transformation from raw data to metrics.

---

#### Examples

- KV parsing failure  
- missing page_type  
- incorrect event mapping  

---

#### Interpretation

Indicates ETL or parsing issues.

---

---

### 5.4 Distribution Sanity Check

Validates whether values fall within expected ranges.

---

#### Examples

- conversion_rate > 1  
- negative latency  

---

---

## 6. Design Principles

---

### 6.1 From Rules to Signals

Traditional approach:


if error → log


---

This system:


validation → signal → aggregation → risk


---

Validation results are treated as structured data.

---

---

### 6.2 Quantification

Instead of:

- “there is a problem”

We measure:

- how many problems exist  

---

This is captured in:


validation_summary_day


---

---

### 6.3 Persistence

Validation is not a one-time check.

- executed daily  
- supports trend analysis  

---

---

### 6.4 Explainability

Each validation must answer:

- why it failed  
- why it triggered a warning  

---

---

## 7. Connection to Drift and Risk

---

### Core Flow


Validation → Drift → Risk


---

### Interpretation

- Validation → Is data correct now?  
- Drift → Has data changed?  
- Risk → Does this change matter?  

---

---

### Key Insight

- Validation detects structural issues  
- Drift detects environmental change  

---

Validation issues can exist continuously,  
while drift occurs when conditions change.

---

---

## 8. Role in Real Systems

---

### Observed Pattern

- validation warnings occur continuously  
- drift alerts spike under specific conditions  

---

### Interpretation

> Structural weaknesses amplified by external changes

---

---

### Typical Issues Detected

- incomplete mapping  
- undefined funnel structures  
- missing events  

---

---

## 9. Key Insights

---

### Validation always fails

Perfect data does not exist.

---

### Validation precedes Drift

Drift analysis is meaningless without correct data.

---

### Validation checks meaning, not just values

It validates relationships and structure.

---

### Validation is the starting point of Risk

In many systems, it contributes the majority of risk signals.

---

---

## 10. Conclusion

Validation Layer is not just an error detection mechanism.

It is:

> A system that defines whether data can be trusted

---

## One-line Summary

Validation = A layer that quantifies data reliability

---

## Portfolio Statement

Data validation was designed not as a simple error check,  
but as a continuously measurable reliability metric.

# Data Reliability Platform — Reliability Signal Layer (v0.2)

## 1. Overview

### Definition

The Reliability Signal layer transforms the state of a data pipeline  
into **quantified, interpretable risk signals**.

It does not directly expose anomaly detection results.  
Instead, it converts them into a form that can be used for operational decision-making.

---

### Purpose

The core flow is:

```

Raw → Anomaly → Signal → Risk → Root Cause → Action

```

Key idea:

- Anomalies alone are not actionable  
- Signals make anomalies interpretable  
- Signals enable risk computation  
- Risk enables decision-making  

---

### Position in v0.2 Architecture

```

Source
→ Stage
→ Validation / Drift / Stream Detection
→ Reliability Signal
→ Risk
→ Root Cause
→ Action

```

The Reliability Signal layer acts as the bridge between detection and decision.

---

## 2. Signal Layer Structure

The v0.2 design consists of two primary components:

---

### 2.1 Batch Signal

**Table**
- `data_risk_score_day_v3`

**Key Columns**

- validation_score  
- drift_score  
- anomaly_score  
- mapping_score  
- final_risk_score  
- risk_grade  

---

**Characteristics**

- Daily aggregation (dt-based)  
- Low noise and stable  
- Suitable for retrospective analysis  

---

### 2.2 Stream Signal

**Table**
- `stream_risk_signal_day`

**Key Columns**

- missing_rate  
- duplicate_ratio  
- ordering_gap_score  
- delay_ms  
- primary_stream_issue  
- stream_risk_score  

---

**Characteristics**

- Near real-time signal generation  
- Captures event-level anomalies quickly  
- Used for operational monitoring  

---

## 3. Signal Generation Logic

---

### 3.1 Batch Signal Computation

**Input**

- Validation results  
- Drift results  
- Anomaly results  
- Mapping results  

---

**Computation**

```

final_risk_score =
w1 * validation_score +
w2 * drift_score +
w3 * anomaly_score +
w4 * mapping_score

```

---

**Characteristics**

- Weighted aggregation  
- Stable risk estimation  
- Suitable for long-term trend analysis  

---

### 3.2 Stream Signal Computation

**Input**

Stream metrics derived from Kafka-based ingestion:

- missing  
- duplicate  
- ordering  
- delay  

---

### Metric Definitions

**Missing**
```

missing_rate = missing_count / expected_count

```

---

**Duplicate**
```

duplicate_ratio = duplicate_count / total_count

```

---

**Ordering**
```

ordering_gap_score = out_of_order_count

```

---

**Delay**
```

delay_ms = max(event_time - ingestion_time)

```

---

### Composite Risk Score

```

stream_risk_score =
max(
missing_score,
duplicate_score,
ordering_score,
delay_score
)

````

---

**Characteristics**

- Worst-case (max-based) aggregation  
- Prioritizes the most critical issue  
- Designed for fast operational response  

---

## 4. Primary Issue Selection

---

### Purpose

To select a single representative issue among multiple anomalies.

---

### v0.2 Logic

Priority order:

1. delay  
2. ordering  
3. missing  
4. duplicate  

---

### Limitation

- Strong bias toward delay  
- May hide the actual root issue  
- Can distort root cause interpretation  

---

## 5. Signal to Risk Bridge

---

### Table

- `data_risk_score_day_v3`

---

### Role

Integrates batch and stream signals into a unified risk view.

---

### Example Logic

```sql
UPDATE data_risk_score_day_v3 d
JOIN stream_risk_signal_day s
  ON d.profile_id = s.profile_id
 AND d.dt = s.dt
SET
  d.stream_risk_score = s.stream_risk_score,
  d.stream_primary_issue = s.primary_stream_issue,
  d.stream_status = s.status;
````

---

### Result

Enables unified access to:

* Batch risk
* Stream risk
* Combined operational state

---

## 6. Signal to Root Cause Mapping

---

### Table

* `data_risk_root_cause_day`

---

### Structure

```
signal → cause_type → cause_code → confidence
```

---

### Example

```
ordering anomaly
→ funnel_distortion
→ conversion_drop
```

---

## 7. Signal to Action Mapping

---

### Table

* `data_reliability_action_day`

---

### Structure

```
root_cause → action_type → priority
```

---

### Example

```
funnel_distortion
→ monitor_funnel
→ priority = medium
```

---

## 8. Design Characteristics of v0.2

---

### Strengths

**1. Clear Layer Separation**

* Signal
* Risk
* Root Cause
* Action

Each layer has a well-defined responsibility.

---

**2. Batch + Stream Integration**

* Historical analysis (batch)
* Real-time detection (stream)

Unified into a single framework.

---

**3. ML / AI Compatibility**

* Signals serve as features
* Signals serve as AI inputs

---

### Limitations

**1. Delay Bias**

* Primary issue selection favors delay

---

**2. Simplistic Aggregation**

* Max-based scoring can distort interpretation

---

**3. Static Thresholds**

* Does not adapt to changing environments

---

**4. False Positives**

* Normal situations may be classified as anomalies

---

## 9. Direction for v0.3

---

### 1. Dynamic Thresholds

* Static → adaptive

---

### 2. Multi-Issue Detection

* Single primary issue → multiple concurrent issues

---

### 3. Weighted Scoring

* Max-based → weighted aggregation

---

### 4. Context-aware Signals

* Incorporate scenario and exogenous factors

---

## 10. Key Takeaways

---

### Definition

Reliability Signal represents
the quantified state of data quality as operational risk.

---

### Structure

```
Batch Signal + Stream Signal
→ Risk Score
→ Root Cause
→ Action
```

---

### v0.2 Characteristics

* Rule-based
* Threshold-based
* Max aggregation
* Single primary issue

---

### Most Important Point

The Reliability Signal layer is not about detection.

It is about transforming data anomalies into a
**decision-ready representation of system risk**.

---

## One-line Summary

The v0.2 Reliability Signal layer converts data anomalies
into a structured form that enables risk-based decision-making.

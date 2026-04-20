# Data Modeling — Data Reliability Platform v0.2 (Engineering Notes)

## Overview

The v0.2 data model is not a traditional OLTP or data warehouse schema.

It is designed as an operational data model that supports the full lifecycle of data reliability:

- Detect anomalies  
- Translate signals into risk  
- Identify root causes  
- Drive operational actions  
- Enable ML/AI-based interpretation  

The goal of this document is not to describe tables,
but to help engineers quickly understand:

- where to start  
- how data flows  
- which tables matter  
- how to minimize development and debugging effort  

---

## 1. Core Data Flow

The entire system can be understood through a single flow:

```

raw → stream summary → stream signal → truth → ml → ai
↘
final risk → root cause → action

```

All tables exist to support this flow.

---

## 2. Layered ERD (Engineering View)

### 2.1 Source / Raw Layer

**Key Table**
- `event_log_raw`

**Role**
- Single source of truth
- Canonical event layer

**When to check**
- Data itself looks incorrect  
- Scenario injection validation  

---

### 2.2 Stage / Load Layer

**Key Tables**
- `stg_webserver_log_hit`
- `stg_event_batch`
- `stg_event_stream`

**Role**
- Ingestion separation
- Batch vs stream processing

**When to check**
- Ingestion issues  
- Batch/stream mismatch  

---

### 2.3 Validation / Mapping / Drift Layer

**Role**
- Generate signals instead of using raw data directly

**Includes**
- Validation (null, rules)
- Mapping (semantic translation)
- Drift (baseline comparison)

**When to check**
- Data values look correct but behavior is abnormal  
- Changes relative to baseline  

---

### 2.4 Stream Detection Layer

**Key Table**
- `stream_reliability_summary_day`

**Role**
- Aggregates stream events into reliability metrics

**When to check**
- Detect stream anomalies  

---

### 2.5 Risk / Bridge Layer (Core)

**Key Tables**
- `stream_risk_signal_day`
- `data_risk_score_day_v3`

**Role**

`stream_risk_signal_day`
- Converts stream metrics into operational risk signals  
- Defines `primary_stream_issue`

`data_risk_score_day_v3`
- Aggregates batch and stream risk  
- Final daily risk indicator  

**Flow**
```

stream summary → stream signal → final risk

```

**When to check**
- To understand why a specific day is problematic  
- Entry point for all analysis  

---

### 2.6 Truth / Scenario Layer

**Key Table**
- `stream_anomaly_truth_day`

**Role**
- Provides ground truth for supervised ML

**When to check**
- Model validation  
- Scenario accuracy  

---

### 2.7 ML Layer

**Key Tables**
- `vw_stream_ml_training_dataset_v4`
- `stream_ml_prediction_day`
- `stream_ml_risk_prediction_day`

**Role**

`vw_stream_ml_training_dataset_v4`
- Feature dataset combining signal and truth  

Prediction tables:
- Classification (anomaly type)
- Regression (risk severity)

**When to check**
- Model output validation  
- Feature-level issues  

---

### 2.8 AI Layer

**Key Tables**
- `ai_stream_incident_context_day`
- `ai_incident_summary_day`
- `ai_recommended_action_day`

**Role**
- Combines signal, truth, and prediction  
- Generates explanation and actions  

**When to check**
- Final interpretation  
- Operational decision support  

---

## 3. Data Dictionary (Core Tables)

### event_log_raw

- Role: canonical source  
- Grain: event-level  
- Key: event_id  
- Upstream: raw logs  
- Downstream: all layers  

**Note**
Changes here impact the entire system  

---

### stream_reliability_summary_day

- Role: stream metric aggregation  
- Grain: (profile_id, dt)  
- Downstream: stream_risk_signal_day  

---

### stream_risk_signal_day

- Role: core risk signal  
- Grain: (profile_id, dt)  

**Key Fields**
- primary_stream_issue  
- stream_risk_score  

**Importance**
Primary input for all downstream decisions  

---

### stream_anomaly_truth_day

- Role: ground truth dataset  
- Downstream: ML  

---

### data_risk_score_day_v3

- Role: final risk aggregation  
- Downstream:
  - root cause  
  - action  
  - dashboards  

---

### data_risk_root_cause_day

- Role: root cause analysis  

---

### data_reliability_action_day

- Role: action recommendation  

---

### vw_stream_ml_training_dataset_v4

- Role: ML feature dataset  
- Includes:
  - signal  
  - truth  
  - engineered features  

---

### stream_ml_prediction_day

- Role: anomaly classification  

---

### stream_ml_risk_prediction_day

- Role: risk regression  

---

### ai_stream_incident_context_day

- Role: AI input layer  
- Combines:
  - signal  
  - truth  
  - prediction  

---

### ai_incident_summary_day / ai_recommended_action_day

- Role:
  - explanation generation  
  - action recommendation  

---

## 4. Engineering Efficiency (Critical)

### 4.1 Tables to Start With (Current System)

Focus on these first:

- data_risk_score_day_v3  
- stream_risk_signal_day  
- stream_anomaly_truth_day  
- vw_stream_ml_training_dataset_v4  
- stream_ml_prediction_day  
- stream_ml_risk_prediction_day  
- ai_stream_incident_context_day  

Understanding these gives you the entire system.

---

### 4.2 Contracts That Must Be Fixed First

These must be stabilized before any major changes:

1. threshold_scope  
2. cause_source  
3. action_source  
4. primary_stream_issue  
5. representative bridge rule  
6. training dataset view version  

Changes here propagate across all downstream layers.

---

## 5. Strengths

- End-to-end structure (raw → AI)  
- Explainable by design  
- ML-ready architecture  
- Unified batch and stream  

---

## 6. Limitations

- Multiple staging layers  
- Ordering inconsistencies  
- Partial contract definition  
- Limited injection flexibility  

---

## 7. Key Takeaway

The v0.2 data model is not a static schema.

It is an evolving operational model designed to interpret
data reliability issues and translate them into actionable insights.

---

## One-line Summary

This model does not define tables —  
it defines how data risk is interpreted across the system.
```

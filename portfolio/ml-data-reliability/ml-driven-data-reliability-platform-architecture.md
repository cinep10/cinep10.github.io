# ML-driven Data Reliability Platform Architecture

An end-to-end system for data quality validation, anomaly detection, root cause analysis, and AI-driven operational decision support

---

## 1. Overview

This project is not a simple data quality validation system.

It is designed as an end-to-end data reliability platform that:

- detects anomalies in data
- explains why they occurred
- quantifies risk
- classifies system state using ML
- translates results into actionable operations using AI

The system evolves from a rule-based validation pipeline into a layered architecture:

Data → Validation → Drift → Structural → Risk → Root Cause → ML → AI

---

## 2. Design Principles

### 2.1 Data Reliability as Dynamic Risk Management

Data quality is not treated as a static validation problem.

Instead, it is defined as a dynamic system where:

- data may be valid but abnormal
- patterns may shift over time
- structural relationships may break

The goal is to detect meaningful deviations, not just invalid values.

---

### 2.2 Explainability-first Architecture

Every layer must produce interpretable outputs.

- Validation explains what is wrong
- Drift explains how much it changed
- Structural explains what broke
- Risk explains severity
- ML explains contributing features
- AI explains what to do

---

### 2.3 Layer Separation

Each layer answers a distinct question:

- Validation: Is the data incorrect?
- Drift: Has the data changed from baseline?
- Structural: Has the structure or relationship changed?
- Risk: How severe is the situation?
- ML: What is the current state?
- AI: What actions should be taken?

This separation ensures interpretability and operational clarity.

---

### 2.4 Failure-safe Design

All layers are designed to tolerate failure.

- ML fallback: rule-based classification
- AI fallback: rule-based reasoning
- external API failure does not break pipeline

---

## 3. System Architecture

```text
[DATA LAYER]
Raw Log → ETL → stg_webserver_log_hit

        ↓

[METRIC LAYER]
metric_value_day

        ↓

[SEMANTIC LAYER]
event_mapping → mapping_coverage_day

        ↓

[VALIDATION LAYER]
validation_result → validation_summary_day

        ↓

[DRIFT LAYER]
metric_drift_result_r

        ↓

[STRUCTURAL ANOMALY]
time anomaly / correlation anomaly

        ↓

[RISK SCORING]
data_risk_score_day

        ↓

[ROOT CAUSE]
root_cause_result

        ↓

--------------------------------------

[ML FEATURE LAYER]
ml_feature_vector_day

        ↓

[ML MODEL]
ml_risk_model

        ↓

[ML PREDICTION]
ml_prediction_result

        ↓

--------------------------------------

[AI INTERPRETATION]
incident summary / action recommendation

        ↓

[OBSERVABILITY]
Dashboard (Grafana)

```

---

## 4. Pre-ML Reliability Pipeline

### 4.1 Validation Layer

Role:

- ensure data completeness, validity, and consistency
- generate data quality signals

Key functions:

- null and missing checks
- expected range validation
- rule-based validation
- statistical anomaly detection

Output:

validation_summary_day

---

### 4.2 Drift Layer

Role:

- measure deviation from baseline behavior

Key methods:

- z-score deviation
- PSI (distribution shift)
- ratio change
- funnel change

Output:

metric_drift_result_r

---

### 4.3 Structural Anomaly Detection

Role:

- detect pattern and relationship breakdowns

Components:

- time-series anomaly detection
- inter-metric correlation anomaly

Focus:

- funnel distortion
- dependency breakdown

---

### 4.4 Risk Scoring

Role:

- aggregate all signals into a unified risk score

Inputs:

- validation results
- drift signals
- structural anomalies

Output:

data_risk_score_day

---

### 4.5 Root Cause Analysis

Role:

- identify which metrics contribute most to anomalies

Output:

- top contributing metrics
- contribution scores
- causal relationships

---

## 5. ML Feature Layer

### Role

Transform pre-ML signals into a structured feature vector for learning.

---

### Inputs

- metric_value_day
- validation_summary_day
- metric_drift_result_r
- structural anomaly outputs
- risk score
- scenario metadata

---

### Feature Composition

metric + quality + drift + anomaly + risk + scenario

---

### Key Design

ML does not consume raw data.

It consumes data that has already been interpreted through the reliability pipeline.

---

### Label Strategy

Labels are generated using scenario-aware logic:

- no scenario → normal
- campaign spike → softened
- incident scenarios → warning / alert
- fallback → risk score

Final classes:

normal / warning / alert

---

## 6. ML Layer

### Role

Classify system state based on feature vectors.

---

### Model

Logistic Regression

---

### Rationale

- interpretable
- stable in production
- easy to debug

---

### Key Features

- class imbalance handling
- fallback-safe training
- probability-based prediction

---

### Output

ml_prediction_result

- predicted_label
- probability distribution

---

### Key Insight

Feature engineering and labeling are more critical than model complexity.

---

## 7. AI Interpretation Layer

### Role

Translate ML and reliability signals into operational insights.

---

### Functions

- incident summarization
- technical explanation
- action recommendation

---

### Design Principles

AI is not a decision-maker.

All decisions are made by:

- validation
- drift
- structural analysis
- risk scoring
- ML

AI interprets these results.

---

### Failure Handling

- LLM success → full reasoning
- LLM failure → rule-based fallback

---

### Outputs

- ai_incident_summary_day
- ai_recommended_action_day

---

### Meaning

AI acts as a reasoning and recommendation layer, not a detection layer.

---

## 8. System Characteristics

### End-to-End Coverage

From data ingestion to action recommendation.

---

### Explainability

All outputs are interpretable and traceable.

---

### Hybrid Approach

Combines:

- rule-based logic
- statistical methods
- ML classification
- AI interpretation

---

### Scenario-driven Design

- anomaly simulation
- supervised learning enabled
- realistic operational scenarios

---

## 9. Current State

| Layer | Status |
|------|--------|
| Pre-ML | Production-ready |
| ML | Implemented |
| AI | Fallback-based operational |

---

## 10. Future Directions

### Explainability Enhancement

- SHAP integration
- feature-level explanation

---

### AI Recommendation Engine

- ranking-based action model

---

### Drift-aware ML

- automated retraining

---

### Feedback Loop

- user feedback integration
- model improvement cycle

---

## Summary

This system transforms data reliability into an operational framework.

It connects:

- validation
- anomaly detection
- risk scoring
- classification
- explanation
- action

into a single architecture.

---

## One-line Definition

A data reliability platform that detects anomalies, explains root causes, classifies system state using ML, and translates results into actionable decisions through AI.


⸻

🔥 진짜 중요한 포인트

이 버전은:
	•	✔ Jekyll / GitHub Pages 바로 사용 가능
	•	✔ 줄바꿈 / 코드블럭 깨짐 없음
	•	✔ 모바일 복붙 안정

⸻

원하면 다음 단계로:

👉 이 글을 Case Study 연결 구조
👉 또는 컨설팅 상품 페이지로 변환

까지 이어서 만들어드릴게요 👍
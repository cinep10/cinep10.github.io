# Data Reliability Platform — ML / AI Engineering (v0.2)

## 1. Overview

In v0.2, ML / AI is not treated as a standalone modeling project.

Instead, it is designed as an **integrated engineering layer within a data reliability platform**, responsible for:

- classifying system state  
- quantifying risk  
- providing interpretable outputs for operational decision-making  

This document focuses on:

- why ML / AI was introduced  
- how it is structurally integrated  
- what engineering challenges were encountered  
- how it operates in a production-like pipeline  

---

## 2. Why ML / AI Is Required

Before v0.2, the system relied on:

- validation / drift / anomaly detection  
- rule-based risk scoring  
- root cause and action mapping  

This approach had limitations:

- no standardized state classification  
- limited quantitative interpretation of severity  
- inconsistent explanation of results  

---

ML / AI is introduced to solve:

- classification of anomaly types  
- regression-based risk estimation  
- structured interpretation layer  

---

## 3. Architectural Position

ML / AI does not operate directly on raw data.

It follows a structured pipeline:

```
Raw → Signal → Truth → Feature → ML → Prediction → AI → Action
```

This ensures:

- explainability  
- stability  
- reusability across domains  

---

## 4. ML Engineering Structure

### 4.1 Core Objectives

ML solves two distinct problems:

#### Classification

- What type of anomaly is this?

Classes:
- normal  
- missing  
- duplicate  
- ordering  
- delay  

---

#### Regression

- How severe is the issue?

Output:
- continuous risk score  

---

## 5. Truth Engineering

### Key Table

- `stream_anomaly_truth_day`

### Role

- provides labeled data for supervised learning  
- defines anomaly ground truth  

---

### Characteristics

- scenario-driven generation  
- aligned with signal layer  
- serves as the training reference  

---

## 6. Feature Dataset Design

### Key View

- `vw_stream_ml_training_dataset_v4`

---

### Design Principle

ML does not consume raw data directly.

Instead:

- signals are transformed into features  
- truth is joined to define labels  

---

### Feature Engineering Strategy

#### 1. Absolute Values

- missing_rate  
- duplicate_ratio  

---

#### 2. Differences

- change from previous state  
- spike detection  

---

#### 3. Statistical Features

- moving average  
- standard deviation  

---

#### 4. Previous State

- prev_issue  

---

#### 5. Temporal Features

- day-of-week  
- periodic patterns  

---

#### 6. Cross Features

- relationships between metrics  
- compound anomaly detection  

---

## 7. Training Architecture

### Classification

Input:
- feature dataset  

Output:
- anomaly type  

---

### Regression

Input:
- same feature dataset  

Output:
- risk severity  

---

### Characteristics

- shared dataset  
- multi-task structure  
- separation of type and severity  

---

## 8. Prediction as an Operational Layer

ML outputs are not transient.

They are stored as part of the system.

---

### Key Tables

- `stream_ml_prediction_day`  
- `stream_ml_risk_prediction_day`  

---

### Purpose

- persistent prediction results  
- integration with downstream systems  
- support for dashboards and AI  

---

## 9. Evaluation and Monitoring

Model outputs are evaluated through:

- prediction vs truth comparison  
- classification reports  
- confusion matrix  
- regression metrics  

---

### Role

- validate model behavior  
- detect data or feature issues  

---

## 10. AI Engineering Structure

### 10.1 Role of AI

AI does not replace ML.

It acts as a **structured interpretation layer**.

---

### 10.2 Architecture

```
Signal + Truth + Prediction
→ Context
→ Summary
→ Action
```

---

### Key Tables

- `ai_stream_incident_context_day`  
- `ai_incident_summary_day`  
- `ai_recommended_action_day`  

---

### Characteristics

- context-driven, not generative-first  
- rule + structured input based  
- deterministic explainability focus  

---

## 11. What Determines AI Quality

AI quality depends primarily on:

- signal quality  
- truth accuracy  
- prediction stability  

---

Not on prompt engineering alone.

---

## 12. Engineering Lessons from v0.2

The most critical aspects are not the models themselves, but:

---

### 1. Truth Engineering

- defining correct labels  
- maintaining consistency  

---

### 2. Feature Contract Engineering

- stable feature definitions  
- version control  

---

### 3. Prediction Storage Engineering

- storing results as system outputs  
- enabling reuse  

---

### 4. Context Engineering

- structuring inputs for AI  
- ensuring explainability  

---

## 13. Current vs Legacy Assets

### Current ML Assets

- stream_anomaly_truth_day  
- vw_stream_ml_training_dataset_v4  
- train_stream_multiclass_model_v2.py  
- train_stream_risk_regressor_v1.py  
- stream_ml_prediction_day  
- stream_ml_risk_prediction_day  

---

### Current AI Assets

- ai_stream_incident_context_day  
- ai_incident_summary_day  
- ai_recommended_action_day  

---

### Legacy / Mixed Assets

- ml_feature_vector_day  
- ml_prediction_result  
- vw_stream_ml_training_dataset_v1/v2/v3  
- older prediction / evaluation artifacts  

---

## 14. Strengths

- signal-driven ML design  
- explainable pipeline structure  
- operational prediction layer  
- structured AI integration  

---

## 15. Limitations

- feature drift not fully managed  
- limited context depth  
- rule-based explainability constraints  
- legacy structures remain  

---

## 16. Direction for v0.3

- feature versioning  
- dynamic thresholds  
- context-aware AI  
- multi-issue classification  
- domain expansion  

---

## 17. Key Takeaways

### ML Structure

```

Signal → Truth → Feature → Prediction

```id="ml-core"

- classification: type of anomaly  
- regression: severity  

---

### AI Structure

```

Signal + Truth + Prediction
→ Context → Summary → Action

```id="ai-core"

---

## 18. One-line Summary

ML / AI in v0.2 is not about building models,  
but about integrating classification, risk estimation, and explainability  
into a structured, operational data reliability system.

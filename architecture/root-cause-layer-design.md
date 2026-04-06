
# Root Cause / Action Layer

A decision layer that decomposes risk into explainable causes and translates them into operational actions

---

## 1. Overview

This system does not stop at detecting anomalies.

Even after computing:

- Validation  
- Drift  
- Structural Anomaly  
- Risk Score  

operators still need to answer fundamental questions:

- What is the problem?  
- Why did the risk increase?  
- Where should we look first?  
- What actions should we take?  

The layers that answer these questions are:

- Root Cause / Contribution Layer  
- Action Engine Layer  

These layers form the critical bridge between:

**Detection → Operational Decision**

---

## 2. Position in the Architecture

Root Cause and Action layers are positioned after the Risk Score layer.
Validation / Drift / Structural / Mapping
↓
Risk Score
↓
Root Cause / Contribution Layer
↓
Action Engine Layer
↓
ML / AI / Dashboard


This structure can be summarized as:

**Transforming risk signals into explainable causes and actionable operations**

---

## 3. Root Cause / Contribution Layer

### 3.1 Role

The Root Cause Layer explains the factors that contributed to the final risk score.

If the Risk Score answers:

> "The system is at high risk today"

Then the Root Cause Layer answers:

> "What caused the risk to increase?"

---

### 3.2 Design Objectives

The layer is designed with three main goals:

#### 1) Explainability

A single risk score is not sufficient for operators.

The system must explain:

- Which signals were significant  
- Which metrics were affected  
- What type of issue occurred  

---

#### 2) Signal Prioritization

Multiple anomaly signals can occur simultaneously.

Not all signals are equally important.

This layer:

- ranks signals  
- highlights dominant causes  
- enables fast triage  

---

#### 3) Action Readiness

The Action Engine requires structured input.

Root Cause Layer prepares:

- normalized cause types  
- linked signals  
- prioritized causes  

---

### 3.3 Input Data

The Root Cause Layer integrates multiple upstream signals.

- data_risk_score_day_v3  
- validation_summary_day  
- metric_drift_result_r  
- metric_time_anomaly_day  
- metric_correlation_anomaly_day  
- mapping_coverage_day  

This is not a simple lookup.

It is a **re-interpretation of all upstream signals**.

---

### 3.4 Output Structure

#### data_risk_root_cause_day

Stores ranked causes.

- profile_id  
- dt  
- cause_rank  
- cause_type  
- related_metric  
- contribution_score  

This table answers:

**Which causes contributed most to the risk**

---

#### risk_signal_link_day

Stores relationships between signals and causes.

Examples:

- validation_fail → pipeline_issue  
- drift_alert → traffic_mix_shift  
- correlation anomaly → funnel_distortion  
- mapping_drop → mapping_issue  

This enables:

**structured explanation instead of isolated signals**

---

### 3.5 Cause Type Abstraction

Raw signals are converted into operational cause types.

Examples:

- funnel_distortion  
- traffic_mix_shift  
- mapping_issue  
- pipeline_issue  
- metric_drift  

This is a critical transformation:

**Signal → Operational Meaning**

---

### 3.6 Contribution-Based Ranking

Each cause is assigned a contribution score.

Conceptually:
contribution_score =
signal_strength
* severity_weight
* confidence_weight


This allows:

- prioritization  
- explainability  
- downstream decision-making  

---

### 3.7 System Significance

This layer is the core of:

**Explainable Data Behavior**

It transforms:

- numeric risk → structured explanation  

---

## 4. Action Engine Layer

### 4.1 Role

The Action Engine converts root causes into operational actions.

If Root Cause answers:

> "What happened?"

Then Action Engine answers:

> "What should we do?"

---

### 4.2 Design Objectives

#### 1) Convert Explanation into Action

Example:

- funnel_distortion → investigate conversion flow  
- mapping_issue → verify mapping rules  

---

#### 2) Standardize Actions

Actions are not free text.

They are categorized:

- mapping_fix  
- funnel_fix  
- traffic_investigate  
- pipeline_check  
- anomaly_investigate  
- monitor_funnel  

This creates:

**an operational action catalog**

---

#### 3) Support ML / AI / Dashboard

- AI uses action candidates for recommendations  
- dashboards display actionable items  
- ML benefits from structured signals  

---

### 4.3 Input Data

- data_risk_root_cause_day  
- risk_signal_link_day  
- data_risk_score_day_v3  

The Action Engine reads:

**risk + cause + signal relationships**

---

### 4.4 Output Structure

#### data_reliability_action_day

- profile_id  
- dt  
- root_cause  
- action_type  
- priority  
- recommended_fix  

This table represents:

**a prioritized operational task list**

---

### 4.5 Action Generation Logic

#### Cause → Action Mapping

Examples:

- mapping_issue → mapping_fix  
- funnel_distortion → funnel_fix  
- traffic_mix_shift → traffic_investigate  
- pipeline_issue → pipeline_check  

---

#### Metric-Aware Action

Actions are contextualized with metrics.

Examples:

- Check submit_capture_rate  
- Check daily_active_users  
- Check mapping_coverage  

---

#### Priority Assignment

Priority is based on:

- risk grade  
- cause rank  
- cause criticality  

This enables:

**what to do first**

---

## 5. Decision Flow Perspective

These layers together form a decision framework:
Risk Score → Problem Severity
Root Cause → Problem Explanation
Action Engine → Operational Response


This is effectively:

**Score → Explanation → Action**

---

## 6. System Strengths

### 6.1 Beyond Detection

Most systems stop at alerts.

This system continues with:

- cause decomposition  
- signal linkage  
- action recommendation  

---

### 6.2 ML / AI Integration

- ML uses structured signals for classification  
- AI uses root cause and actions for reasoning  

Explainability is reused downstream.

---

### 6.3 Operational Design

This is not a research model.

It is designed for:

- real operators  
- real decisions  
- real actions  

---

## 7. Limitations

### 7.1 Limited Cause Taxonomy

Currently focused on web-log domain.

---

### 7.2 Priority Simplification

Can be extended with:

- SLA  
- business criticality  

---

### 7.3 No Explicit Decision Layer

Decision logic is partially embedded.

A dedicated decision layer could improve clarity.

---

## 8. Future Directions

### 8.1 Root Cause Enhancement

- refined contribution scoring  
- stronger signal graph  
- expanded cause taxonomy  

---

### 8.2 Action Engine Enhancement

- owner_hint  
- priority refinement  
- richer action templates  

---

### 8.3 Domain Expansion (Finance)

Potential extensions:

- approval_issue  
- execution_missing  
- balance_inconsistency  

This layer becomes even more critical in financial systems.

---

## 9. Summary

The Root Cause / Action Layer:

decomposes risk into explainable causes  
and translates those causes into operational actions

This transforms the system from:

- anomaly detection  
- risk scoring  
- ML classification  

into:

**an operational Data Reliability Framework**

---

## One-line Definition

Root Cause / Action Layer =  
A decision layer that explains risk through structured causes and translates them into actionable operations

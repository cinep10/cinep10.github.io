# Explainable Data Behavior

## Building a System That Explains How Data Moves

---

## 1. Overview

Most data systems focus on collecting, processing, and visualizing data.

However, they rarely answer a more important question:

> Why is the data behaving this way?

---

This project was designed to go beyond traditional analytics.

Not to detect anomalies.  
Not to build ML models.

But to:

> Build a system that explains data behavior in a structured and interpretable way.

---

### Core Direction

- anomaly detection ❌  
- model-first ML ❌  

→ **data structure + distribution + meaning-based interpretation ⭕**

---

### Key Question

> Can we explain why data changed?

---

## 2. Architecture (Pre-ML Layer)

The system is structured as a layered interpretation pipeline.


Raw Log
→ Parsing / Load
→ Metric Generation

[VALIDATION]
→ validation_summary_day

[DRIFT]
→ metric_drift_result

[RISK]
→ data_risk_score_day

[INTERPRETATION]
→ data_risk_root_cause_day

[ACTION]
→ data_reliability_action_day

[LINKAGE]
→ risk_signal_link_day

[STRUCTURAL]
→ correlation anomaly / time anomaly

[OBSERVABILITY]
→ Grafana Dashboard


---

### Key Concept

> Detection is not enough.

This system connects:

**Detection → Interpretation → Action**

---

## 3. Observability with Grafana

### 📊 Dashboard Overview

![Dashboard Overview](./images/dashboard_overview.png)

---

### (1) Validation

- Fail: 4  
- Warn: 26  

**Interpretation**

- pipeline is functioning  
- some rule-based quality issues exist  

→ Data is usable but not perfect  

---

### (2) Drift

- Alert metrics: 175  
- Warn metrics: 57  

**Interpretation**

- significant distribution changes detected  
- not just noise  

→ behavioral shift in users or traffic  

---

### (3) Root Cause

Main causes:

- funnel_distortion  
- metric_drift  

---

### Evolution

| Before | After |
|------|------|
| funnel_distortion only | drift + funnel separated |

---

**Meaning**

> The system now distinguishes:

- structural issues  
- behavioral changes  

---

### (4) Funnel State

Examples:

- auth_attempt → exists  
- auth_success → exists  
- card_apply_submit → exists  
- loan_apply_submit → exists  

---

**Interpretation**

- funnel is working  
- no missing-event issue  

→ problem is not absence  
→ but **conversion change**

---

### (5) Conversion Metrics

- auth_success_rate ≈ 0.32  
- card_conversion ≈ 0.45  
- loan_conversion ≈ 0.45 ~ 0.52  

---

**Interpretation**

- overall within normal range  
- local abnormal patterns detected  

---

### (6) Mapping Coverage

- mostly stable  
- partial degradation in some segments  

---

**Interpretation**

- not primary root cause  
- but contributing factor  

---

### (7) Risk / Action / Linkage

- Risk score generated  
- Action triggered (mapping_fix, pipeline_issue)  
- Linkage active  

---

**Interpretation**

> The system now connects:

problem → cause → action

---

## 4. Key Components Explained

---

### Validation


Rule-based data integrity check


- completeness  
- mapping consistency  
- funnel constraints  

---

### Drift


Distribution change detection


- traffic shift  
- behavior change  
- feature variation  

---

### Funnel Metrics


Step-wise behavioral structure


Example:


auth → apply_start → submit


---

### Conversion Metrics


Business outcome indicators


- auth_success_rate  
- card_conversion  
- loan_conversion  

---

### Root Cause


Categorized explanation of issues


- funnel_distortion → structural problem  
- metric_drift → behavioral change  
- mapping_gap → data definition issue  

---

### Action


Recommended response


- mapping_fix  
- funnel_fix  
- pipeline_issue  
- anomaly_investigation  

---

## 5. Key Results

---

### 1. The problem was not ML

Most issues were caused by:

- data structure  
- funnel definition  
- mapping consistency  

---

### 2. Explainability achieved

Before:


Anomaly detected → unknown reason


After:


Anomaly detected
→ drift?
→ funnel issue?
→ mapping issue?
→ what action to take?


---

### 3. System maturity

| Stage | Status |
|------|------|
| Data ingestion | ✅ |
| Validation | ✅ |
| Drift detection | ✅ |
| Root cause analysis | ✅ |
| Action system | ✅ |
| Explainability | ✅ |

---

## 6. Career Perspective (Important)

This project is not about building a pipeline.

It demonstrates:

> Designing a system that understands and explains data behavior.

---

### Positioning

- Data Engineer ❌  
- Analyst ❌  

→ **Data Reliability / Data Architect ⭕**

---

### Core Capability

- defining data meaning  
- validating data structure  
- detecting behavioral change  
- translating issues into risk  
- explaining root causes  

---

## 7. Future Directions

---

### Scenario-based testing

- campaign  
- external events  
- system anomalies  

---

### Root cause refinement

- domain-specific thresholds  
- funnel sensitivity tuning  

---

### Action engine automation


IF funnel_distortion
→ check mapping coverage
→ detect missing events
→ suggest fix


---

### ML integration


feature → risk prediction


→ Explainable ML layer

---

## Final Insight

> We are no longer observing data.

> We are understanding how data behaves.

---

## One-line Summary

Explainable Data Behavior =
A system that interprets, explains, and operationalizes data changes

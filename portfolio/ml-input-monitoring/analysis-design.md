# Explainable Data Behavior: System Design

## 1. Problem Definition

Traditional anomaly detection systems have structural limitations:

- They detect anomalies but cannot explain why
- Results are reduced to a simple status (normal / abnormal)
- Difficult to translate into operational actions

This project aims to solve these limitations by redefining the goal:

> Not detecting anomalies, but building **Explainable Data Behavior**

---

## 2. Design Principles

The system is not ML-centric, but **data reliability-centric**.


Metric → Drift → Risk Score → Root Cause → ML → Scenario


Key principles:

- The system must work without ML
- ML is used only as a supporting layer

---

## 3. Risk-Based Architecture

Instead of anomaly detection, a risk-based model is used:


risk_score =
validation_score

drift_score
anomaly_score
mapping_score

Advantages:

- Fully interpretable
- Explainable at metric level
- Directly connected to root cause

---

## 4. Role of ML

ML is intentionally limited to the following roles:

1. Probability estimation based on risk_score
2. Feature importance extraction
3. Supporting explanation of drift impact

Therefore:

- ML is not a decision engine
- ML is an **explainability layer**

---

## 5. Critical Design Decision

The most important decision:

> The system must be fully functional without ML

This led to:

- Rule-based risk engine implemented first
- Root cause analysis designed before ML
- ML placed at the final stage

---

## 6. Scenario-Based Design

To simulate real-world failures:

- funnel_break
- auth_failure
- mixed_incident

These scenarios validate the full pipeline:


Cause → Metric Change → Drift → Risk → Root Cause → ML


---

## 7. Integration with Data Reliability

This system extends the data reliability framework:

- Validation → data quality
- Drift → distribution change
- Mapping → schema consistency

It is more accurate to describe this as:

> A **data reliability extension system**, not an ML system

---

## 8. Conclusion

The key idea:

> The goal is not anomaly detection,  
> but building a system that explains data behavior

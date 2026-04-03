# Architecture

This section describes the data system not as isolated technologies,
but from the perspective of the **data engineering lifecycle and data reliability**.

Data flows through the following stages:

```text
Data Generation → Storage → Ingestion → Transformation → Analysis → Machine Learning
```

The goal of this architecture is not simply to build a data pipeline,
but to design a system that makes data **reliable and explainable**.

---

## Architecture Philosophy

This architecture is designed to answer the following questions:

* Can the data be trusted?
* How is the data changing over time?
* Is the change risky?
* Why did this issue occur?
* What actions should be taken?

This system is not just a data processing pipeline,
but a structure that connects **data state → interpretation → decision-making**.

---

## 1. Data Generation

The starting point of any data system is source data.

Unlike traditional architectures that rely on real production logs,
this system treats **data generation itself as a design problem**.

Instead of using real logs,
data is generated using a simulator that models real-world behavior.

This simulator incorporates:

* user behavior flows (sessions and funnels)
* identity structure (pcid, sid, uid)
* temporal patterns (hourly, weekday)
* exogenous variables (weather, campaigns, system conditions)
* probabilistic event distributions

It also allows controlled injection of anomalies such as drift and failures.

This enables:

* reproducible data environments
* controlled anomaly experiments
* generation of ML training data

Related:

* [Synthetic Web Log Simulator Architecture](/architecture/synthetic-web-log-simulator-architecture)

---

## 2. Data Storage

The usability of data is determined by how it is stored.

This layer defines the structure and format of the data.

* Apache log format
* KV-based extensible fields
* raw data preservation

The storage design becomes the foundation for all downstream processing.

---

## 3. Data Ingestion

Raw data cannot be used directly and must be transformed into structured data.

This stage includes:

* log parsing
* field normalization
* data loading

Result:

* stg_webserver_log_hit

This is the starting point of the data processing pipeline.

---

## 4. Data Transformation

This stage converts data into measurable and interpretable forms.

---

### 4.1 Metric Generation

* transform logs into measurable metrics
* user, session, and event-based aggregation
* time-based aggregation structures

This layer defines how data is measured.

---

### 4.2 Semantic Interpretation

* convert URLs and events into business-level meaning
* define event types and categories
* establish funnel structures

At this stage, raw logs become **interpretable business events**.

---

## 5. Data Analysis & Reliability

This is the core of the architecture.

The goal is not just analysis,
but determining whether the data is **valid and stable over time**.

---

### 5.1 Validation

Ensures structural and logical correctness of data.

* value validation
* relationship validation
* rule-based anomaly detection

Related:

* [Drift Detection Design](/architecture/analytics-pipeline-validation-framework)

---

### 5.2 Drift Detection

Detects changes in data distribution and patterns.

* temporal changes
* distribution shifts
* structural changes

Related:

* [Drift Detection Design](/architecture/drift-detection-design)

---

### 5.3 Risk Scoring

Combines validation and drift signals into a unified risk score.

* signal-based aggregation
* impact-based weighting

Related:

* [Risk Scoring Design](/architecture/risk-scoring-design)

---

### 5.4 Decision Framework

Transforms data states into operational decisions.

* OK / WARN / FAIL classification
* automated decision rules

Related:

* [Data Decision Framework](/architecture/data-decision-framework)

---

## 6. Root Cause & Action

This stage moves beyond detection to explanation and response.

* root cause analysis
* impact interpretation
* action recommendation

This connects detection → diagnosis → action.

---

## 7. Machine Learning Extension

The data reliability architecture extends into ML systems.

* feature-level data management
* linking data changes to model impact
* interpreting prediction behavior

At this stage, data quality directly affects model performance.

---

## Data Reliability Perspective

The core concept of this architecture is **Data Reliability**.

Data Reliability is not just about data quality,
but about continuously observing and interpreting the state of data.

The system is structured as follows:

Data
→ Metric
→ Validation (correctness)
→ Drift (change detection)
→ Risk (impact assessment)
→ Root Cause (explanation)
→ Action (response)

Through this flow, data is transformed from raw values into
**interpretable system state information**.

---

## Summary

This architecture connects the entire lifecycle from data generation to machine learning.

It enables:

* reliable data systems
* interpretable behavior
* explainable decision-making

By designing the data generation layer itself,
the system provides both reproducibility and controllability.

---

## One-line Definition

An architecture that models reality through synthetic data generation
and transforms data into a reliable and explainable system state

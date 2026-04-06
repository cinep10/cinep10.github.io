# Data Reliability

This portfolio presents a **production-oriented Data Reliability System** built from an engineering perspective.

The goal is not limited to anomaly detection or data validation.  
Instead, the system is designed to transform data signals into **operational decisions** through a structured pipeline:

```text
Data → Signal → Risk → Cause → Action
```

---

## Pipeline Overview

The system follows an end-to-end data reliability pipeline:

```text
Raw Data
→ Metric
→ Semantic Mapping
→ Validation
→ Drift / Structural Anomaly
→ Risk Score
→ Root Cause & Action
→ Observability
```

Each layer is independently implemented, yet tightly connected to form a **decision-ready system**.

---

## 1. Data → Metric Layer

Transforms raw data into structured, analyzable metrics.

- [Data Layer: From Raw Logs to Reliable Data](/portfolio/data-reliability/data-layer)
- [Metric Layer](/portfolio/data-reliability/metric-layer)

Key responsibilities:

- Data ingestion and normalization
- Time-series metric construction
- Domain-specific metric definition (traffic, auth, funnel, etc.)

---

## 2. Semantic Mapping Layer

Converts raw data into meaningful business events.

- [Semantic Mapping Layer](/portfolio/data-reliability/semantic-mapping-layer)

Key responsibilities:

- Mapping raw events/URLs into semantic events
- Defining funnel stages (view → apply → submit)
- Ensuring interpretability of data

---

## 3. Validation Layer

Verifies the correctness of the data itself.

- [Validation Layer (Technical Deep Dive)](/portfolio/data-reliability/validation-layer)

This layer answers:

- Is the data present? (missing / null)
- Is the value valid? (range / rule)
- Is it logically consistent? (funnel / constraint)

In short:

Validation determines whether **the data is wrong**.

---

## 4. Drift / Structural Anomaly Layer

Detects whether data behavior has changed compared to baseline.

- [Drift Layer](/portfolio/data-reliability/drift-layer)

Key functions:

- Drift detection (distribution change, z-score, PSI)
- Time-series anomaly detection (pattern deviation)
- Correlation anomaly detection (relationship break)

In short:

This layer determines whether **the data behaves differently**.

---

## 5. Risk Score Layer

Aggregates multiple signals into a single operational metric.

- [Risk Layer](/portfolio/data-reliability/risk-layer)

Key functions:

- Integrates validation, drift, anomaly, and mapping signals
- Produces a day-level risk score
- Assigns risk grade (normal / warning / alert)

In short:

This layer answers:

**"How risky is today's data state?"**

---

## 6. Root Cause & Action Layer

Transforms risk signals into explainable causes and operational actions.

- [Root Cause Layer](/portfolio/data-reliability/root-cause-layer)

This layer combines two critical functions:

### Root Cause Analysis

- Decomposes risk into contributing signals
- Maps signals into structured cause types
- Links causes to specific metrics
- Ranks causes based on contribution

Examples of cause types:

- funnel_distortion
- traffic_mix_shift
- mapping_issue
- pipeline_issue
- metric_drift

This answers:

**"Why did the risk increase?"**

---

### Action Generation

- Maps cause types into action types
- Generates metric-specific action items
- Assigns priority based on risk and contribution
- Produces operational recommendations

Examples:

- Check submit_capture_rate
- Check daily_active_users
- Verify mapping coverage
- Inspect upstream pipeline

This answers:

**"What should be done?"**

---

### Layer Significance

- Risk Score → defines problem magnitude
- Root Cause → explains the reason
- Action → defines response

This layer transforms:

**signals → decisions → actions**

---

## 7. Observability (Grafana)

Provides real-time visibility into system behavior.

- [Explainable Data Behavior](/portfolio/data-reliability/explainable-data-behavior)

The system integrates with Grafana dashboards to monitor:

- Validation fail/warn trends
- Drift and anomaly alerts
- Risk score trends
- Top failing metrics
- Root cause and action summaries

This enables:

- Fast detection
- Quick diagnosis
- Immediate action

---

## ML / AI Extension

ML and AI are built on top of the data reliability pipeline.

- ML: risk classification (normal / warning / alert)
- AI: incident reasoning and action recommendation

See:

- [/portfolio/ml-data-reliability](/portfolio/ml-data-reliability)

---

## System Characteristics

This system is defined by:

- End-to-end pipeline from data to action
- Explainability-first design
- Combination of rule-based and statistical methods
- Operationally deployable architecture
- ML/AI extensibility

---

## Key Concept

Data Reliability is not just validation.

It is a system that connects:

- Data quality verification
- Behavioral change detection
- Structural anomaly analysis
- Risk aggregation
- Root cause explanation
- Action recommendation

---

## One-line Definition

Data Reliability =  
A system that transforms data signals into operational decisions

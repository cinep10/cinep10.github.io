# Architecture

This section documents both the high-level architecture
and the detailed design of each layer.

The goal is to explain not only how the system is structured,
but also why it was designed in this way.

---

## 1. Architecture

This part focuses on system-level design and conceptual frameworks.

It answers:

* Why this architecture exists
* What problems it solves
* How each component is connected

---

### Core Architecture

* [Data Reliability Architecture](/architecture/data-reliability-architecture)
* [Analytics Pipeline Validation Framework](/architecture/analytics-pipeline-validation-framework)
* [Drift Detection Design](/architecture/drift-detection-design)
* [Risk Scoring Design](/architecture/risk-scoring-design)
* [Data Decision Framework](/architecture/data-decision-framework)

---

## 2. Analysis and Design

This part focuses on how each layer is designed and implemented.

It answers:

* How raw data is transformed
* How metrics are defined
* How reliability is evaluated
* How changes and risks are interpreted

---

### Layer Design

* [Data Layer](/portfolio/data-reliability/data-layer)
* [Metric Layer](/portfolio/data-reliability/metric-layer)
* [Validation Layer](/portfolio/data-reliability/validation-layer)
* [Drift Layer](/portfolio/data-reliability/drift-layer)
* [Risk Scoring](/architecture/risk-scoring-design)
* [Root Cause Analysis](/portfolio/data-reliability/root-cause-layer)

---

## 3. Relationship Between Architecture and Design

The system is structured as a continuous flow:

```text
Data
→ Metric
→ Validation
→ Drift
→ Risk
→ Root Cause
```

---

Architecture defines the structure.

Design defines how each component works in practice.

---

## 4. Perspective

This is not a collection of independent components.

It is a connected system that:

* validates data
* detects change
* evaluates impact
* explains behavior

---

The focus is not on building pipelines,
but on building systems that make data reliable and explainable.

---

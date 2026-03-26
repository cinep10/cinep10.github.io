# Engineering Notes

This page documents how data systems are built, validated, and made reliable.

The focus is not only on building pipelines,  
but on answering a more fundamental question:

> Can this data be trusted?

---

## Overview

The content is structured as a reliability flow:

```text
Data → Metric → Validation → Drift → Risk → Decision
```

Each note represents a component of this system,  
based on real-world implementation and operational experience.

---

## Core Framework

The following documents define how data is interpreted and controlled.

- [Data Reliability Architecture](./data-reliability-architecture)  
  Overall structure for managing data correctness, change, and impact.

- [Data Risk Scoring Architecture](./data-risk-scoring)  
  Method for integrating multiple signals into a unified risk score.

- [Data Decision Framework](./data-decision-framework)  
  Criteria for determining whether data is acceptable or requires action.

---

## Data Reliability (Engineering)

This section focuses on how data is generated, validated, and monitored.

- [Data Layer](./data-reliability/data-layer)  
  Raw log generation, parsing, normalization, and loading.

- [Analytics Pipeline Validation Framework](./data-reliability/analytics-pipeline-validation-framework)  
  Validation rules for ensuring data correctness.

- [Web Log Session ID Data Consistency](./data-reliability/web-log-session-id-data-consistency)  
  Case study on data consistency issues.

→ [Go to Data Reliability](./data-reliability/data-reliability)

---

## Data Platform

This section covers how data pipelines and metrics are designed.

- (coming soon) Metric Layer  
- (coming soon) ETL Architecture  

---

## Observability

This section focuses on monitoring and visualizing data systems.

- [Grafana Dashboard Image Capture](./grafana-dashboard-image-capture)  
  Automating dashboard rendering and reporting.

→ [Go to Data Platform](./data-reliability/data-reliability)

---

## Incident & Recovery

This section covers data-related issues and recovery strategies.

- (coming soon) Data Incident Case  

→ [Go to Incident & Recovery](./data-reliability/data-reliability)

---

## Key Idea

A system is not reliable because it runs successfully.

It is reliable when its data can be trusted.
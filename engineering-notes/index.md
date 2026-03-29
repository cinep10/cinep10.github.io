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

- [Data Reliability Architecture](/architecture/data-reliability-architecture)   
  Overall structure for managing data correctness, change, and impact.

- [Data Risk Scoring Architecture](/architecture/data-risk-scoring-architecture)   
  Method for integrating multiple signals into a unified risk score.

- [Data Decision Framework](/architecture/data-decision-framework)   
  Criteria for determining whether data is acceptable or requires action.

---

## Data Reliability

This section focuses on how data is generated, validated, and monitored.

- [Data Reliability Approach](./data-reliability/data-reliability-approach)  
  Framework for understanding data trust.


→ [Go to Data Reliability](/engineering-notes/data-reliability/)

---

## Data Platform

This section covers how data pipelines and metrics are designed.

- (coming soon) Metric Layer  
- (coming soon) ETL Architecture  

→ [Go to Data Platform](/engineering-notes/data-platform/)

---

## Observability

This section focuses on monitoring and visualizing data systems.

- [Grafana Dashboard Image Capture](./observability/grafana-dashboard-image-capture)  
  Automating dashboard rendering and reporting.

→ [Go to Observability](/engineering-notes/observability/)

---

## Incident & Recovery

This section covers data-related issues and recovery strategies.

- [WSL ext4 Recovery Case Study](./incident/wsl-ext4-recovery)

→ [Go to Incident & Recovery](/engineering-notes/incident/)

---

## Key Idea

A system is not reliable because it runs successfully.

It is reliable when its data can be trusted.

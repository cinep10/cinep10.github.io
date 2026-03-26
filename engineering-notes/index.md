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

- [Data Reliability Architecture](/portfolio/data-reliability/data-reliability-architecture)
  Overall structure for managing data correctness, change, and impact.

- (coming soon) Data Risk Scoring Architecture
  Method for integrating multiple signals into a unified risk score.

- (coming soon) Data Decision Framework
  Criteria for determining whether data is acceptable or requires action.

---

## Data Reliability (Engineering)

This section focuses on how data is generated, validated, and monitored.

- [Data Reliability Approach](./data-reliability/data-reliability-approach)  
  Framework for understanding data trust.

- [Verifying Page View Consistency from Web Access Logs](./data-reliability/verifying-page-view-consistency-from-web-access-logs.md)
  Real-world consistency issue.


→ [Go to Data Reliability (Engineering)](./data-reliability/data-reliability)

---

## Data Platform

This section covers how data pipelines and metrics are designed.

- (coming soon) Metric Layer  
- (coming soon) ETL Architecture  

→ [Go to Data Platform](./data-platform/data-platform)

---

## Observability

This section focuses on monitoring and visualizing data systems.

- [Grafana Dashboard Image Capture](./observability/grafana-dashboard-image-capture)  
  Automating dashboard rendering and reporting.

→ [Go to Observability](./observability/observability)

---

## Incident & Recovery

This section covers data-related issues and recovery strategies.

- [WSL ext4 Recovery Case Study](./incident/wsl-ext4-recovery)

→ [Go to Incident & Recovery](./incident/incident)

---

## Key Idea

A system is not reliable because it runs successfully.

It is reliable when its data can be trusted.

# Engineering Notes

This page documents how to build data systems that are not only functional, but trustworthy.

The focus is not just on pipelines,  
but on answering a more important question:

> Can this data be trusted?

---

## Overview

The content is structured around a single flow:

Data → Metric → Validation → Drift → Risk → Dashboard


---

## Featured Notes (Start Here)

### Data Reliability
- [Data Reliability Approach](./data-reliability/data-reliability-approach)
- [Analytics Pipeline Validation Framework](./data-reliability/analytics-pipeline-validation-framework)
- [Web Log Session ID Data Consistency](./data-reliability/web-log-session-id-data-consistency)

### Data Platform
- [Data Layer: From Raw Logs to Reliable Data](./data-platform/data-layer)

### Observability
- [Grafana Dashboard Image Capture Automation](./observability/grafana-dashboard-image-capture)

### Incident & Recovery
- [WSL ext4 Recovery Case Study](./incident/wsl-ext4-recovery)

---

## 1. Data Reliability

Ensuring data is correct, consistent, and interpretable.

- [Data Reliability Approach](./data-reliability/data-reliability-approach)
- [Analytics Pipeline Validation Framework](./data-reliability/analytics-pipeline-validation-framework)
- [Web Log Session ID Data Consistency](./data-reliability/web-log-session-id-data-consistency)

→ [See all Data Reliability notes](./data-reliability/data-reliability)

---

## 2. Data Risk

Turning data signals into decisions.

- (coming soon) Risk Scoring Framework
- (coming soon) Decision Framework (OK / WARN / FAIL)

→ [See all Data Risk notes](./data-risk/data-risk)

---

## 3. Data Platform

Building data pipelines and metric systems.

- [Data Layer: From Raw Logs to Reliable Data](./data-platform/data-layer)

→ [See all Data Platform notes](./data-platform/data-platform)

---

## 4. Observability

Making systems visible and explainable.

- [Grafana Dashboard Image Capture Automation](./observability/grafana-dashboard-image-capture)

→ [See all Observability notes](./observability/observability)

---

## 5. Incident & Recovery

Understanding and resolving data issues.

- [WSL ext4 Recovery Case Study](./incident/wsl-ext4-recovery)

→ [See all Incident notes](./incident/incident)

---

## Key Idea

A system is not reliable because it runs successfully.

It is reliable when its data can be trusted.

---

## One-line Summary

This is not a collection of notes.

It is a record of building a data reliability system.

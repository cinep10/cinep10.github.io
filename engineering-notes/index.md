# Engineering Notes

This page documents how data systems are built, validated, and made reliable.

The focus is not just on building pipelines,  
but on answering a more important question:

> Can this data be trusted?

---

## Overview

The content is organized around a single flow:

```text
Data → Metric → Validation → Drift → Risk → Dashboard
```

Each note represents a part of this flow,  
based on real-world implementation and operational experience.

---

## 1. Data Reliability

Ensuring that data is correct, consistent, and usable.

- [Data Reliability Approach](./data-reliability-approach)
- [Web Log Session ID Data Consistency](./web-log-session-id-data-consistency)

---

## 2. Validation & Data Quality

Defining how data correctness is verified.

- [Verifying Page View Consistency from Web Access Logs](./verifying-page-view-consistency-from-web-access-logs)

---

## 3. Drift & Change Detection

Detecting when data changes in unexpected ways.

- (coming soon) Drift Detection Design

---

## 4. Risk & Decision

Turning data signals into actionable decisions.

- (coming soon) Risk Scoring Framework  
- (coming soon) OK / WARN / FAIL Decision Model  

---

## 5. Pipeline & Platform

How data is collected, processed, and structured.

- (coming soon) Data Pipeline Architecture  
- (coming soon) Metric Design  

---

## 6. Observability

Making data systems visible and interpretable.

- [Grafana Dashboard Image Capture Automation](./grafana-dashboard-image-capture)

---

## 7. Incident & Recovery

Understanding and resolving data issues.

- [WSL ext4 Recovery Case Study](./web-log-session-id-data-consistency)

---

## Key Idea

A system is not reliable because it runs successfully.

It is reliable when its data can be trusted.

---

## One-line Summary

This is not a collection of engineering notes.

It is a record of building a data reliability system.


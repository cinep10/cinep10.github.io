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

Defining how data correctness is verified.
Detecting when data changes in unexpected ways.
Ensuring that data is correct, consistent, and usable.

- [Data Reliability Approach](./data-reliability-approach)
- [Verifying Page View Consistency from Web Access Logs](./verifying-page-view-consistency-from-web-access-logs)
- [Web Log Session ID Data Consistency](./web-log-session-id-data-consistency)

---

## 2. Risk & Decision

Turning data signals into actionable decisions.

- (coming soon) Risk Scoring Framework  
- (coming soon) OK / WARN / FAIL Decision Model  

---

## 3. Pipeline & Platform

How data is collected, processed, and structured.

- [Data Layer](./data-layer)

---

## 4. Observability

Making data systems visible and interpretable.

- [Grafana Dashboard Image Capture Automation](./automating-grafana-dashboard-image-capture)

---

## 5. Incident & Recovery

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


# Portfolio

This section introduces architectural ideas and framework-oriented work related to **data reliability and data risk control**.

The designs focus on ensuring that operational data remains trustworthy across the entire lifecycle — from event collection to analytics and decision-making.

---

## Portfolio Overview

- [Statistical Data Drift Framework](./drift-framework)
- [Data Reliability Architecture](./data-reliability-architecture)
- [Digital Channel Data Validation](./digital-channel-data-validation)

---

## Focus

This portfolio is built around three themes:

### 1. Statistical Stability

Detecting abnormal changes in operational data such as:

- distribution shift
- sudden metric deviation
- null ratio spikes
- event volume anomalies

### 2. Reliability Architecture

Designing validation and monitoring layers across:

- event collection
- pipeline processing
- warehouse loading
- metric generation

### 3. Validation Across Systems

Comparing metrics across:

- client events
- server logs
- collectors
- analytics engines

The goal is to reduce interpretation risk and improve trust in operational data.
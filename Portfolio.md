# Portfolio

This section describes architectural ideas and experimental frameworks related to **data reliability and data risk control**.

---

## Statistical Data Drift Framework

A statistical monitoring framework designed to detect abnormal changes in operational data.

The framework focuses on identifying issues such as:

- distribution shift
- sudden metric deviation
- null ratio spikes
- event volume anomalies

Typical methods include:

- PSI (Population Stability Index)
- ratio monitoring
- anomaly band detection
- statistical baseline comparison

The goal is to detect hidden failures in data pipelines before they affect business decisions.

---

## Data Reliability Architecture

A design approach that ensures operational data remains trustworthy across distributed systems.

Typical architecture:

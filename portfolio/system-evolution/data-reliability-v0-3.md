# Data Reliability Platform – v0.3

## Integrating Data Quality, Performance, and Availability into a Unified Risk Architecture

---

# 1. Overview

Problems in data pipelines generally appear in three forms:

- Incorrect data (Data Quality Issue)
- Delayed data (Performance Issue)
- Interrupted data flow (Availability Issue)

Traditional systems usually manage these separately:

- Batch validation
- Stream monitoring
- Infrastructure monitoring

However, in real production environments, these issues occur simultaneously within the same data flow.

---

## Core Question

```text
Is this a data quality issue?
A performance issue?
Or an availability issue?
```

v0.3 was designed to answer this question through a unified architecture.

---

# 2. v0.3 Goal

The primary objective of v0.3 is straightforward:

> Integrate data anomalies, delays, and interruptions into a single operational risk model.

---

## Target Architecture

```text
Source
→ Canonical Event
→ Batch / Stream
→ Data / Stream / Performance / Availability Risk
→ Integrated Risk
→ Root Cause
→ ML / AI
→ Operational Execution
```

---

# 3. Core Architecture Design

---

## 3.1 Canonical Event Layer

The most important architectural decision in v0.3 is:

```text
Canonical Event = Single Source of Truth
```

---

## Why This Was Necessary

v0.2 exposed several structural problems:

- Different reference datasets between batch and stream paths
- Duplicated source parsing logic
- Weak lineage tracking

---

## Solution

```text
event_log_raw
→ canonical_events
→ downstream systems
```

---

## Role of the Canonical Layer

- Unified reference point for all pipelines
- Branching point before batch and stream separation
- Foundation for data consistency

---

## 3.2 Branch Architecture

After canonicalization, data flows into two execution paths.

```text
canonical_events
  ├── batch path
  └── stream path
```

---

### Batch Path

```text
canonical_events
→ stg_event_batch
→ analyzer
→ validation / mapping / drift
```

---

### Stream Path

```text
event_log_raw
→ Kafka Producer
→ Kafka Consumer
→ stream reliability layer
→ stream risk signal
```

---

# 3.3 Risk Layer Expansion

The main architectural evolution in v0.3 is the expansion of the risk layer.

---

## v0.2

```text
data risk
stream risk
```

---

## v0.3

```text
data risk
stream risk
performance risk
availability risk
operational scenario
```

---

## Key Principle

v0.3 does not use metrics directly.

Instead:

```text
metric → interpretable operational risk
```

---

# 3.4 Integrated Risk Structure

```text
risk_layer_signal_day
risk_layer_score_day
risk_layer_context_day
```

---

## Responsibilities

- Aggregate multiple operational signals
- Provide inputs for Root Cause and AI layers
- Support operational decision-making

---

# 4. Scenario-driven Testing

v0.3 introduces scenario-based operational testing.

---

## 4.1 Stream Scenarios

Examples:

- Traffic drop with latency increase
- Partial missing events
- Traffic spike

---

## 4.2 Operational Scenarios

Examples:

- Lag spike
- No-data interval
- Consumer delay
- Backlog accumulation

---

## Design Principle

```text
source scenario
stream scenario
operational scenario
```

Each scenario operates at a different layer and can reproduce compound operational failures.

---

# 5. Detection Layer

---

## 5.1 Lag Spike Detection

### Problem

Lag spikes existed but were not detected correctly.

### Root Cause

Incorrect interarrival-based measurements.

---

## Solution

```text
processing_lag_ms = replayed_at - event_time
```

---

## Result

- Actual processing delay became measurable
- Scenario results aligned with detection outputs

---

## 5.2 No-data Detection

```text
event_time gap detection
```

---

## Result

Accurate identification of interrupted data intervals.

---

## 5.3 Performance Risk

Main indicators:

- Freshness delay
- Consumer lag
- Throughput reduction
- Backlog accumulation

---

## 5.4 Availability Risk

Main indicators:

- No-data interval
- Downtime
- Runtime status

---

# 6. Interpretation Layer

---

## 6.1 Lag Normalization

### Problem

Lag values increased excessively under replay conditions.

---

## Solution

```text
lag_adj_ms = actual_elapsed - expected_elapsed
```

---

## 6.2 Threshold Calibration

```text
warn = p95
fail = p99
critical = p99 + 2σ
```

---

## 6.3 Root Cause Mapping

Examples:

```text
lag_spike → consumer_delay
no_data → canonical_gap
traffic_spike → load_increase
```

---

## 6.4 Integrated Risk Score

```text
score =
  max * 0.5 +
  avg * 0.3 +
  persistence * 0.2
```

---

## 6.5 ML / AI Integration

### ML

- Risk classification
- Risk prediction

### AI

- Incident summary
- Action recommendation

---

# 7. Execution Layer

---

## Operational Problems

- Weak execution tracking
- Low reproducibility
- Limited operational automation

---

## Solutions

---

### Experiment Registry

- Execution history management
- Pipeline execution logging

---

### Scenario Manifest

```text
Deterministic execution guarantee
```

---

### Unified Runner

```bash
run_v03_full_backfill.sh
```

---

## Execution Flow

```text
reset
→ scenario definition
→ pipeline execution
→ result validation
→ experiment tracking
```

---

# 8. v0.3 Achievements

---

## Architecture

```text
source
→ canonical
→ batch / stream
→ risk
→ root cause
→ ML / AI
```

---

## Functional Capabilities

- Anomaly detection
- Lag detection
- No-data detection
- Risk scoring
- Root cause analysis
- AI-based explanation

---

## Operational Capabilities

- Reproducible execution
- Scenario-based testing
- Experiment traceability

---

# 9. Core Design Principles

---

## 1. Canonical First

```text
All downstream processing begins after canonicalization.
```

---

## 2. Risk as Interpretation

```text
Metrics become meaningful only after risk interpretation.
```

---

## 3. Scenario-driven Testing

```text
Operational failures must be simulated intentionally.
```

---

## 4. Deterministic Execution

```text
Same input → same result
```

---

# 10. Direction for v0.4

If v0.3 focused on building a functioning system,
v0.4 focuses on building an operationally sustainable system.

---

## Planned Expansion

- Advanced source simulation
- Experiment governance
- Replay / resume capability
- ML / AI enhancement
- Monitoring and alert integration

---

# Conclusion

v0.3 connects data reliability problems through the following operational chain:

```text
Detection
→ Interpretation
→ Explanation
→ Execution
```

The most important architectural shift is this:

> Data quality issues are no longer treated as isolated anomalies,
> but as operational risk signals within a unified reliability architecture.

---

# Final Summary

v0.3 establishes a unified architecture that interprets
data anomalies, delays, and interruptions as operational risk.

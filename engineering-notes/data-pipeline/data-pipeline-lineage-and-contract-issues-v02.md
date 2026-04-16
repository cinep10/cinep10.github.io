# Data Pipeline — Lineage, Contract, and Stream/Batch Integration Issues (v0.2)

## Overview

In v0.2, the data pipeline evolved from a batch-centric architecture (v0.1)
to a hybrid system incorporating Kafka-based stream processing.

While this enabled real-time data ingestion and anomaly detection,
a set of structural issues emerged that were not isolated bugs,
but systemic problems across the pipeline.

These issues can be grouped into four key areas:

- Data lineage limitations  
- Stream/Batch path fragmentation  
- Scenario injection strategy  
- Lack of contract standardization  

This document analyzes these issues from an engineering perspective,
including observed symptoms, technical causes, interim fixes, and design directions.

---

## 1. Source Lineage and Snapshot Management

### Symptoms

- The same raw log files were reused across multiple runs  
- Results were not reproducible even for the same date  
- `metric_value_day` outputs were inconsistent  

### Technical Issue

- Source data was managed at the file level only  
- No concept of date-based snapshot or versioning  
- Load lineage was not explicitly tracked  

### Why It Happened in v0.2

- Focus on rapid scenario testing  
- Reuse of raw log files for convenience  
- No explicit source versioning strategy  

### Interim Fix in v0.2

- Manual separation of files for certain runs  
- Partial lineage tracking through execution logs  

→ Not a fundamental solution  

### Direction for v0.3

Introduce explicit source snapshot management:

```

raw_log (dt=2025-04-01, version=1)
raw_log (dt=2025-04-01, version=2)

```

- Versioned source control  
- Explicit lineage tracking  
- Reproducible execution units  

---

## 2. Stream Path Fragmentation

### Symptoms

Stream testing was performed through multiple paths:

```

(1) Kafka-based stream path
(2) DB-based post-processing path

```

- Same anomaly produced different results depending on path  

### Technical Issue

- No single authoritative stream pipeline  
- Runtime path and test path diverged  
- Semantic inconsistency between ingestion paths  

### Why It Happened in v0.2

- Kafka environment instability  
- Need for fast iteration and testing  
- Allowing DB-based fallback paths  

### Interim Fix in v0.2

- Mixed usage of Kafka and DB-based execution  
- Ad-hoc comparison between results  

→ No consistent baseline  

### Direction for v0.3

Standardize the stream runtime path:

```

event_log_raw → Kafka → consumer → stg_event_stream

```

- Enforce a single runtime path  
- Separate test logic from runtime pipeline  

---

## 3. Scenario Injection Strategy

### Symptoms

- Scenario injection was performed at the DB level  
- Testing was not based on raw source data  

### Technical Issue

- Source truth and test data diverged  
- Injection location affected downstream results  
- Inconsistent behavior between batch and stream  

### Why It Happened in v0.2

- High complexity of raw log injection  
- Difficulty synchronizing batch and stream  
- Need for stable and repeatable testing  

### Interim Fix in v0.2

Scenario injection applied after canonicalization:

```

event_log_raw → (DB-level injection) → downstream

```

- Improved reproducibility  
- Simplified testing workflow  

### Direction for v0.3

Introduce dual injection strategies:

```

(1) Source-stage injection
(2) Post-load injection

```

---

## 4. Canonicalization and Branching Structure

### Symptoms

- Inconsistent results between batch and stream processing  
- Same event interpreted differently across pipelines  

### Technical Issue

- Canonical layer defined but not strictly enforced  
- Normalization not guaranteed before branching  
- Downstream transformations altered event semantics  

### Direction for v0.3

Enforce canonical processing before branching:

```

raw → canonical → branch (batch / stream)

```

---

## 5. Contract Standardization Issues

### Symptoms

- Kafka wrapper parameter mismatches  
- CLI inconsistencies  
- Threshold ambiguity  
- Grafana query misalignment  
- Incorrect join conditions  

### Technical Issue

- No defined contract between layers  
- Schema and semantics not standardized  
- Weak alignment between pipeline and observability  

### Direction for v0.3

Standardize:

- CLI interfaces  
- Threshold schemas  
- Metric definitions  
- Dashboard queries  
- Join conditions  

---

## Key Takeaway

The issues in v0.2 were not isolated bugs,
but structural problems in lineage, processing paths,
injection strategy, and contract design.

---

## One-line Summary

v0.2 introduced stream processing, but inconsistencies in lineage,
contract, and runtime paths led to pipeline integrity issues.
```

---


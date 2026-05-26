# Architecture

This section describes the overall data reliability architecture of the current v0.5 portfolio.

The architecture is not designed as a simple ETL pipeline or anomaly detector.

Instead, it focuses on measuring inconsistency and distortion across:

```text
Behavior
↔ Transaction
↔ State
```

and connecting:

```text
Measurement
→ Reliability Analytics
→ Semantic Interpretation
→ Unified Risk
→ Operational Action
→ ML / AI Governance
```

into a:

```text
Cross-Domain Measurement-to-Decision Reliability Architecture
```

The core question is:

```text
When does operational data become unreliable,
and how does that distortion affect KPI interpretation
and operational decision-making?
```

---

# Overall Architecture Flow

```text
Customer Journey
→ Data Generation
→ Ingestion
→ Transformation
→ Analysis
→ Decision / Action
→ ML / AI Governance
→ Observability
```

---

# 1. Customer Journey

The architecture begins not from isolated events,
but from:

```text
realistic user behavioral flows
```

Core structure:

```text
Customer Journey
├─ Behavior Log
├─ Transaction Log
└─ State Transition Log
```

Important principle:

```text
Journey is the source
Behavior / Transaction / State
are parallel derivations
```

→ [View Customer Journey](./customer-journey)

---

# 2. Data Generation

Operational logs are generated from the customer journey layer.

Generated sources include:

* Behavior Log
* Transaction Log
* State Transition Log

The architecture also supports source-level anomaly injection.

Examples:

* source_partial_missing
* source_identity_drift
* source_schema_drift
* transaction_missing_anomaly

The objective is:

```text
realistic operational data distortion generation
```

→ [View Data Generation](/architecture/data-generation/)

---

# 3. Ingestion

Generated source logs are ingested into
the raw/stage pipeline layers.

Core ingestion flow:

```text
raw_snapshot_manifest
→ stg_webserver_log_hit
→ stg_wc_log_hit
→ event_log_raw
```

Transaction and state logs are stored in separate raw layers.

Key objectives:

```text
preserve source lineage
+
maintain source-first propagation
```

→ [View Ingestion](/architecture/ingestion/)

---

# 4. Transformation

Raw data is transformed into
canonical business structures.

Core structure:

```text
event_log_raw
→ canonical_behavior_events

transaction_log_raw
→ canonical_transaction_events

state_log_raw
→ canonical_state_events
```

Cross-domain mappings are then created across:

```text
Behavior ↔ Transaction
Transaction ↔ State
```

Important principle:

```text
canonical = business normalization
canonical ≠ risk scoring
```

→ [View Transformation](/architecture/transformation/)

---

# 5. Analysis

This is the core reliability analysis layer of v0.5.

Representative measurements:

* behavior_transaction_match_rate
* transaction_state_match_rate
* payment_order_gap
* duplicate_order_count
* delivery_delay

Core philosophy:

```text
Measurement ≠ Risk
```

Meaning:

```text
Measurement = observation
Risk = semantic interpretation
```

The architecture prioritizes:

```text
DIRECT_MEASUREMENT-based reliability analysis
```

over scenario-name heuristics.

→ [View Analysis](/architecture/analysis/)

---

# 6. Decision / Action

This is the primary differentiating layer of v0.5.

Current authoritative chain:

```text
Measurement
→ Reliability Analytics
→ Semantic Interpretation
→ Unified Risk
→ Operational Action
```

The key objective is to connect:

```text
technical anomalies
→ operational meaning distortion
→ operational decision risk
```

→ [View Decision / Action](/architecture/decision-action/)

---

# 7. ML / AI Governance

ML is not treated as the final decision-maker.

Current philosophy:

```text
ML = supplemental prediction only
```

AI is also not designed as a free-generation layer.

Instead:

```text
AI = evidence-constrained explanation
```

Core principle:

```text
AI generates truth = X
AI explains governed evidence = O
```

→ [View ML / AI Governance](/architecture/ml-ai-governance/)

---

# 8. Observability

Finally, measurement results,
semantic risks,
and operational actions are exposed
through operational observability.

The objective is not simply dashboard monitoring,
but:

```text
Reliability Decision Observability
```

The focus is:

```text
Why was the KPI distorted?
Why did operational decision-making become risky?
```

→ [View Observability](/architecture/observability/)

---

# Final Direction

The current direction of v0.5 is:

```text
Behavior Signal
+
Transaction Signal
+
State Signal
→ Reliability
→ Operational Risk
→ Operational Decision
```

The architecture therefore evolves beyond anomaly detection into:

```text
Operational Reliability Decision Architecture
```

that explains:

```text
how operational data distortion
contaminates KPI interpretation
and operational decision-making
```

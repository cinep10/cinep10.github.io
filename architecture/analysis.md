# Measurement / Analytics Architecture

The current measurement and analytics layer is not designed as a simple anomaly detection system.

Instead, the architecture focuses on:

```text
Behavior Evidence Measurement
+
Behavior ↔ Transaction ↔ State Reconciliation Measurement
+
Runtime Evidence Integration
+
Measurement-driven Reliability Analytics
```

The core question is not:

```text
"What scenario was executed?"
```

but rather:

```text
"What operational inconsistency was actually measured,
and how did it propagate across layers?"
```

v0.4 measures the instability of behavioral data itself as operational evidence, while v0.5 uses Behavior / Transaction / State consistency as authoritative measurement. 

---

# Measurement / Analytics Architecture Overview

![Analysis](/assets/images/measurement-analytics-architecture.png)

---

# Measurement Layer

The measurement layer is divided into two major areas:

```text
Behavior Evidence Measurement
Reconciliation Measurement
```

Behavior Evidence Measurement evaluates the quality, stability, distribution, and runtime characteristics of behavioral data itself.

Reconciliation Measurement evaluates inconsistencies between:

```text
Behavior
↔ Transaction
↔ State
```

---

# Behavior Evidence Measurement

Behavior Evidence Measurement evaluates how behavioral data becomes unstable during collection, transformation, and runtime execution.

This layer does not produce authoritative business judgment.

Its role is:

```text
Operational evidence generation
for behavioral data quality and runtime stability
```

---

# Batch Measurement

Batch Measurement evaluates whether daily behavioral data maintains stable volume, distribution, session structure, and conversion structure.

Overall flow:

```text
canonical_events
→ stg_event_batch
→ analyzer_b_v5_v04.py
→ batch_behavior_measurement_day
→ batch_behavior_distribution_day
```

---

# analyzer_b_v5_v04.py

`analyzer_b_v5_v04.py` is the core batch measurement analyzer.

It converts `stg_event_batch` into operational behavioral measurements.

Primary responsibilities:

```text
identity resolution
sessionization
pageview normalization
conversion measurement
visitor/session aggregation
```

Identity stitching priority:

```text
uid
→ pcid
→ ip
```

Session boundaries are created using inactivity thresholds, while pageviews are normalized using:

```text
view_only
```

logic to prevent system events or click events from inflating behavioral metrics.

Representative measurements:

```text
pageview_count
visit_count
visitor_count
session_count
conversion_count
avg_pageviews_per_visit
avg_session_duration
repeat_visit_count
```

Batch Measurement therefore evaluates:

```text
Is the behavioral data operationally stable and analytically trustworthy?
```

rather than simple row counts. 

---

# Batch Distribution Measurement

Batch Distribution Measurement evaluates distributional shifts in behavioral data.

Representative dimensions:

```text
event distribution
page distribution
journey stage distribution
campaign distribution
coupon distribution
traffic actor distribution
```

The goal is not merely detecting volume reduction.

Instead, the system asks:

```text
Which segment, funnel, page,
or campaign became distorted?
```

For example:

```text
Stable overall traffic
+
decreased checkout/payment events
```

may indicate funnel distortion rather than generic traffic loss.

---

# Stream Measurement

Stream Measurement evaluates the stability of real-time or near-real-time event flows.

Overall flow:

```text
canonical_events
→ stg_event_stream
→ stream_reliability_summary_day
```

When Kafka is disabled:

```text
canonical_events
→ stream_replay_event
→ stg_event_stream
```

When Kafka is enabled:

```text
canonical_events
→ producer
→ consumer
→ stg_event_stream
```

Representative measurements:

```text
stream completeness
duplicate rate
latency
ordering quality
producer / consumer parity
```

The objective is:

```text
Can events flow through the stream layer
without loss, duplication, delay,
or ordering distortion?
```

---

# Operational Measurement

Operational Measurement evaluates pipeline execution quality rather than the data itself.

Representative measurements:

```text
pipeline availability
runtime delay
processing completeness
input/output count
fallback status
```

Key questions:

```text
Is pipeline execution stable?
Was batch input generated correctly?
Did replay execution complete successfully?
Did runtime latency accumulate?
```

This layer separates:

```text
data quality
+
execution quality
```

---

# Realism Measurement

Realism Measurement evaluates whether generated customer journeys resemble realistic operational behavior.

Representative measurements:

```text
pageviews per visit
session duration
repeat visitor ratio
anonymous/authenticated ratio
sid/ip/user-agent stability
non-linear navigation rate
```

This layer is critical because:

```text
If source behavior itself is unrealistic,
reconciliation measurement loses operational meaning.
```

Realism Measurement therefore protects downstream analytical validity.

---

# Runtime Evidence Integration

`v05_runtime_evidence_day` is not a simple intermediate table.

Its role is:

```text
Integrating v0.4 behavioral evidence
into runtime evidence
usable by v0.5 analytics
```

Inputs include:

```text
batch evidence
stream evidence
operational evidence
realism evidence
```

Representative scores:

```text
runtime_evidence_score
batch_evidence_score
stream_evidence_score
operational_evidence_score
realism_evidence_score
dominant_runtime_signal
```

Core principle:

```text
runtime evidence
=
supplementary evidence

reconciliation measurement
=
authoritative measurement
```

v0.4 explains instability in behavioral evidence itself, while v0.5 explains authoritative business inconsistency between Behavior / Transaction / State. 

---

# Reconciliation Measurement

Reconciliation Measurement is the core measurement layer in v0.5.

Measurements are performed from three perspectives:

```text
Behavior ↔ Transaction
Transaction ↔ State
State Timeliness / Orphan State
```

---

# Behavior ↔ Transaction Measurement

This measurement evaluates whether customer behavior actually resulted in business transactions.

Inputs:

```text
canonical_behavior_events
canonical_transaction_events
```

Representative keys:

```text
journey_id
order_id
payment_id
cart_id
coupon_id
uid
```

Representative measurements:

```text
behavior_transaction_match_rate
behavior_only_count
transaction_only_count
```

Interpretation:

```text
behavior_only_count increase
→ behavior exists without transaction evidence

transaction_only_count increase
→ transaction exists without behavioral evidence
```

This layer evaluates:

```text
Can behavior and transaction
mutually explain each other?
```

---

# Transaction ↔ State Measurement

This measurement evaluates whether transaction state transitions occurred correctly.

Inputs:

```text
canonical_transaction_events
canonical_state_events
```

Representative keys:

```text
transaction_id
order_id
payment_id
delivery_id
refund_id
journey_id
```

Representative measurements:

```text
transaction_state_match_rate
transaction_without_state_count
orphan_state_count
p95_transaction_state_gap_ms
```

Interpretation:

```text
transaction_without_state_count increase
→ transaction exists without state transition

orphan_state_count increase
→ state exists without transaction evidence

p95_transaction_state_gap_ms increase
→ delayed state transition
```

This layer evaluates:

```text
Are transaction workflows operationally consistent?
```

---

# State Timeliness / Orphan State Measurement

State analysis evaluates more than state existence.

The architecture evaluates:

```text
When did the state occur?
Was it connected to a valid transaction?
Did it violate operational SLA?
Does state exist without transaction evidence?
```

Representative measurements:

```text
orphan_state_count
transaction_without_state_count
p95_transaction_state_gap_ms
refund_transition_gap
delivery_delay
```

This layer evaluates:

```text
Can state data reliably explain
the operational workflow?
```

---

# v05_reconciliation_measurement_day

`v05_reconciliation_measurement_day` is the authoritative measurement center.

Representative outputs:

```text
behavior_transaction_match_rate
transaction_state_match_rate

behavior_only_count
transaction_only_count
transaction_without_state_count
orphan_state_count

duplicate_order_count
duplicate_payment_count

conversion_gap
payment_order_gap
refund_transition_gap
coupon_reconciliation_gap
p95_transaction_state_gap_ms
```

This table becomes the direct input to:

```text
reliability_analysis_result_day_v05
```

---

# Analytics Layer

The analytics layer calculates reliability component scores from measured evidence.

Core principles:

```text
No scenario-name heuristics
Measurement-delta driven
Baseline/noise separation
Runtime evidence as supplementary input
```

Meaning:

```text
A system is not risky
because the scenario is named "source_partial_missing".

It becomes risky because
actual measurement deltas changed.
```

---

# reconciliation_gap_score

`reconciliation_gap_score` measures inconsistency between:

```text
Behavior
↔ Transaction
↔ State
```

Representative components:

```text
behavior_only_rate
transaction_only_rate
transaction_state_unmatched_rate
orphan_state_rate
transaction_without_state_rate
```

Methodology:

```text
Convert gap counts into normalized rates
Calculate baseline deviation
Aggregate multiple reconciliation gaps
```

Meaning:

```text
How severely is operational consistency broken?
```

---

# propagation_score

`propagation_score` measures how far source or canonical instability propagated downstream.

Example:

```text
behavior source loss
→ canonical behavior reduction
→ match rate degradation
→ reconciliation gap expansion
```

Methodology:

```text
source/canonical delta analysis
mapping gap analysis
reconciliation delta analysis
```

Meaning:

```text
Did upstream instability propagate
into downstream operational inconsistency?
```

---

# amplification_score

`amplification_score` measures whether anomalies became amplified through duplication or replay.

Representative inputs:

```text
duplicate_order_count
duplicate_payment_count
stream duplicate rate
replay duplicate evidence
```

Methodology:

```text
Convert duplicate counts into normalized rates
Compare against baseline duplicate levels
Validate using batch/stream evidence
```

Meaning:

```text
Did a single anomaly become amplified
through replay or duplication?
```

---

# distortion_score

`distortion_score` measures the likelihood of KPI or business interpretation distortion.

Representative inputs:

```text
conversion_gap
payment_order_gap
refund_transition_gap
coupon_reconciliation_gap
delivery_delay
```

Methodology:

```text
Identify KPI-related gaps
Calculate baseline deviation
Aggregate funnel/payment/refund/coupon distortion
```

Meaning:

```text
Can operational KPI interpretation become distorted?
```

---

# transaction_loss_score

`transaction_loss_score` measures transaction loss or transaction-state disconnection.

Representative inputs:

```text
transaction_only_gap
transaction_without_state_gap
payment_order_gap
transaction_state_unmatched_rate
```

Methodology:

```text
Validate transaction existence
Validate downstream state transitions
Validate payment/order linkage
```

Meaning:

```text
Did transactions disappear,
or fail to propagate into operational state?
```

---

# customer_impact_score

`customer_impact_score` evaluates impact from the customer journey perspective.

Representative inputs:

```text
behavior gap
transaction_state gap
state delay
delivery delay
coupon/conversion distortion
```

Methodology:

```text
Connect journey-stage gaps
Evaluate checkout/payment/delivery/refund disruption
```

Meaning:

```text
Can operational inconsistency
impact customer experience?
```

---

# runtime_evidence_score

`runtime_evidence_score` is the supplementary runtime score inherited from v0.4 evidence.

Components:

```text
batch_evidence_score
stream_evidence_score
operational_evidence_score
realism_evidence_score
```

Methodology:

```text
Batch instability
Stream instability
Pipeline execution instability
Source realism degradation
```

Important:

```text
runtime_evidence_score
≠
authoritative business risk
```

Instead, it supplements reconciliation analytics.

---

# Final Architecture Definition

The current measurement and analytics layer is not a simple anomaly detection architecture.

More precisely, it is a:

```text
Behavior Evidence Measurement
+
Behavior-Transaction-State Reconciliation Measurement
+
Runtime Evidence Integration
+
Measurement-driven Reliability Analytics
```

architecture.

The philosophy is simple:

```text
Measure first.
Analyze second.
Judge using measurement deltas,
not scenario names.
```

Ultimately, this layer serves as the:

```text
Operational Measurement-to-Analytics Foundation
```

of the overall Reliability Architecture.

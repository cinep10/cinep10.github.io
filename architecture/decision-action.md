# Decision / Action Architecture

The core differentiator of the current v0.5 architecture is that it is not a simple anomaly detection pipeline.

Instead, the architecture connects:

```text
Measurement
→ Reliability Analytics
→ Unified Risk
→ Operational Action
```

into a complete:

```text
Measurement-to-Decision Reliability Architecture
```

This means the system does not merely explain:

```text
“What is abnormal?”
```

but also:

```text
Why is it risky?
What kind of risk is it?
What is the final risk level?
What operational action is required?
```

---

# Decision / Action Architecture Overview

![Decision / Action](/assets/images/decision-action-architecture.png)

---

# Overall Structure

The current Decision / Action Layer consists of three major architectural layers:

```text
1. Reliability Analytics Architecture
2. Unified Risk Architecture
3. Operational Action Architecture
```

An important principle is that:

```text
each layer has an independent responsibility
```

In other words:

```text
Analytics
=
risk signal analysis

Risk
=
final risk integration

Action
=
operational response decision
```

---

# Reliability Analytics Architecture

Reliability Analytics transforms:

```text
Measurement
→ Risk Signal
```

This layer analyzes consistency and instability across:

```text
Behavior
↔ Transaction
↔ State
```

Representative inputs include:

```text
Behavior Measurement
Transaction Measurement
State Measurement
Runtime Evidence Measurement
```

---

# Analytics Categories

The current Reliability Analytics layer consists of the following major categories.

---

## Reconciliation Analytics

Purpose:

```text
Cross-domain consistency analysis
```

Representative metrics:

```text
behavior_transaction_match_rate
transaction_state_match_rate
transaction_without_state
orphan_state
behavior_only
```

Key questions:

```text
Did behavior occur without transactions?
Did transactions occur without state transitions?
```

In other words:

```text
Behavior ↔ Transaction ↔ State
Consistency Analysis
```

---

## Propagation Analytics

Purpose:

```text
Analyze how anomalies propagate downstream
```

Representative analysis targets:

```text
source propagation
canonical propagation
mapping propagation
measurement propagation
```

Example:

```text
source anomaly
→ canonical distortion
→ reconciliation gap
→ risk escalation
```

This layer focuses on:

```text
cross-layer anomaly propagation
```

---

## Amplification Analytics

Purpose:

```text
Analyze anomaly amplification and duplication
```

Representative analysis targets:

```text
duplicate order
duplicate payment
retry amplification
replay amplification
```

Example:

```text
a single order request
→ amplified into duplicate payments
```

---

## Distortion Analytics

Purpose:

```text
Analyze business KPI distortion risk
```

Representative analysis targets:

```text
conversion_gap
payment_order_gap
coupon_gap
refund_transition_gap
```

Example:

```text
conversion appears normal,
but actual transaction/state flows are inconsistent
```

---

## Customer Impact Analytics

Purpose:

```text
Analyze customer experience impact
```

Representative analysis targets:

```text
delivery delay
payment-state gap
refund delay
customer transaction failure
```

Example:

```text
payment completed,
but delivery state was never updated
```

---

## Runtime Evidence Analytics

Purpose:

```text
Analyze Batch / Stream / Runtime operational stability
```

Representative analysis targets:

```text
batch evidence
stream evidence
operational evidence
realism evidence
```

Example:

```text
stream replay mismatch
batch distribution drift
operational delay
```

An important principle:

```text
Runtime Evidence
=
Supplementary Evidence
```

Runtime evidence supports reliability interpretation,
but does not override authoritative reconciliation consistency.

---

# Unified Risk Architecture

Unified Risk integrates:

```text
multiple analytics signals
→ a single operational risk structure
```

This layer combines:

```text
Reconciliation
Propagation
Amplification
Distortion
Customer Impact
Runtime Evidence
```

into a unified operational risk interpretation.

---

# Unified Risk Categories

The current Unified Risk layer consists of the following categories.

---

## Reconciliation Risk

Meaning:

```text
Behavior ↔ Transaction ↔ State
consistency risk
```

---

## Distortion Risk

Meaning:

```text
Business KPI distortion risk
```

---

## Transaction Loss Risk

Meaning:

```text
Transaction loss or state-transition disconnection risk
```

---

## Customer Impact Risk

Meaning:

```text
Customer experience impact risk
```

---

## Runtime Reliability Risk

Meaning:

```text
Batch / Stream / Runtime operational instability risk
```

---

# Unified Reliability Score

These risk components are ultimately integrated into:

```text
overall_risk_score
final_risk_level
```

Representative risk levels:

```text
stable
low
warning
high
critical
```

An important policy is:

```text
No False Escalation
```

Meaning:

```text
evidence existence
≠
immediate high-risk escalation
```

Baseline operational noise should not trigger unnecessary escalation.

---

# Operational Action Architecture

The Operational Action Layer transforms:

```text
Risk
→ Action
```

This layer determines:

```text
What operational response is required?
```

based on the final risk interpretation.

---

# Action Categories

The current Action Layer consists of the following categories.

---

## Reconciliation Action

Purpose:

```text
Behavior ↔ Transaction ↔ State
consistency validation
```

Representative actions:

```text
transaction reconciliation audit
payment-state validation
coupon reconciliation review
```

---

## Pipeline Validation Action

Purpose:

```text
Batch / Stream / Runtime
operational validation
```

Representative actions:

```text
runtime evidence review
stream replay validation
batch pipeline validation
```

---

## Replay / Recovery Action

Purpose:

```text
Recovery and replay-based operational restoration
```

Representative actions:

```text
transaction replay
state recovery
duplicate suppression
```

---

## Monitoring Action

Purpose:

```text
Observation and monitoring-based operational response
```

Representative actions:

```text
monitor
observe
targeted validation
```

---

# No-false-action Guard

One of the most important principles of the current architecture is:

```text
No False Action
```

Meaning:

```text
runtime evidence existence
≠
immediate operational escalation
```

For example:

```text
final_risk_level = low
dominant_semantic_risk = None
```

results in:

```text
recommended_action = no action
```

---

# Architecture Responsibility Separation

One of the core architectural philosophies is:

```text
clear separation of responsibilities
```

In other words:

---

## Analytics analyzes risk signals

---

## Risk integrates final operational severity

---

## Action determines operational response

---

Each layer must not directly override another layer’s responsibility.

```text
Analytics
≠
Action

Risk
≠
Measurement

Action
≠
Risk Calculation
```

---

# Technical Philosophy

The technical philosophy of v0.5 is:

```text
SQL
=
Measurement / Persistence

R
=
Reliability Analytics / Risk Interpretation

Python
=
Operational Orchestration / Action
```

In other words:

```text
SQL
→ What was measured?

R
→ Why is it risky?

Python
→ What operational action should be taken?
```

---

# Final Definition

The current Decision / Action Architecture is not a simple alerting system.

More precisely, it is a:

```text
Measurement
→ Reliability Analytics
→ Unified Risk
→ Operational Action
```

architecture that interprets:

```text
Behavior
↔ Transaction
↔ State
```

from the perspectives of:

```text
consistency
distortion
propagation
customer impact
operational stability
```

and ultimately determines:

```text
What operational action should be taken?
```

This is a:

```text
Cross-domain Decision Reliability Architecture
```

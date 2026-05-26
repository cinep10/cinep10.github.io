# Customer Journey Architecture

## Overview

In v0.5, Customer Journey does not simply mean a clickstream or a sequence of user actions.

Within the current architecture, Customer Journey acts as an:

```text id="j4z0p1"
Operational Truth Model
```

that simultaneously generates:

```text id="d8r1f2"
Behavior
Transaction
State
```

The architecture therefore goes beyond simple weblog analytics.

Instead, it is designed as a:

```text id="a7n3k9"
Cross-Domain Operational Reliability Architecture
```

for measuring inconsistency and distortion across:

```text id="r2k9q4"
Behavior ↔ Transaction ↔ State
```

---

# Overall Structure

The core structure of v0.5 is:

```text id="v9q4w7"
Customer Journey
├─ Behavior Log
├─ Transaction Log
└─ State Transition Log
```

An important design principle is:

```text id="f3m8u1"
Behavior Logs do not directly trigger Transaction Logs
```

Instead, the actual structure is:

```text id="y1t5g8"
Customer Journey Model
→ Behavior Projection
→ Transaction Projection
→ State Projection
```

This means all three logs are:

```text id="q6b2n5"
different operational observation surfaces
generated from the same journey
```

---

# Why Customer Journey Matters

Traditional systems are often modeled as:

```text id="w4s7c2"
Behavior Event
→ Transaction
→ State Change
```

However, real operational environments frequently produce inconsistencies such as:

```text id="m7x2k1"
Behavior exists
BUT Transaction is missing

Transaction exists
BUT State transition is missing

State exists
BUT Behavior does not exist
```

Meaning:

```text id="h8r4d6"
Behavior / Transaction / State
```

are separate operational observation layers,
and they are not always perfectly aligned.

v0.5 is specifically designed to measure and interpret this:

```text id="z3v8j5"
Cross-domain inconsistency
```

---

# Journey Stages

Customer Journey is modeled as a stage-based operational flow.

Example:

```text id="p1q7x9"
visit
→ browse
→ search
→ product_view
→ cart
→ coupon_apply
→ checkout
→ payment
→ order
→ delivery
→ refund
```

Each stage is represented differently across:

* Behavior Logs
* Transaction Logs
* State Transition Logs

---

# Identity and Lineage

Multiple identities are generated within the journey.

Representative examples:

```text id="t6n3w8"
journey_id
pcid
sid
uid

order_id
payment_id
delivery_id
cart_id
coupon_id
transaction_id
```

These identities function as lineage keys connecting:

```text id="g5v1q4"
Behavior ↔ Transaction ↔ State
```

An important design principle in v0.5 is:

```text id="u2k7m9"
Identity realism
```

For example:

```text id="l4q8s2"
browse stage:
anonymous possible

checkout/payment stage:
authenticated possible
```

This reflects realistic:

```text id="c7m2d5"
partial identity consistency
```

found in real operational systems.

---

# Behavior Log

Behavior Logs record user web/app interactions.

Examples:

```text id="s5t8n4"
page_view
search
product_view
cart_view
checkout_click
payment_click
```

v0.5 preserves a:

```text id="r9m3x7"
W3C-compatible weblog structure
```

The primary objective of Behavior Logs is:

```text id="y8k4v2"
measuring behavioral observation reliability
```

Representative risks include:

* source_partial_missing
* source_latency_degradation
* source_identity_drift
* conversion distortion
* bot traffic anomaly

---

# Transaction Log

Transaction Logs record business events.

Examples:

```text id="n4c8w1"
cart_created
payment_requested
payment_approved
order_created
refund_requested
```

The structure is closer to:

```text id="q2m7v6"
JSONL-based business event architecture
```

The core objective is:

```text id="h6t1x3"
verifying whether user behavior
actually resulted in transactions
```

Representative measurements:

```text id="p8j4m7"
behavior_transaction_match_rate
behavior_only_count
transaction_only_count
```

---

# State Transition Log

State Transition Logs record post-transaction workflow and operational state transitions.

Examples:

```text id="v1x7k5"
order_state: created → delivered
payment_state: requested → approved
delivery_state: assigned → delivered
refund_state: requested → completed
```

This layer represents:

```text id="m5q9t2"
Workflow / Operational State Truth
```

The primary objective is:

```text id="f4w8j6"
verifying whether operational state transitions
continued correctly after transactions
```

Representative measurements:

```text id="x3n7r1"
transaction_state_match_rate
state_transition_gap_count
state_delay_ms
```

---

# Core Design Philosophy

The most important design philosophy of v0.5 is:

```text id="j8v2m4"
Behavior alone is not sufficient.
Transaction alone is not sufficient.
Operational Risk only becomes visible
when State is connected as well.
```

Meaning:

```text id="z7m1k9"
Behavior
+
Transaction
+
State
```

must be interpreted together in order to answer questions such as:

```text id="r5x8c1"
Did the customer click payment?
Was the payment actually approved?
Was the order state correctly created?
Did delivery states continue properly?
Was the refund completed successfully?
```

---

# Final Direction

In v0.5, Customer Journey is not simply a clickstream model.

More precisely, it is:

```text id="u6q3m8"
an Operational Truth Model
that simultaneously generates
Behavior / Transaction / State
```

and also serves as the:

```text id="y4n7v2"
lineage root of cross-domain reconciliation
```

for operational reliability analysis.

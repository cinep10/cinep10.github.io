# Customer Journey Architecture

## Overview

In v0.5, Customer Journey does not simply mean a clickstream or a sequence of user actions.

Within the current architecture, Customer Journey acts as an:

```text
Operational Truth Model
```

that simultaneously generates:

```text
Behavior
Transaction
State
```

The architecture therefore goes beyond simple weblog analytics.

Instead, it is designed as a:

```text
Cross-Domain Operational Reliability Architecture
```

for measuring inconsistency and distortion across:

```text
Behavior ↔ Transaction ↔ State
```

---

# Overall Structure

The core structure of v0.5 is:

```text
Customer Journey
├─ Behavior Log
├─ Transaction Log
└─ State Transition Log
```

An important design principle is:

```text
Behavior Logs do not directly trigger Transaction Logs
```

Instead, the actual structure is:

```text
Customer Journey Model
→ Behavior Projection
→ Transaction Projection
→ State Projection
```

This means all three logs are:

```text
different operational observation surfaces
generated from the same journey
```

---

# High-level Architecture

![Data Generation Architecture](/assets/images/high-level-architecture.png)

---

# Why Customer Journey Matters

Traditional systems are often modeled as:

```text
Behavior Event
→ Transaction
→ State Change
```

However, real operational environments frequently produce inconsistencies such as:

```text
Behavior exists
BUT Transaction is missing

Transaction exists
BUT State transition is missing

State exists
BUT Behavior does not exist
```

Meaning:

```text
Behavior / Transaction / State
```

are separate operational observation layers,
and they are not always perfectly aligned.

v0.5 is specifically designed to measure and interpret this:

```text
Cross-domain inconsistency
```

---

# Journey Stages

Customer Journey is modeled as a stage-based operational flow.

Example:

```text
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

```text
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

```text
Behavior ↔ Transaction ↔ State
```

An important design principle in v0.5 is:

```text
Identity realism
```

For example:

```text
browse stage:
anonymous possible

checkout/payment stage:
authenticated possible
```

This reflects realistic:

```text
partial identity consistency
```

found in real operational systems.

---

# Behavior Log

Behavior Logs record user web/app interactions.

Examples:

```text
page_view
search
product_view
cart_view
checkout_click
payment_click
```

v0.5 preserves a:

```text
W3C-compatible weblog structure
```

The primary objective of Behavior Logs is:

```text
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

```text
cart_created
payment_requested
payment_approved
order_created
refund_requested
```

The structure is closer to:

```text
JSONL-based business event architecture
```

The core objective is:

```text
verifying whether user behavior
actually resulted in transactions
```

Representative measurements:

```text
behavior_transaction_match_rate
behavior_only_count
transaction_only_count
```

---

# State Transition Log

State Transition Logs record post-transaction workflow and operational state transitions.

Examples:

```text
order_state: created → delivered
payment_state: requested → approved
delivery_state: assigned → delivered
refund_state: requested → completed
```

This layer represents:

```text
Workflow / Operational State Truth
```

The primary objective is:

```text
verifying whether operational state transitions
continued correctly after transactions
```

Representative measurements:

```text
transaction_state_match_rate
state_transition_gap_count
state_delay_ms
```

---

# Core Design Philosophy

The most important design philosophy of v0.5 is:

```text
Behavior alone is not sufficient.
Transaction alone is not sufficient.
Operational Risk only becomes visible
when State is connected as well.
```

Meaning:

```text
Behavior
+
Transaction
+
State
```

must be interpreted together in order to answer questions such as:

```text
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

```text
an Operational Truth Model
that simultaneously generates
Behavior / Transaction / State
```

and also serves as the:

```text
lineage root of cross-domain reconciliation
```

for operational reliability analysis.

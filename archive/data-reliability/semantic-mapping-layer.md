# Semantic Mapping Layer

## Translating Raw Logs into Meaningful Business Events

---

## 1. Overview

In a data pipeline, raw logs are simply collections of strings.

For example:

```text
GET /loan/apply.do
GET /auth/otp.do
GET /card/apply.do
GET /transfer/confirm.do
```

At this stage, the system cannot determine:

* whether this is a view, start, or submission
* whether it is an authentication event
* which step of a funnel it belongs to

In other words, raw logs alone are not usable for analysis or decision-making.

---

## 2. Purpose

The purpose of the Semantic Mapping Layer is:

> to transform raw logs into meaningful behavioral events

Without this layer:

* raw URLs remain simple strings
* funnels cannot be defined
* validation, drift, and risk become meaningless numbers

---

## 3. Processing Flow

```text
raw url / method / event
    ↓
event_mapping
    ↓
semantic event transformation
    ↓
funnel_stage / event_type / category assignment
    ↓
mapping coverage calculation
    ↓
used in validation / risk / action
```

Actual implementation flow:

```text
stg_webserver_log_hit
    ↓
apply event_mapping
    ↓
split into mapped / unmapped
    ↓
generate mapping_coverage_day
```

---

## 4. Role of event_mapping

The `event_mapping` table is not just a lookup table.
It functions as a rule-based dictionary.

Example:

| url             | method | event_name        | funnel_stage | category |
| --------------- | ------ | ----------------- | ------------ | -------- |
| /auth/otp.do    | GET    | otp_request       | auth         | login    |
| /loan/apply.do  | GET    | loan_apply_start  | start        | loan     |
| /loan/submit.do | POST   | loan_apply_submit | submit       | loan     |

Through this mapping:

```text
/auth/otp.do → authentication event
/loan/apply.do → loan application start
/loan/submit.do → loan submission
```

Raw logs are transformed into structured behavioral meaning.

---

## 5. Why It Matters

### 5.1 Foundation of Funnel Definition

Funnel analysis begins with a simple question:

> What is the conversion rate from start to submit?

To answer this, the following must be defined first:

```text
url → event → funnel_stage
```

Without this mapping, funnel analysis is not possible.

---

### 5.2 Quality Baseline for Validation

Without mapping:

* unknown events increase
* unmapped URLs increase
* funnel steps are missing

This leads to persistent validation warnings.

---

### 5.3 Direct Input to Risk Scoring

Mapping coverage directly affects risk calculation.

```text
incomplete mapping
→ lower coverage
→ higher mapping_score
→ higher risk_score
```

Mapping is not auxiliary data.
It is a core input to the Risk Layer.

---

### 5.4 Connection to Action Engine

Example:

```text
funnel_distortion + low coverage
→ likely mapping issue
→ action: mapping_fix
```

Mapping quality directly influences system actions.

---

## 6. Mapping Coverage

Key metric:

```text
mapping_coverage = mapped_events / total_events
```

Example:

| dt         | total  | mapped | unmapped | coverage |
| ---------- | ------ | ------ | -------- | -------- |
| 2026-03-05 | 100000 | 98000  | 2000     | 0.98     |
| 2026-03-06 | 120000 | 90000  | 30000    | 0.75     |

---

### Interpretation

A drop in coverage is not necessarily an error.

It may indicate:

* new URLs introduced
* system structure changes
* missing event definitions

This makes coverage a signal of structural change.

---

## 7. Mapped vs Unmapped

### Mapped

* events successfully interpreted using mapping rules

### Unmapped

* events with no matching rule

Examples:

* new pages
* test endpoints
* typos
* newly introduced paths

---

### Key Insight

An increase in unmapped events means:

```text
not just missing data ❌
but potential structural change ⭕
```

---

## 8. Relationship with Other Layers

### Validation

* increase in unmapped → more validation warnings

### Drift

* sudden spike in unmapped → may appear as drift

### Risk

* lower coverage → higher risk score

### Root Cause

* funnel_distortion + low coverage
  → possible mapping issue

### Action

* recommended action: mapping_fix

---

## 9. Operational Strategy

### Normal State

* run mapping_coverage monitoring automatically

### When Issues Occur

* analyze top unmapped paths
* identify new URLs
* update mapping rules if necessary

---

### Principle

```text
no daily manual updates ❌
update only when needed ⭕
```

---

## 10. Automation vs Human Intervention

### What Can Be Automated

* coverage calculation
* unmapped detection
* risk integration

### What Cannot Be Automated

* defining event meaning
* designing funnel stages
* interpreting business context

---

### Key Principle

```text
detection can be automated
meaning definition requires human input
```

---

## 11. Design Summary

The Semantic Mapping Layer:

* translates raw logs into business events
* manages translation quality through coverage
* provides the foundation for validation, drift, risk, and action

---

## Summary

The Semantic Mapping Layer is the foundation of data interpretation.

Without it:

* metrics lose meaning
* validation becomes unreliable
* risk becomes misleading

---

## One-line Definition

Semantic Mapping Layer =
a layer that translates raw logs into business events
and manages the quality of that translation through coverage

---

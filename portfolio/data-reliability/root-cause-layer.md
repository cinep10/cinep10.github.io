# Root Cause / Action Layer Implementation

An implementation layer that decomposes risk signals into explainable causes and translates them into operational actions

---

## 1. Overview

The Root Cause / Action Layer is not a secondary or optional feature in this project.

It is the core interpretation layer that transforms pre-ML signals into a form that operators can actually use.

From an implementation perspective, this layer has three responsibilities:

- Decompose a high Risk Score back into its contributing signals  
- Organize those signals into human-readable cause types  
- Convert each cause into an actionable operational task  

Structurally, the flow is as follows:

```text
risk / validation / drift / anomaly / mapping
    ↓
root_cause_and_contribution_runner
    ↓
data_risk_root_cause_day
risk_signal_link_day
    ↓
action_engine_runner
    ↓
data_reliability_action_day
```

This is the layer that turns the system from a detection pipeline into an explainable, operational data reliability system.

---

## 2. Execution Flow

In the actual pipeline, the Root Cause / Action Layer runs after the Risk Score layer.

```text
validation
drift
time anomaly
correlation anomaly
mapping coverage
risk score
    ↓
root cause
    ↓
action engine
    ↓
ML / AI / Dashboard
```

In execution order, the flow is typically:   
**1.**	Risk Score calculation completed   
**2.**	Root Cause runner executed   
**3.**	Action Engine executed

This means:
	- The Root Cause Layer is a post-risk interpretation layer
	- The Action Engine is an operational translation layer that consumes root cause outputs

---

## 3. Related Implementation Files

The key implementation files in the current project are:

Root Cause Layer
	- pipelines/root_cause_and_contribution_runner.py

Action Layer
	- pipelines/action_engine_runner_v2.py

Main Output Tables
	- data_risk_root_cause_day
	- risk_signal_link_day
	- data_reliability_action_day

In other words, the implementation pattern is:

```text
runner → persistence → downstream consumption
```

---

## 4. Root Cause Layer Implementation

### 4.1 Input Data

root_cause_and_contribution_runner does not rely on a single table.

It assumes multi-signal interpretation across the pre-ML pipeline.

Representative inputs include:
	•	data_risk_score_day_v3
	•	validation_result or validation_summary_day
	•	metric_drift_result_r
	•	metric_time_anomaly_day
	•	metric_correlation_anomaly_day
	•	mapping_coverage_day

To explain the risk for a specific date, the runner re-reads the major signals generated for that same date.

---

### 4.2 Core Implementation Idea

The core idea of this runner can be summarized in one sentence:

It decomposes the final risk score back into signal-level causes.

If the Risk Score is already an aggregated result, then Root Cause breaks that result down into components such as:
	•	increased validation failures
	•	high-drift metrics
	•	correlation anomalies
	•	mapping coverage degradation

Each of these becomes a cause candidate, and the runner scores and ranks them.

This is not a simple alert generator.
It is a mechanism for reconstructing the structure behind the risk.

---

### 4.3 Internal Processing Flow

The internal flow can be understood as follows:

```text
receive input parameters (profile_id, dt)
    ↓
load risk_score_day
    ↓
collect validation signals
    ↓
collect drift signals
    ↓
collect time anomaly signals
    ↓
collect correlation anomaly signals
    ↓
collect mapping coverage signals
    ↓
generate cause candidates
    ↓
calculate contribution scores
    ↓
rank causes
    ↓
store data_risk_root_cause_day
    ↓
store risk_signal_link_day
```

The key point is that the score is transformed back into an explainable structure.

---

### 4.4 Cause Candidate Generation

The runner does not store raw signals as-is.

Instead, it translates them into operator-oriented cause types.

Common cause types in the current project include:
	•	funnel_distortion
	•	traffic_mix_shift
	•	metric_drift
	•	mapping_issue
	•	pipeline_issue

Examples:
	•	correlation anomaly in the submit/apply relationship
→ funnel_distortion
	•	sharp drop in daily_active_users with drift alert
→ traffic_mix_shift
	•	mapping coverage degradation
→ mapping_issue
	•	multiple validation failures
→ pipeline_issue

This is a key transformation:

raw anomaly → operational cause type

---

### 4.5 Contribution Score Calculation

Root Cause is not just a list of labels.
It quantifies how much each cause contributed to the final risk.

Conceptually:

contribution_score =
    signal_strength
    × severity_weight
    × confidence_weight

Examples:
	•	higher drift score → larger contribution
	•	more validation failures → larger contribution
	•	correlation anomaly on a key funnel metric → additional weighting

This is not simple counting.
It combines signal strength, importance, and confidence.

---

### 4.6 data_risk_root_cause_day

This is the final table for storing date-level root cause results.

Representative fields include:
	•	profile_id
	•	dt
	•	cause_rank
	•	cause_type
	•	cause_code
	•	related_metric
	•	confidence
	•	contribution_score

Multiple rows can exist for a single date, and cause_rank = 1 represents the most important root cause.

A conceptual example:

2026-03-29
1 | funnel_distortion | submit_capture_rate | 0.82
2 | traffic_mix_shift | daily_active_users  | 0.63
3 | mapping_issue     | mapping_coverage    | 0.44

This table shows which issue mattered most, in ranked form.

---

### 4.7 risk_signal_link_day

This table stores relationships between signals and causes.

Examples:
	•	drift_alert → funnel_distortion
	•	mapping_drop → mapping_issue
	•	validation_fail → pipeline_issue

This matters because it allows the system to explain Root Cause as a signal chain rather than as an isolated point.

In practice:
	•	data_risk_root_cause_day = final cause summary
	•	risk_signal_link_day = structure behind the cause

---

## 5. Technical Meaning of the Root Cause Layer

### 5.1 Intermediate Structure for Explainability

Many systems stop after storing a risk score.

This project does not.

Instead, it transforms risk into an explainable structure with three levels:
	•	the final score
	•	the causes behind that score
	•	the signal links behind those causes

This is one of the strongest differentiators in the current system.

---

### 5.2 Role as AI Input Structure

Without this layer, AI summaries degrade significantly.

A raw risk score alone is not enough for high-quality explanations.

AI needs root causes and signal links to generate:
	•	incident titles
	•	technical summaries
	•	recommended actions

In that sense, the Root Cause Layer also acts as an explainability preprocessor for AI.

---

### 5.3 Operator-friendly Translation

Operators understand:
	•	“submit_capture_rate collapsed, causing funnel_distortion”

much better than:
	•	“risk score = 0.77”

This layer turns numbers into operational language.

---

## 6. Action Engine Layer Implementation

### 6.1 Input Data

action_engine_runner_v2.py typically consumes:
	•	data_risk_root_cause_day
	•	risk_signal_link_day
	•	data_risk_score_day_v3

It does not generate actions from risk score alone.
It reads both causes and signal relationships together.

---

### 6.2 Core Implementation Idea

The core idea of the Action Engine is:

Map cause types into action types, then specialize action titles and recommended fixes based on related metrics.

It is essentially a rule-based operational translator.

---

### 6.3 Internal Processing Flow

```text
The flow is:

receive profile_id, dt
    ↓
load root cause results for the date
    ↓
identify top cause types
    ↓
map cause type → action type
    ↓
generate action title based on related metric
    ↓
determine priority
    ↓
generate recommended_fix
    ↓
store data_reliability_action_day
```

If Root Cause is the explanation layer, then Action Engine is the execution layer.

---

### 6.4 Cause Type → Action Type Mapping

This mapping is one of the most important implementation points.

Representative mappings include:
	•	mapping_issue → mapping_fix
	•	funnel_distortion → funnel_fix
	•	traffic_mix_shift → traffic_event
	•	pipeline_issue → pipeline_issue
	•	metric_drift → anomaly_investigate

In other words:
	•	cause_type = explanation-oriented classification
	•	action_type = operation-oriented classification

---

### 6.5 Action Title Generation

Action types alone are too abstract.

The engine attaches the related metric to make the action concrete.

Examples:
	•	Check loan_view_count
	•	Check otp_request_count
	•	Check daily_active_users
	•	Check submit_capture_rate
	•	Check mapping_coverage_loan

Conceptually:

action_title = "Check " + related_metric

This is simple, but highly effective in dashboards and operational workflows.

---

### 6.6 Priority Assignment

Priority is usually assigned based on:
	•	risk grade
	•	cause rank
	•	cause type criticality

Examples:
	•	cause_rank = 1 → higher priority
	•	pipeline_issue → higher priority
	•	simple traffic_event → medium priority

The current structure is not yet a full SLA system, but it is already extensible toward high / medium / low operational prioritization.

---

### 6.7 Recommended Fix Generation

recommended_fix is another important implementation point.

It does not stop at “investigation needed.”
It provides cause-specific inspection guidance.

Examples:

mapping_issue
	•	verify event mapping rules
	•	inspect whether unmapped URLs increased

funnel_distortion
	•	check submit_capture_rate
	•	inspect the apply → submit flow

traffic_mix_shift
	•	inspect traffic source / campaign
	•	verify sudden spike/drop patterns

pipeline_issue
	•	inspect parser / loader / upstream schema

In practice, recommended_fix is a compact form of an operational runbook.

---

### 6.8 data_reliability_action_day

Representative fields:
	•	profile_id
	•	dt
	•	metric_nm
	•	root_cause
	•	action_type
	•	priority
	•	recommended_fix

This table functions as an operational action catalog, even without AI.

It can be consumed directly in dashboards.

---

## 7. How Root Cause and Action Work Together

These two layers are tightly connected.

```text
risk_score
    ↓
root cause decomposition
    ↓
action generation
```

This means:
	•	high risk score
	•	leads to root cause generation
	•	which leads to action generation

Because of this structure, the project is not just:
	•	a score-based system
	•	an anomaly system
	•	an ML system

It becomes an explainable, operational data reliability system.

---

## 8. Strengths of the Current Implementation

### 8.1 It turns risk into explainable causes

It does not stop at storing scores.

### 8.2 It turns causes into actions

It does not stop at explanation.

### 8.3 It connects directly to AI and dashboards

AI summaries and action recommendations are based on this layer.

### 8.4 It is highly extensible to financial domains

This layer becomes even stronger when extended to deposits / loans.

Examples of future cause types:
	•	approval_issue
	•	execution_missing
	•	balance_inconsistency

---

## 9. Current Limitations

### 9.1 Cause taxonomy is still web-log oriented

The current taxonomy is centered on funnel / mapping / traffic / pipeline patterns.

### 9.2 Priority rules can be more sophisticated

Business criticality and SLA-aware prioritization can be improved.

### 9.3 Owner hints are still weak

It would be stronger if actions could automatically point to likely owners.

Examples:
	•	data-platform
	•	product
	•	marketing
	•	backend

---

## 10. Future Directions

### 10.1 Expand into an explicit Decision Framework Layer

A separate decision_framework_runner could explicitly define:
	•	decision state
	•	action_required_flag

### 10.2 Add finance-oriented root causes

Examples:
	•	approval_drop
	•	execution_missing
	•	balance_mismatch

### 10.3 Improve action templates

recommended_fix can evolve into a richer operational runbook structure.

### 10.4 Tighter integration with AI

AI could use cause rank and action priority more directly.

---

## 11. Summary

From an implementation perspective, the Root Cause / Action Layer is:

a decision-support layer that decomposes the signals behind the risk score into structured causes and translates those causes into operational actions

Because of this layer, the project is no longer just a score calculation system.

It becomes a system with:

Explainable Data Behavior + Operational Action Recommendation

---

## One-line Definition

Root Cause / Action Layer =
An implementation layer that decomposes risk signals into explainable causes and translates them into operational actions


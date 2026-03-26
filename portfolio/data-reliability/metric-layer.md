# Metric Layer

## Transforming Raw Events into Measurable and Reliable Signals

---

## 1. Problem

After structuring raw logs in the Data Layer,  
the data is still not directly usable for analysis.

Raw events only describe:

- who performed an action  
- when it occurred  
- which resource was accessed  
- what type of event was triggered  

---

However, real-world systems require answers to questions such as:

- How many active users are there?  
- Is the login success rate normal?  
- Is the funnel conversion stable?  
- How long do sessions last?  

---

### Core Issue

> Raw events do not provide meaning  
> They must be interpreted into metrics

---

### Critical Risk

Even with the same raw data:

- different identity definitions → different DAU  
- different page view rules → different traffic  
- different event interpretation → different conversion rates  

---

> If metrics are unstable,  
> Validation, Drift, and Risk all become unreliable

---

## 2. Design Goal

The Metric Layer is designed to:

> Transform raw events into reproducible and interpretable measurements

---

### Objectives

- define consistent interpretation rules  
- standardize identity and session logic  
- provide time-based aggregation  
- serve as a foundation for Validation, Drift, and Risk  

---

## 3. Architecture


stg_webserver_log_hit
↓
event interpretation / identity resolution / sessionization
↓
hourly aggregation
↓
metric_value_hh
↓
daily aggregation
↓
metric_value_day


---

## 4. Input Data

The Metric Layer operates on structured staging data:


stg_webserver_log_hit


---

### Key Fields

- dt / ts  
- ip  
- method  
- url_norm  
- status  
- uid / pcid / sid  
- evt  
- page_type  
- kv_raw  

---

These fields are transformed into behavioral metrics.

---

## 5. Key Design Decisions

---

### 5.1 Metrics are Interpretation, Not Aggregation

A metric is not just a count.

It is the result of interpretation rules.

---

### Example: Page View

Before counting page views, we must define:

- allowed HTTP methods  
- valid status codes  
- exclusion of static resources  
- event type filtering  

---

> Metric Layer encodes interpretation rules into the system

---

### 5.2 Identity Resolution

User-based metrics require a stable identity definition.

---

### Hierarchical Identity Strategy


uid → pcid → ip


---

### Logic

1. use uid when available  
2. fallback to pcid  
3. fallback to ip  

---

### Reason

- uid is not always present  
- pcid may be missing  
- ip alone is unreliable  

---

> A hierarchical approach balances accuracy and coverage

---

### 5.3 Sessionization

Session is not blindly trusted from logs.

It is redefined based on behavior.

---

### Rule

- same identity  
- inactivity gap (e.g. 1800 seconds)  
→ new session  

---

### Reason

- sid may be missing or inconsistent  
- behavior-based session is more reliable  

---

### Derived Metrics

- session_count  
- avg_session_duration_sec  

---

### 5.4 Event Standardization

Raw logs contain inconsistent event naming.

---

### Example Mapping Rules

- otp + request → otp_request  
- auth + success → auth_success  
- login + success → login_success  
- loan + submit → loan_apply_submit  

---

### Purpose

Standardize events for consistent analysis.

---

> Metric Layer bridges raw data and semantic meaning

---

## 6. Metric Design

Metrics are grouped into four categories.

---

### 6.1 User Activity Metrics

---

#### Daily Active Users (DAU)


DAU = count(distinct identity with valid activity)


---

#### Page View Count

Defined as meaningful page interactions, not raw hits.

---

#### Avg Session Duration


duration = event_ts - session_start_ts
avg_duration = mean(duration)


---

#### New User Ratio


new_user_ratio = new_users / total_users


---

### 6.2 Authentication Metrics

---

- auth_attempt_count  
- auth_success_count  
- auth_fail_count  

---

#### Success Rate


auth_success_rate = success / attempt


---

#### Failure Rate


auth_fail_rate = fail / attempt


---

### 6.3 Funnel Metrics

---

#### Loan Funnel

- loan_view_count  
- loan_apply_start_count  
- loan_apply_submit_count  

---

#### Card Funnel

- card_apply_start_count  
- card_apply_submit_count  

---

#### Conversion Rate


submit_rate = submit / start


---

### Key Insight

> Relationships matter more than absolute counts

---

### 6.4 System Metrics

---

#### Raw Event Count

Used for:

- volume monitoring  
- pipeline comparison  
- traffic vs quality analysis  

---

## 7. Aggregation Strategy

---

### Hourly (HH)

Purpose:

- drift detection  
- pattern analysis  
- anomaly localization  

---

### Daily (DAY)

Purpose:

- risk calculation  
- reporting  
- decision-making  

---

### Design Insight

- HH = sensitivity  
- DAY = stability  

---

## 8. Output Structure

---

### metric_value_hh

- profile_id  
- dt / hh  
- metric_name  
- metric_value  
- numerator_value  
- denominator_value  

---

### metric_value_day

- profile_id  
- dt  
- metric_name  
- metric_value  
- numerator_value  
- denominator_value  

---

### Advantages

- preserves numerator/denominator  
- supports recalculation  
- shared input for downstream layers  

---

## 9. Role in the System

Metric Layer is not just a transformation step.

It defines the meaning of the system.

---

### Why It Matters

- Validation depends on metric correctness  
- Drift depends on metric stability  
- Risk depends on metric signals  

---

## 10. Key Insights

---

### Metrics define system behavior

Different definitions produce different conclusions.

---

### Metric design equals reliability design

Unstable metrics lead to unreliable systems.

---

### Ratios and relationships are critical

They reveal structure, not just volume.

---

### Metric Layer is often underestimated

But it determines all downstream logic.

---

## 11. Conclusion

Metric Layer is not about counting events.

It is about:

> Defining how data is interpreted and measured

---

## One-line Summary

Metric Layer = Converting raw data into measurable and reliable meaning

---

## Portfolio Statement

Metrics were designed not as simple counts,  
but as behavior-driven measurements incorporating identity resolution,  
sessionization, event standardization, and funnel structure.

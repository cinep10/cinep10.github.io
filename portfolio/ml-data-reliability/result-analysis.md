# Web Log-Based Data Reliability Platform Results

## Interpreting the Pre-ML → ML → AI Pipeline Through Grafana Dashboards

---

## 1. Overview

These results are not just about showing that the dashboards render correctly.

The key outcome is that the reliability signals generated in the Pre-ML pipeline are consistently propagated into the ML and AI layers. The Grafana dashboards make that end-to-end flow visible.

The system can be summarized as:

Detection → Aggregation → Explanation → Action

More concretely:

* The Pre-ML dashboard shows data quality issues, drift, structural anomalies, and root causes directly.
* The ML dashboard compresses those signals into a normal / warning / alert state model.
* The AI dashboard translates the results into incident summaries and recommended actions.

This means the system is no longer a standalone anomaly detector. It operates as an end-to-end data reliability platform.

---

## 2. Pre-ML Dashboard Analysis

### Dashboard

**Data Reliability Control - Operating Review v6**

This is the primary operational dashboard.
It presents the actual reliability state of the system before ML or AI interpretation.

---

### 2.1 Top KPI Panels

The top row contains the core indicators for Validation, Drift, and Root Cause.

#### Latest Validation Fail = 1

This means that on the latest date, exactly one validation rule failed at the fail level.

Interpretation:

* The data is not completely broken.
* However, at least one explicit rule violation exists.

In other words, the system is not in full failure mode, but it is not fully healthy either.

---

#### Latest Validation Warn = 24

This means that 24 validation rules were triggered at the warning level on the latest date.

Interpretation:

* A large number of borderline data quality issues are accumulating.
* The data is not entirely invalid, but multiple warning-level anomalies are present.

This suggests a state of distributed instability rather than catastrophic data corruption.

---

#### Latest Drift Alert = 100

This is one of the most important signals in the dashboard.

A Drift Alert count of 100 means that the dominant source of current risk is not explicit data failure, but large-scale deviation from baseline behavior.

Interpretation:

* The system state is being driven more by behavioral change than by hard validation failures.
* This is not a single metric anomaly; it indicates simultaneous changes across many metrics.

In practical terms, drift is the main driver of the current operational risk.

---

#### Latest Drift Warn = 49

Drift warnings are also elevated.

The combination of 100 alerts and 49 warnings suggests broad instability across the metric space.

Interpretation:

* This is unlikely to be noise.
* It is more consistent with structural changes in traffic, funnel behavior, or capture patterns.

---

#### Latest Root Cause Summary

The latest root cause summary includes:

* funnel_distortion
* metric_drift
* traffic_mix_shift

Interpretation:

The current state is not driven by a simple validation issue. It is a composite condition formed by:

* funnel distortion
* traffic composition changes
* metric-level drift

This panel effectively acts as the top-level label for the operational state of the system.

---

### 2.2 Data Availability Check

This panel shows row counts and latest dates for major tables such as:

* `data_risk_score_day_v3`
* `validation_summary_day`
* `metric_drift_result_r`

This is not just a health panel. It is an operational readiness check.

It answers:

* Are the upstream tables actually populated?
* Is the dashboard showing a real pipeline result, or just an empty view?
* Are the downstream interpretations valid to read?

This is a practical feature of an operational system, not just an experiment dashboard.

---

### 2.3 Validation Summary Trend

This panel shows trends in:

* `pass_count`
* `fail_count`
* `warn_count`

Observed pattern:

* pass remains dominant
* fail remains low
* warn persists over time

Interpretation:

The current issue is not primarily large-scale data breakage.
It is better described as a state where warning-level signals accumulate while drift increases.

That distinction matters because it changes how operators interpret the system:

* not a broken pipeline
* but an unstable behavioral pattern

---

### 2.4 Drift Alert / Warn Trend

This panel shows the temporal evolution of drift alerts and warnings.

Observed pattern:

* a major increase appears around mid-to-late January
* elevated drift continues afterward
* recent values remain high

Interpretation:

Drift is not a one-time anomaly.
It is a recurring risk axis across multiple time periods.

This confirms that drift is not a supporting signal.
It is one of the core engines of risk in the current system.

---

### 2.5 Mapping Coverage & Capture Rate

This panel includes metrics such as:

* `mapping_coverage_auth`
* `mapping_coverage_card`
* `mapping_coverage_loan`
* `submit_capture_rate`
* `success_outcome_capture_rate`

This panel is important because it measures not only data presence, but semantic interpretability.

Interpretation:

If mapping coverage degrades:

* event interpretation degrades
* funnel metrics become distorted
* downstream risk, root cause, and action quality also degrade

This is one of the main differentiators of the project.
It treats semantic coverage as part of reliability, not as a secondary concern.

---

### 2.6 Funnel Health Trend

This panel tracks key funnel metrics such as:

* `auth_attempt_count`
* `auth_success_count`
* `card_apply_start_count`
* `card_apply_submit_count`
* `loan_apply_start_count`
* `loan_apply_submit_count`

Observed pattern:

* periodicity exists
* stage movements are not fully synchronized
* submit-stage metrics fluctuate more than upstream stages in some periods

Interpretation:

When funnel stages stop moving together, the system produces `funnel_distortion` as a root cause.

This panel is therefore a direct evidence panel for root cause generation.

---

### 2.7 Root Cause Trend

This panel tracks the occurrence of cause types over time.

The dominant causes include:

* funnel_distortion
* metric_drift
* traffic_mix_shift

Interpretation:

Recent system risk is not explained by a single cause.
It is a composite state created by multiple cause types.

The repeated prominence of `funnel_distortion` indicates that the dominant failure mode is a recurring breakdown in funnel-stage alignment.

---

### 2.8 Latest Validation Detail

This panel provides the latest validation details by rule group.

Representative rule groups include:

* funnel
* sanity
* cross_system
* mapping_quality

Interpretation:

This panel allows operators to identify which reliability dimension is currently affected.

It enables questions such as:

* Should we inspect funnel integrity first?
* Is this primarily a mapping-quality issue?
* Is there a cross-system inconsistency?

This makes the dashboard operationally actionable rather than purely descriptive.

---

### 2.9 Latest Drift Detail

This panel lists the most affected metrics in the latest drift calculation.

Representative examples include:

* `estimated_missing_rate`
* `new_user_ratio`
* `avg_session_duration_sec`
* `raw_event_count`
* `daily_active_users`
* `collector_event_count`

Interpretation:

Drift is not isolated to one funnel metric.
It extends into user composition, session behavior, and raw collection volume.

This suggests that the issue is broader than conversion instability.
It points to possible changes in traffic mix, capture structure, or collection behavior.

---

### 2.10 Latest Root Cause Detail / Top Root Causes on Latest Day

This panel shows the top-ranked root causes for the latest date.

Representative ranking:

1. funnel_distortion
2. funnel_distortion
3. traffic_mix_shift
4. metric_drift
5. metric_drift

Related metrics include:

* `loan_apply_submit_count`
* `card_apply_submit_count`
* `daily_active_users`
* `otp_request_count`
* `loan_view_count`

Interpretation:

The most recent issue is primarily driven by submit-stage funnel distortion.
Traffic mix shift and metric drift follow as secondary contributors.

This is important because the system is not only saying “today is risky.”
It is identifying which metrics are responsible and why.

---

### 2.11 Funnel Snapshot on Latest Day

This panel provides a snapshot of latest funnel volumes.

Its role is simple but important:

It gives operators direct access to raw latest-day volumes while they interpret root causes and trends.

This allows three levels of reading at once:

* root cause
* historical trend
* latest raw snapshot

---

### 2.12 No Data Diagnostic Summary / Interpretation Guide

These panels provide practical operational guidance.

#### No Data Diagnostic Summary

Confirms whether each layer actually has available data.

#### Interpretation Guide

Documents how the dashboard should be read and how patterns connect to likely actions.

These are not decorative panels.
They function as embedded operational documentation for dashboard users.

---

## 3. ML Dashboard Analysis

### Dashboard

**ML Backfill Monitoring - Prediction and Scenario Review v2**

This dashboard shows how the ML layer interprets the outputs of the Pre-ML pipeline.

---

### 3.1 Top KPI Panels

#### ML Feature Rows = 89

This means that 89 rows were stored in `ml_feature_vector_day`.

Interpretation:

* the ML input dataset was successfully generated
* the Pre-ML → ML handoff is working correctly

---

#### Latest Alert Probability = 79.4%

This indicates that the latest date is much closer to an alert or warning state than to a normal state.

Interpretation:

* the latest date is high-risk
* the direction is aligned with the Pre-ML risk results

---

#### Latest Actual = 0.800

This is the actual latest risk score.

Interpretation:

* the latest state is clearly high-risk
* it is consistent with the 79.4% alert probability

---

#### Scenario Rows = 61

This represents the row count of `scenario_experiment_result_day`.

Interpretation:

The ML layer is not just predicting passively.
It is connected to a scenario-based validation loop.

---

### 3.2 ML Table Availability

This panel confirms that the following key tables are available:

* `ml_feature_vector_day`
* `ml_prediction_result`
* `scenario_experiment_result_day`

This is the ML equivalent of a readiness panel.

---

### 3.3 Target Risk Status Distribution

Observed distribution:

* alert = 29
* warning = 20
* normal = 40

This is a very important result.

Interpretation:

* labels are not collapsing into a single class
* class diversity is sufficient for supervised classification
* the system has moved beyond the earlier single-class fallback state

This is strong evidence that the current ML layer is operating on a valid classification structure.

---

### 3.4 Latest Prediction Summary

Recent predictions show patterns such as:

* 2026-03-31 → warning
* 2026-03-30 → alert
* 2026-03-29 → alert

Interpretation:

* the recent period remains in a high-risk zone
* the latest date appears to have shifted slightly from alert toward warning

This is meaningful because the model is not collapsing into a single alert state.
It is differentiating between adjacent severity levels.

---

### 3.5 Prediction vs Actual Risk Score

This panel compares:

* `prob_alert * 100`
* `actual_risk_score`

Observed behavior:

* both curves move broadly in the same direction during major risk periods

Interpretation:

The ML model is not acting as an independent anomaly detector.
It is better understood as a risk-aware classifier that compresses the Pre-ML risk state into operational classes.

---

### 3.6 Predicted Risk Status Trend

This panel shows the predicted status over time as points.

Interpretation:

* normal, warning, and alert all appear
* no single-class collapse is present
* the model is functioning as a true classifier rather than a degenerate predictor

---

### 3.7 Scenario Experiment Result / Scenario Risk vs Predicted Alert

These panels compare scenario-level risk and predicted alert probability.

Interpretation:

* scenario-specific risk and predicted alert generally move in the same direction
* the ML model is not just reacting to superficial metrics
* it appears to capture some of the structural risk introduced by the scenarios

This is important because it suggests that the model is scenario-aware at a useful operational level.

---

### 3.8 Feature Vector Sample

This panel shows sample rows from the input feature vector.

Its value is that it reveals not only prediction outputs, but also the realism and structure of the inputs themselves.

This makes the ML dashboard more trustworthy, because operators can inspect both:

* what the model predicted
* what the model actually saw

---

### 3.9 Prediction Detail

Recent prediction details show that:

* actual alert cases are generally captured as alert or warning
* warning states are placed near warning/alert boundaries
* normal states remain distinct

Interpretation:

The model is not perfect, but it is effective for what it is supposed to do:

**risk stratification**

That is sufficient for an operational warning system.

---

## 4. AI Dashboard Analysis

### Dashboard

**AI Incident Review for MariaDB v2**

This dashboard translates the outputs of Pre-ML and ML into operator-facing language and actions.

---

### 4.1 Top KPI Panels

#### Latest Confidence Score = 65%

This indicates that the current AI summary confidence is around 65%.

Interpretation:

* the system is currently in `fallback_rule_based` mode
* confidence is relatively stable because the mode is rule-based rather than generative

This means the AI layer is currently operating as a stable operational summarizer, not a full LLM reasoning engine.

---

#### Latest Risk Score = 0.800

The AI summary directly incorporates the latest risk score.

Interpretation:

AI is not making an independent risk judgment.
It is interpreting the risk already produced upstream.

---

#### Latest Predicted Alert Probability = 79.4%

The AI layer also directly reads the predicted alert probability from the ML layer.

Interpretation:

AI and ML are tightly connected.
AI summaries are grounded in real model outputs.

---

### 4.2 AI Incident Level Trend

This panel shows incident level over time.

Observed pattern:

* normal in early periods
* warning appears in mid-risk periods
* alert appears in later high-risk windows

Interpretation:

The AI incident level is directionally aligned with Pre-ML and ML outputs.
This confirms that AI is acting as an operational summarization layer, not as a conflicting independent classifier.

---

### 4.3 Confidence Score vs Predicted Alert Probability

This panel compares:

* confidence score
* predicted alert probability

Interpretation:

* confidence remains relatively stable
* severity changes are mainly driven by predicted alert probability

This shows that the AI layer is not inventing severity.
It is using upstream risk signals to generate an explanation.

---

### 4.4 AI Incident Summary Detail

This is one of the most important panels in the AI dashboard.

Representative fields:

* dt
* incident_level
* incident_title
* confidence_score
* llm_model
* executive_summary
* technical_summary
* business_impact

Recent examples include:

* 2026-03-31 → warning / funnel_distortion detected
* 2026-03-30 → alert / funnel_distortion detected
* 2026-03-29 → alert / funnel_distortion detected

Interpretation:

The AI layer is using the latest root cause as the incident title and translating the risk state into an operational report structure.

The fact that `llm_model = fallback_rule_based` is also important.

Even without external LLM reasoning, the system is already capable of producing structured operational summaries.

---

### 4.5 AI Recommended Actions

Representative recent actions include:

* Check loan_view_count
* Check otp_request_count
* Check daily_active_users
* Check submit_capture_rate
* Check page_view_count
* Check raw_event_count

Action types include:

* anomaly_investigate
* traffic_investigate
* monitor_funnel

Evidence includes:

* metric_drift
* traffic_mix_shift
* funnel_distortion

Interpretation:

This panel is critical because it shows that the AI layer does not stop at explanation.

It turns risk and root cause into an operational checklist.

---

### 4.6 ML vs AI Decision Alignment

This panel compares ML predicted risk status with AI incident level.

Interpretation:

The two layers are broadly aligned.
This confirms that AI is not acting as an independent judge.
It is a higher-order interpretation layer grounded in ML outputs.

---

### 4.7 Action Type Distribution

Representative distribution:

* anomaly_investigate = 161
* monitor_funnel = 71
* traffic_investigate = 29

Interpretation:

The dominant operational burden in the recent period has been:

* metric anomaly investigation
* funnel monitoring

This tells us what kind of work the system is generating most often for operators.

---

## 5. Overall Interpretation

The most important conclusions from the Grafana results are the following.

### 5.1 The real core is still Pre-ML

The clearest result is that the real foundation of the system remains the Pre-ML reliability pipeline.

Signals such as:

* validation
* drift
* mapping
* structural anomaly
* root cause

already have strong explanatory power on their own.

ML and AI are important, but the core logic of the system still lives in the Pre-ML pipeline.

---

### 5.2 ML is functioning well as a risk-aware classifier

The ML layer should not be interpreted as a fully independent anomaly detector.

It is better understood as a classifier that compresses:

* risk score
* scenario structure
* reliability signals

into operational classes.

The current results show:

* no class collapse
* meaningful separation of high-risk periods
* practical differentiation between warning and alert

That is already a strong and useful operational state.

---

### 5.3 AI has entered the system naturally as an interpretation layer

The AI layer is currently running in `fallback_rule_based` mode, but the results are still meaningful.

It already produces:

* incident levels
* incident summaries
* recommended actions
* alignment with ML outputs

This means the AI layer has found the right architectural position:

not as a decision engine, but as an operational interpreter.

---

## 6. Conclusion

These Grafana results show that the system has successfully implemented the following structure in practice:

Detection
→ Risk Aggregation
→ Root Cause Explanation
→ Action Recommendation
→ ML Classification
→ AI Interpretation

The platform now connects:

* data quality validation
* behavioral change detection
* risk scoring
* root cause analysis
* action generation
* ML-based risk compression
* AI-based incident summarization

This is no longer a simple anomaly detection pipeline.

It is an operational data reliability platform.

---

## 7. Next Steps

The next stage of improvement is clear.

* ML: richer labels and stronger multi-class design
* AI: real LLM reasoning integration
* Decision Layer: explicit OK / WARN / FAIL operational states
* Domain Expansion: deposits / loans funnel integrity and state consistency

The current version is a first complete operational form.
The next step is structured expansion into financial domain workflows.

---

## One-line Summary

These Grafana results demonstrate that the Pre-ML → ML → AI → Action pipeline is fully connected in practice.

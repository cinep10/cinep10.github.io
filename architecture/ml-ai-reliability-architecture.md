# ML / AI Reliability Architecture

The current v0.5 ML/AI structure is not a conventional “AI-driven prediction system.”

The core philosophy is:

```text
ML
=
Reliability Calibration Layer

AI / LLM
=
Evidence-bound Explanation & Validation Layer
```

This means:

```text
ML does not determine final operational risk
LLM does not perform unrestricted reasoning
```

The authoritative core of the architecture remains:

```text
Measurement
→ Reliability Analytics
→ Unified Risk
→ Operational Action
```

ML/AI operates only after this stage for:

```text
Calibration
Explanation
Validation
Governance
```

The most important principle is:

```text
ML/AI is also governed inside the Reliability Architecture
```

---

# ML / AI Architecture Overview

![ML / AI Architecture](/assets/images/ml-ai-architecture.png)

---

# Overall Structure

The current ML/AI Architecture consists of two major layers:

```text
1. ML Reliability Calibration Architecture
2. AI / LLM Reliability Architecture
```

An important principle is:

```text
ML/AI
≠
Authoritative Decision Engine
```

The authoritative decision flow remains:

```text
Measurement
Analytics
Risk
Action
```

while ML/AI performs:

```text
Calibration
Interpretation
Validation
```

---

# ML Reliability Calibration Architecture

The purpose of the current ML layer is not:

```text
Prediction-first ML
```

Instead, the core purpose is:

```text
Reliability Calibration
```

The ML layer analyzes:

```text
Is the current risk/action distribution reasonable?
Is baseline falsely escalated?
Is semantic concentration occurring?
```

---

# ML Architecture Flow

The current ML flow is:

```text
Measurement
→ Analytics
→ Risk
→ Action
→ Feature Snapshot
→ Calibration
```

Importantly:

```text
ML does not consume raw logs directly
```

The ML layer consumes:

```text
measurement
analytics
semantic
risk
action
```

outputs from the reliability pipeline.

This is one of the key architectural differentiators.

---

# ML Feature Snapshot

Representative structure:

```text
Feature Snapshot
```

Representative feature categories:

```text
Reconciliation Feature
Analytics Feature
Risk Feature
Action Feature
Runtime Evidence Feature
```

The feature snapshot preserves:

```text
Behavior ↔ Transaction ↔ State
```

reliability results as structured operational features.

The important principle is:

```text
ML does not directly interpret raw events
```

It only consumes outputs already processed through the Reliability Pipeline.

---

# Why Simple ML Algorithms Were Chosen

The reason complex ML models were intentionally deprioritized is clear.

The core philosophy is:

```text
Black-box Accuracy
<
Reliability Explainability
```

The most important requirement is:

```text
Operational risk must be explainable
```

Therefore, the architecture prioritizes:

```text
Threshold Calibration
Distribution Analysis
Baseline-relative Scoring
Semantic Consistency Review
```

instead of:

```text
XGBoost
Deep Learning
Transformer Forecasting
```

---

# Threshold Calibration

This is currently the most important ML function.

Purpose:

```text
Prevent False Escalation
```

Example:

```text
high runtime evidence alone
must not trigger semantic escalation
```

This is fundamentally:

```text
measurement-driven threshold calibration
```

---

# Distribution Calibration

Purpose:

```text
Stabilize semantic/risk/action distribution
```

Representative analysis:

```text
risk concentration
semantic over-convergence
low/no-action over-convergence
```

This is effectively:

```text
distribution anomaly detection
```

---

# Baseline-relative Calibration

The core philosophy is:

```text
Relative change from baseline
>
Absolute value
```

The system focuses on:

```text
How far did the current state deviate from the normal baseline?
```

This is fundamentally:

```text
baseline-relative anomaly scoring
```

---

# Risk Weight Calibration

Current risk interpretation is composed of multiple components:

```text
reconciliation_gap
distortion
runtime evidence
customer impact
transaction loss
```

ML calibration also performs:

```text
weight tuning
```

Its role is to determine:

```text
Which signals are overly dominating final risk interpretation?
```

---

# Why Heavy Prediction Models Were Not Prioritized

In the current architecture:

```text
Prediction Accuracy
```

is less important than:

```text
Operational Reliability Interpretability
```

For example:

```text
Deep Learning predicts high risk
```

but:

```text
cannot explain why
```

then the result becomes difficult to trust operationally.

Therefore the architecture prioritizes:

```text
Explainable calibration-centered ML
```

---

# Future ML Expansion

Possible future extensions include:

---

## Isolation Forest

Purpose:

```text
runtime evidence outlier detection
distribution anomaly detection
```

---

## Autoencoder

Purpose:

```text
Behavior ↔ Transaction ↔ State
normal pattern reconstruction
```

---

## Gradient Boosting

Purpose:

```text
future escalation probability prediction
```

---

## Temporal Forecasting

Purpose:

```text
runtime evidence trend forecasting
transaction-state delay forecasting
```

---

# AI / LLM Reliability Architecture

The core philosophy of the AI Layer is:

```text
LLM
≠
Authoritative Risk Engine
```

The LLM acts as:

```text
Evidence-bound Reliability Explanation Engine
```

The AI layer only explains:

```text
measurement
analytics
risk
action
```

results already produced by the reliability engine.

---

# Why LLM Cannot Be the Authoritative Core

LLMs structurally have several limitations.

---

## Non-deterministic Output

The same input may produce:

```text
different outputs
```

This introduces:

```text
lack of reproducibility
```

---

## Hallucination Risk

LLMs may generate:

```text
non-existent operational explanations
```

Example:

```text
explaining a payment outage
when no outage evidence exists
```

---

## Unsupported Causal Reasoning

LLMs may produce:

```text
unsupported causal assumptions
```

without measurement evidence.

---

## Wrong Operational Recommendation

LLMs may recommend:

```text
urgent escalation
```

even during normal baseline conditions.

---

# Why Evidence-bound AI Was Chosen

The key principle is:

```text
Free-form LLM reasoning is restricted
```

Therefore LLM inputs are constrained to:

```text
measurement
analytics
risk
action
runtime evidence
```

This creates an:

```text
Evidence-bound Context
```

architecture.

---

# AI Architecture Flow

The current AI flow is:

```text
Measurement
→ Analytics
→ Risk
→ Action
→ AI Context
→ AI Explanation
→ AI Validation
→ AI Reliability Score
```

Importantly:

```text
LLM does not directly read raw logs
```

---

# AI Function Categories

The AI Layer performs four major functions.

---

## Incident Explanation

Purpose:

```text
Explain why the system is risky
```

Example:

```text
Behavior-Transaction reconciliation gap increased
because transaction_state_match_rate decreased.
```

---

## Operational Narrative

Purpose:

```text
Generate operator-readable incident narratives
```

Example:

```text
Potential transaction-state consistency degradation detected.
```

---

## Evidence Summarization

Purpose:

```text
Summarize measurement / semantic / risk evidence
```

---

## Action Reasoning

Purpose:

```text
Explain why a specific operational action was recommended
```

---

# AI Reliability Validation Architecture

One of the most important architectural principles is:

```text
AI output is also subject to reliability validation
```

Meaning:

```text
LLM output
≠
truth
```

---

# Validation Categories

The current AI Validation layer consists of:

---

## Missing Evidence Validation

Purpose:

```text
Prevent explanations without evidence
```

---

## Unsupported Explanation Validation

Purpose:

```text
Prevent explanations unrelated to measurements
```

---

## Hallucinated Reconciliation Validation

Purpose:

```text
Prevent fabricated reconciliation interpretations
```

---

## Wrong Recommendation Validation

Purpose:

```text
Prevent operational recommendations inconsistent with risk/action evidence
```

---

# AI Reliability Score

Validation results are integrated into:

```text
AI Reliability Score
```

Representative levels:

```text
pass
review
fail
```

---

# PASS_WITH_REVIEW Policy

An important operational policy is:

```text
AI FAIL
≠
Pipeline FAIL
```

Meaning:

```text
AI validation FAIL
+
AI reliability level = review
→ PASS_WITH_REVIEW
```

This means:

```text
The core reliability engine remains authoritative,
while AI explanations require human review.
```

---

# Architecture Responsibility Separation

One of the most important architectural principles is:

```text
Clear Separation of Responsibility
```

Meaning:

---

## Measurement defines authoritative truth

---

## Analytics generates risk signals

---

## Risk determines final operational severity

---

## Action determines operational response

---

## ML performs calibration

---

## AI performs explanation

---

## AI Validation prevents hallucination

---

Therefore:

```text
ML
≠
Final Decision

LLM
≠
Risk Authority
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
Reliability Analytics / Calibration

Python
=
Orchestration / Validation / Action

LLM
=
Evidence-bound Interpretation
```

Meaning:

```text
SQL
→ What was observed?

R
→ Why is it risky?

Python
→ How should it be executed and validated?

LLM
→ What should be explained?
```

---

# Final Definition

The current ML/AI Architecture is not a generic AI operations layer.

More precisely, it is a:

```text
Measurement
→ Reliability Analytics
→ Unified Risk
→ Operational Action
→ ML Calibration
→ AI Explanation
→ AI Validation
→ AI Reliability
```

architecture that governs:

```text
Behavior
↔ Transaction
↔ State
```

reliability decisions through:

```text
Calibration
Interpretation
Validation
Governance
```

This is fundamentally a:

```text
ML / AI Reliability Architecture
```

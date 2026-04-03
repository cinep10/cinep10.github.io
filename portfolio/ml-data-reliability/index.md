# ML Data Reliability

ML Data Reliability focuses on ensuring that machine learning systems operate on reliable, consistent, and interpretable data.

This category extends data reliability concepts into ML systems, bridging data engineering and model behavior.

---

## Architecture Position

ML Data Reliability is positioned after the data reliability pipeline.

```text
data → metric → validation → drift → risk
                                ↓
                        ML feature layer
                                ↓
                        model prediction
                                ↓
                        explainability
```

This layer connects data quality with model behavior.

---

## Scope

This category covers:

* feature-level data validation
* input data drift detection
* distribution analysis
* training vs serving consistency
* prediction impact and explainability

The focus is not on model development, but on **data reliability for ML systems**.

---

## Perspective

This category does not focus on building models.

Instead, it focuses on:

* ensuring reliable model inputs
* detecting data-driven risks
* explaining model behavior

---

## Related Topics

* Feature Drift Detection
* Prediction Drift
* Training vs Serving Consistency
* Explainable AI

---

## Summary

ML Data Reliability ensures that machine learning systems operate on stable and trustworthy data.

It bridges:

* data engineering
* data reliability
* machine learning systems

---

## One-line Definition

ML Data Reliability =
ensuring that machine learning systems operate on reliable, consistent, and explainable data

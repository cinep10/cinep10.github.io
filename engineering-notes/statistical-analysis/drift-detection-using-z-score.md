# Drift Detection using Z-score (R Implementation)

## Statistical approach for detecting distribution changes in metrics

---

## 1. Problem

In data reliability systems, drift is not always obvious.

Even when validation passes:

* metrics can shift gradually
* user behavior can change
* distribution can move

The question is:

> How can we quantify “change” in data?

---

## 2. Approach

One of the simplest and most effective methods is:

Z-score based drift detection

---

## 3. Data Setup

Example metric:

* hourly user_count
* baseline: previous 7 days

---

## 4. Z-score Definition

```
z = (x - mean) / std
```

Where:

* x = current value
* mean = historical average
* std = standard deviation

---

## 5. R Implementation

```r
compute_z_score <- function(x, baseline) {
  mean_val <- mean(baseline)
  std_val <- sd(baseline)
  
  if (std_val == 0) {
    return(0)
  }
  
  return((x - mean_val) / std_val)
}
```

---

## 6. Example

```r
baseline <- c(100, 105, 98, 102, 110, 95, 101)
current <- 130

z <- compute_z_score(current, baseline)
print(z)
```

---

## 7. Interpretation

| Z-score | Meaning |
| ------- | ------- |
| < 2     | normal  |
| 2~3     | warning |
| > 3     | anomaly |

---

## 8. Key Insight

* Z-score is simple but effective
* Works well for stable metrics
* Sensitive to sudden spikes

---

## 9. Limitation

* assumes normal distribution
* weak for skewed data
* not robust for seasonality

---

## 10. When to Use

* quick drift detection
* baseline monitoring
* real-time alerting

---

## Summary

Z-score provides a lightweight statistical method
to quantify metric deviation from historical patterns.

# [Case] Traffic Spike caused Conversion Distortion

## 1. Problem
Traffic increased significantly, but conversion rate dropped unexpectedly.

## 2. Symptoms
- user_count increased
- conversion_rate decreased
- no system error detected

## 3. Root Cause
Funnel distortion caused by imbalance between click and submit events.

## 4. Solution
- Applied drift detection on funnel metrics
- Linked validation + drift + risk scoring
- Identified structural anomaly

## 5. Result
- Early detection of anomaly
- Improved metric reliability

## 6. Insight
Even when data is valid, structural imbalance can distort business interpretation.
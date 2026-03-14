# Web Log Analytics Data Consistency Issue

## Session ID in URL Rewriting Causing PV Mismatch

In web analytics environments, it is common to compare metrics generated from different systems.

Typical examples include:

- web server access logs
- analytics platforms
- dashboard metrics

Ideally, the numbers should match.

However, in real-world systems, discrepancies often appear.

This article describes a real operational case where **Page View (PV) metrics differed between web server logs and the analytics platform**, and how the issue was investigated and resolved.

All service identifiers and URLs have been anonymized.

---

# 1. Problem

During a validation process, a discrepancy was discovered for a specific page.

| Source | Page Views |
|------|------|
| Web Server Access Log | 1,587 |
| Analytics Platform | 1,097 |

The difference was significant (about 30%).

The analyzed page looked like this:

https://example.service/page.do?event_id=14

---

# 2. Analytics Architecture

The analytics system architecture looked like this:

```text
User Browser
│
▼
Web Server (Access Log)
│
▼
Log Collection
│
▼
Analytics Engine
│
▼
Dashboard / Reports
```

In theory:

Web Log PV  ≈  Analytics PV

But the numbers were different.

---

# 3. Log Investigation

During the log inspection, an unusual URL pattern appeared.

Normal URL

/service/page.do?event_id=14

Actual URL in logs

/service/page.do;jsessionid=ABCD123456789?event_id=14

Important part:

;jsessionid=XXXX

This value represents the **Session ID**.

---

# 4. Example Log Entry

Example access log:

192.168.0.1 - - [12/Feb/2026:09:12:44] “GET /service/page.do;jsessionid=A123BC?event_id=14 HTTP/1.1” 200
192.168.0.2 - - [12/Feb/2026:09:12:51] “GET /service/page.do;jsessionid=D992XY?event_id=14 HTTP/1.1” 200

From the web server perspective, these requests are simply page views.

However, the analytics system processed them differently.

---

# 5. Session Delivery Mechanism

Session IDs are typically delivered in two ways.

## Cookie-based Session

Most modern systems use cookies.

Cookie: JSESSIONID=ABCD123456789

The session ID does not appear in the URL.

---

## URL Rewriting Session

If cookies are unavailable, session IDs are embedded into the URL path.

Example:

/page.do;jsessionid=ABCD123456789

This method is called **URL Rewriting**.

The investigated system was using URL rewriting.

---

# 6. Why Data Consistency Broke

Web log URLs:

/service/page.do;jsessionid=ABC123
/service/page.do;jsessionid=XYZ987

Analytics system interpretation:

/service/page.do

During the URL parsing stage, the analytics engine either:

- treated session URLs as separate pages
- filtered them out

This produced the following behavior.

| URL | Processing |
|----|----|
| /page.do | counted |
| /page.do;jsessionid=xxxx | filtered or treated as different |

As a result:

- some page views were excluded
- some were split across different URLs

Which caused the PV mismatch.

---

# 7. Validation Using SQL

To validate the discrepancy, raw logs were analyzed.

Example SQL validation query:

SELECT
COUNT(*) AS pv
FROM
access_logs
WHERE
request_uri LIKE ‘/service/page.do%’

Result:

pv = 1587

However, analytics system results:

pv = 1097

This confirmed that some requests were not included in analytics aggregation.

---

# 8. Solution

The solution was to normalize URLs by removing session identifiers.

Example regex rule:

;jsessionid=.*

Or a normalized pattern:

/service/page.do(;jsessionid=.*)?

After normalization:

/page.do
/page.do;jsessionid=xxxxx

Both URLs are treated as the same page.

---

# 9. Result After Fix

After adjusting URL normalization rules:

| Source | Page Views |
|------|------|
| Web Server Access Log | 1,587 |
| Analytics Platform | 1,790 |

The PV increased because previously filtered logs were now included.

---

# 10. Data Reliability Perspective

This issue is not simply a logging problem.

It is a **data reliability problem**.

Modern analytics pipelines depend on multiple processing layers:

```text
User Event
│
▼
Web Server
│
▼
Log Collector
│
▼
Data Pipeline
│
▼
Analytics Engine
│
▼
Dashboard
```

Small inconsistencies in any stage can produce misleading metrics.

---

# 11. Hidden Data Failures

Many data problems are not system failures.

Instead, they are **hidden data failures** such as:

- event filtering
- parameter parsing differences
- session rewriting
- schema mismatches

These failures do not break the system.

They silently affect analytics results.

---

# 12. Relationship to Data Drift

This case also relates to **data drift detection**.

A sudden PV change may appear like a business change.

Example:

PV yesterday : 1500
PV today     : 1100

Possible interpretations:

- real user behavior change
- marketing campaign change
- **data pipeline failure**

Statistical monitoring systems may detect this as **data drift**.

However, the root cause can be structural issues such as URL normalization.

---

# 13. Recommended Validation Workflow

A reliable validation workflow should include:

```text
Web Access Log
│
▼
Raw Log Validation
│
▼
URL Pattern Analysis
│
▼
Analytics Filter Inspection
│
▼
Metric Reconciliation
```

This approach helps identify discrepancies early.

---

# 14. Key Lessons

## 1. Always inspect URL structure

Important factors include:

- URL parameters
- session identifiers
- tracking parameters
- URL rewriting rules

---

## 2. Log-based validation is critical

Analytics systems should always be validated against raw logs.

---

## 3. Session IDs are a common cause of data inconsistency

Especially in environments using:

- Java web applications
- URL rewriting sessions
- proxy rewriting
- CDN URL modifications

---

# Conclusion

In this case, the discrepancy between web logs and analytics data was caused by **session IDs embedded in URLs through URL rewriting**.

By normalizing URL structures and removing session identifiers during analysis, the issue was resolved.

Reliable analytics requires not only data pipelines but also **careful validation of data collection structures**.

Understanding how data is generated, processed, and interpreted is essential for maintaining trustworthy analytics systems.

---

# Reference Architecture

```text
User
│
▼
Web Server
│
▼
Access Log
│
▼
Log Collector
│
▼
Analytics Engine
│
▼
Dashboard
```

# Verifying Page View Consistency from Web Access Logs

In web analytics environments, it is common to observe differences between **raw web server requests** and **page view (PV) metrics reported by analytics systems**.

At first glance, it may appear that both numbers should match. However, in practice, the definition of a *page view* is usually determined by the analytics system's internal filtering and aggregation rules.

This note summarizes a practical approach to **verifying PV consistency using web access logs**.

---

# Why PV Counts Often Do Not Match

Web server access logs record every HTTP request received by the server.  
However, analytics systems typically apply filtering rules before counting page views.

Examples of requests recorded in access logs include:

- Static resource requests (images, CSS, JavaScript)
- API calls
- background requests
- error responses
- requests with different query parameters
- URLs containing session identifiers

Because of this, the **total number of log requests is rarely equal to the PV reported by an analytics system**.

The objective of validation is therefore **not to match total request counts**, but to verify that **page-level aggregation follows the same definition.**

---

# Key Principles for PV Validation

## 1. Do Not Compare Total Request Counts

Raw server request counts include many non-page requests.

Analytics systems typically filter these out using internal rules.  
Therefore, a direct comparison of totals is not meaningful.

---

## 2. Focus on Top Pages

A practical approach is to compare **Top URLs or key pages**.

If the page view counts for major pages match closely, it is likely that the aggregation logic is aligned.

Typical validation targets include:

- Top landing pages
- frequently accessed service pages
- major entry points

---

## 3. Normalize URLs Before Aggregation

The same logical page may appear as different URLs in logs due to:

- query parameters
- session identifiers
- absolute vs relative URLs

A common normalization approach is:

- remove scheme and host
- remove query strings
- remove session identifiers

After normalization, URLs can be aggregated by **path**.

---

# Typical PV Aggregation Rules

When deriving PV from web access logs, the following rules are commonly applied.

### HTTP Methods

Requests using the following methods may be included:

- GET
- POST

In many environments, POST requests may also correspond to page transitions.

---

### Status Codes

The status code range included in PV calculation should be confirmed.

A typical rule may include:

- HTTP status codes from **200 to 599**

Some analytics systems may include certain error responses in page view counts.

---

### Static Resource Filtering

Static files should usually be excluded from PV aggregation.

Typical examples include:

- CSS
- JavaScript
- images
- fonts
- icons

One important detail is that **the filtering should be applied to the path portion of the URL**, not the query string.

For example:

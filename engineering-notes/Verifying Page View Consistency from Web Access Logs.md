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

/downloadFile.do?file=test.jpg

Although the query contains `.jpg`, the path itself represents an application endpoint and may still correspond to a page request.

---

# Recommended Validation Process

A stable validation workflow usually follows these steps.

### Step 1: Confirm Comparison Scope

Ensure that both systems are using:

- the same date range
- the same log source
- the same service scope

---

### Step 2: Derive PV from Access Logs

Apply the defined aggregation rules:

- allowed HTTP methods
- status code range
- URL normalization
- static resource filtering

This produces page-level PV counts derived from raw logs.

---

### Step 3: Compare Top Pages

Compare the following between the two systems:

- Top URLs
- page rankings
- PV counts

Minor differences may occur due to timing or environment differences, but the overall distribution should be consistent.

---

### Step 4: Confirm Status Code Handling

To better understand the analytics system behavior, it can be useful to check whether certain status codes are included in the PV calculation.

This can be done by separating requests into groups such as:

- successful responses
- server errors

and verifying how they affect page counts.

---

# Lessons Learned

The most important insight from PV validation work is that **data consistency problems are usually definition problems rather than counting problems**.

The key is to align the following:

- page definition
- URL normalization rules
- filtering criteria

Once these rules are aligned, the difference between analytics reports and server logs becomes much easier to explain.

---

# Conclusion

Verifying page view metrics using access logs requires a clear understanding of **how page views are defined and filtered**.

Instead of comparing total request counts, a more reliable method is to:

- normalize URLs
- filter non-page resources
- aggregate requests by page
- compare top pages across systems

This approach provides a practical and reproducible way to validate analytics metrics derived from web traffic data.

---

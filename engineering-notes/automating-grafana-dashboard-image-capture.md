# Automating Grafana Dashboard Image Capture (with Image Renderer)

## 1. Problem

In a data reliability platform, dashboards are not just for visualization.  
They often need to be **captured and stored as artifacts** for reporting, monitoring, and downstream usage.

Typical requirements include:

- Generate dashboard images for a specific time range
- Apply filters (e.g., profile_id)
- Capture both full dashboards and individual panels
- Integrate with ETL pipelines

This leads to the need for:

> Automating Grafana dashboard rendering as images (PNG)

---

## 2. Goal

The objective of this implementation was:

- Capture dashboard images based on time range
- Support variable-based filtering
- Generate both dashboard-level and panel-level images
- Integrate with existing data pipelines

---

## 3. Architecture Overview

```text
ETL Pipeline → Data Warehouse → Grafana Dashboard
↑
Render API (Script)
↓
Grafana Image Renderer (Docker)
↓
PNG Image Files
↓
Report / Slack / Storage
```


The key design:

> Grafana and Renderer are separated as independent components

---

## 4. Why Docker?

Grafana image rendering is not a simple API call.  
It requires **Headless Chrome (Chromium)** execution.

This introduces several issues:

- OS-level dependencies
- Font and rendering inconsistencies
- Environment-specific failures

---

### Why Docker was used

1. Environment standardization  
   → Same behavior across local and production

2. Dependency isolation  
   → No need to install Chromium manually

3. Service separation  
   → Renderer runs independently from Grafana

4. Operational stability  
   → Easy restart and recovery

---

## 5. Issues Encountered

### 1) Render API failure

```text
failed to render: net::ERR_CONNECTION_REFUSED
```

---

### 2) Partial "No data" in panels

- Grafana UI → works correctly
- Rendered image → missing data in some panels

---

### 3) Authentication error

```text
Invalid API key
```

---

## 6. Root Cause Analysis

### 1) Incorrect API token

Problem:
- Service account name was used instead of token

Solution:
- Generate a proper token (e.g., `glsa_...`)
- Use it in Authorization header

---

### 2) Docker networking issue

Initial setup:

```text
http://host.docker.internal:3000
```

→ Not accessible from container

---

Solution:

```text
http://<host-ip>:3000
```

→ Works correctly

---

### 3) Time range mismatch (critical)

Problem:
- Render API uses one time range
- Grafana UI uses another

→ Leads to inconsistent results

---

### 4) Timezone mismatch

- Grafana UI: KST (UTC+9)
- Render API: UTC

→ Requires explicit conversion

---

## 7. Solution

### Accurate time conversion

```bash
date -d "2026-02-23 00:00:00 UTC" +%s
date -d "2026-03-09 23:59:59 UTC" +%s
```

Convert to milliseconds before passing to API.

---

## 8. Capture Script

```bash
#!/usr/bin/env bash
set -euo pipefail

GRAFANA_URL="${1:-http://127.0.0.1:3000}"
TOKEN="${2:-}"
DASH_UID="${3:-}"
OUT_DIR="${4:-./grafana_captures}"
FROM="${5:-now-14d}"
TO="${6:-now}"

ORG_ID="${ORG_ID:-1}"
TZ_NAME="${TZ_NAME:-Asia/Seoul}"

mkdir -p "$OUT_DIR"

curl -sS \
  -H "Authorization: Bearer $TOKEN" \
  "$GRAFANA_URL/api/dashboards/uid/$DASH_UID" > "$OUT_DIR/meta.json"

PANEL_IDS=$(jq -r '.. | objects | select(has("id")) | .id' "$OUT_DIR/meta.json")

for PANEL_ID in $PANEL_IDS; do
  URL="$GRAFANA_URL/render/d-solo/$DASH_UID/_?panelId=$PANEL_ID&from=$FROM&to=$TO&tz=$TZ_NAME"

  curl -sS -L \
    -H "Authorization: Bearer $TOKEN" \
    -o "$OUT_DIR/panel_${PANEL_ID}.png" "$URL"
done
```

---

## 9. Best Practices
### 1) Avoid manual timestamp calculation
High risk of mistakes

### 2) Use ISO time format
```text
from=2026-02-23T09:00:00
to=2026-03-10T08:59:59
```

### 3) Always include dashboard variables
var-profile_id=...

Missing variables → "No data"

### 4) Validate Docker networking

```bash
curl http://<host-ip>:3000
```

---

## 10. Key Insight

> Grafana UI and Render API do not behave the same

Key differences:
- Timezone handling
- Timestamp format
- Network path
- Authentication

---

## 11. Data Reliability Perspective

This is not just an automation task.

It is about:

> Ensuring that visualized results are reproducible and trustworthy

Pipeline perspective:
- ETL → data generation
- Grafana → visualization
- Renderer → output artifact

The renderer becomes:

> The final step of data reliability

---

## 12. Conclusion

With this setup, the following became possible:

- Automated dashboard image generation
- Panel-level analysis
- Integration with ETL pipelines
- Stable operation using Docker



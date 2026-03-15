# DWKim Engineering Notes

Data Reliability • Data Drift • Analytics Infrastructure

This site documents engineering notes and architectural ideas related to **data reliability, analytics pipelines, and drift monitoring** in modern digital systems.

---

# About

I focus on improving the reliability of analytics data used in digital services.

Modern digital platforms depend on complex analytics pipelines composed of multiple systems:

Web Applications  
Web Server Logs  
Collectors  
Data Pipelines  
Analytics Warehouses  
Dashboards  

Failures can occur silently between these systems and eventually affect business metrics and decision making.

My work focuses on detecting and controlling these hidden risks in analytics pipelines.

---

# Core Topics

### Data Reliability

Ensuring analytics data remains trustworthy across multiple systems.

Topics include:

- cross-system validation
- analytics data consistency
- pipeline reliability
- metric reconciliation

---

### Data Drift Monitoring

Detecting unexpected statistical changes in operational data.

Examples:

- abnormal user behavior shifts
- sudden metric changes
- distribution drift
- traffic anomalies

Methods explored:

- Population Stability Index (PSI)
- ratio monitoring
- statistical anomaly detection

---

### Data Observability

Monitoring the operational health of analytics pipelines.

Key signals include:

- freshness
- volume
- schema
- pipeline latency

Observability provides early detection of pipeline failures before they affect reporting systems.

---

# Portfolio

Architecture frameworks and design documents related to data reliability and drift monitoring.

Topics include:

- Data Reliability Architecture
- Drift Monitoring Framework
- Cross-System Data Validation
- Data Observability Design

👉 **Open Portfolio**

[Go to Portfolio](/portfolio)

---

# Engineering Notes

Technical investigations and operational experiences from real-world analytics environments.

Examples include:

- WSL filesystem behavior
- web log data consistency issues
- analytics pipeline troubleshooting
- monitoring and validation techniques

👉 **Open Engineering Notes**

[Go to Engineering Notes](/engineering-notes)

---

# Site Structure

This site is organized into two main sections.

Home
│
├── Portfolio
│      ├ Architecture Design
│      ├ Drift Monitoring Framework
│      └ Validation Architecture
│
└── Engineering Notes
├ Linux / WSL
├ Data Pipeline
└ Observability

---

# Contact

LinkedIn  
GitHub
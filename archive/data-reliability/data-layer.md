# Data Layer: From Raw Logs to Reliable Data

## 1. Problem

Most data systems focus on building pipelines and generating metrics.

However, a fundamental question is often overlooked:

> Can the data itself be trusted?

In real-world systems, the following issues frequently occur:

- Missing or incomplete data
- Broken event structures
- Inconsistent identifiers (user / session)
- Distorted metrics due to parsing errors

These problems originate from the very beginning of the pipeline.

> If the Data Layer is incorrect, everything built on top of it becomes unreliable.

---

## 2. Role of the Data Layer

The Data Layer is the foundation of the entire system.

Its purpose is not simply to ingest data, but to:

> Transform raw, unstructured events into reliable and structured data

---

### What the Data Layer enables

- Analytics
- Validation
- Drift detection
- Risk scoring
- ML feature generation

---

## 3. Architecture Overview

```text
Raw Log (Simulator)
↓
ETL / Parsing
↓
TSV (Intermediate)
↓
Bulk Load (DB)
↓
stg_webserver_log_hit
```

---

## 4. Raw Log Simulator

### 4.1 Concept

Instead of relying on real production logs,  
this system uses a **controlled data generation layer**.

> A Synthetic Event Generator

---

### 4.2 Why use a simulator?

Limitations of real log-based systems:

- Dependence on production data
- Difficulty reproducing edge cases
- Limited test scenarios

---

### Solution

Generate controlled patterns:

- Traffic spikes / drops
- Event distribution changes
- Behavioral shifts

→ Enables testing of validation, drift, and risk logic

---

### 4.3 Data Structure

The simulator is based on a behavior-driven model:

- user (pcid)
- session (sid)
- event (evt)
- page type (page_type)
- device / country / user agent
- latency / status

---

### Example log

```text
192.168.0.1 [09/Mar/2026:10:23:45 +0900] "GET /login.do HTTP/1.1" 200 512 "-" "Mozilla/5.0" "pcid=abc123;sid=xyz456;evt=view;page_type=login"
```

---

### Key Design: Parseable Logs

```text
pcid=abc123;sid=xyz456;evt=view;page_type=login
```

Logs are designed from the beginning to align with database schema.

---

### 4.4 Scenario-based generation

The simulator can control:

- Traffic volume
- Page distribution
- Event ratios
- User behavior

---

> It is effectively a **drift generation engine**

---

### 4.5 Unified pipeline design

```text
Simulator → Log → Parser → TSV → DB
```

---

### 4.6 Design characteristics

- Streaming-like processing  
- Deterministic outputs  
- Schema-first design  
- ETL-friendly log format  

---

### 4.7 Key Insight

> Data reliability is determined at the data generation stage, not after parsing

---

## 5. ETL / Parsing Layer

### 5.1 Role

This layer converts raw logs into structured, meaningful data.

---

### 5.2 Processing flow

```text
Raw Log
→ Regex Parsing
→ Timestamp normalization
→ URL normalization
→ Key-Value extraction
→ TSV generation
→ DB Load
```

---

### 5.3 Core transformations

#### 1) Parsing

- IP
- Timestamp
- Method
- URL
- Status
- User Agent
- Key-Value fields

---

#### 2) Normalization

URL normalization:

```text
/login.do?ref=home → /login.do
```

Timestamp normalization:

```text
09/Mar/2026 → 2026-03-09
```

---

#### 3) Key-Value structuring

```text
pcid=abc123;sid=xyz456;evt=view
→ structured columns
```

---

#### 4) Schema mapping

Align parsed data to database schema

---

## 6. TSV Intermediate Layer

### Why introduce TSV?

```text
Log → TSV → DB
```

---

### Benefits

1. Debugging  
   → Inspect intermediate results

2. Reprocessing  
   → Reload without raw logs

3. Performance  
   → Enables bulk loading

---

## 7. Bulk Loading Strategy

### LOAD DATA INFILE

```sql
LOAD DATA LOCAL INFILE 'file.tsv'
INTO TABLE stg_webserver_log_hit
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n';
```

---

### Why this approach?

#### 1) Performance

- INSERT → row-by-row
- LOAD DATA → file-based

→ significantly faster

---

#### 2) Stability

- Reduced transaction overhead
- Lower database load

---

#### 3) Separation of concerns

- Parsing and loading are decoupled

---

## 8. Design Comparison

### Conventional approach

```text
Log → Python → INSERT
```

---

### This system

```text
Log → TSV → Bulk Load
```

---

## 9. Data Quality Perspective

The Data Layer acts as:

> The first filter of data reliability

---

### Common failure cases

- Parsing errors → broken columns  
- URL normalization issues → distorted metrics  
- Missing key-value fields → loss of traceability  

---

## 10. Output Table

### stg_webserver_log_hit

Role:

- Base dataset for all downstream layers

---

## 11. Conclusion

This Data Layer integrates:

- Data generation (Simulator)
- Data structuring (Parsing)
- Data loading (Bulk ingestion)

---

> It is not just an ingestion layer,  
> but a foundation for data reliability.

---

## One-line Summary

> If the Data Layer is wrong, everything above it is wrong.

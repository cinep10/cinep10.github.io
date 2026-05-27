# Data Ingestion Architecture

The current ingestion layer is not designed as a simple ETL ingestion pipeline.

Instead, the architecture focuses on preserving:

```text
Source Provenance
+
Scenario Identity Propagation
+
Behavior Event Lineage Preservation
```

through an:

```text
Operational Reliability Collection Architecture
```

The core objective is not simply loading logs into a database.

It is to make the following traceable:

```text
Which source file
was generated from which scenario/run,
and through what lineage
it became a behavior event raw record
```

---

# Collection Architecture Overview

![Data Generation Architecture](/assets/images/high-level-architecture.png)

---

# Overall Flow

The current collection architecture follows this flow:

```text
Source Log File
→ raw_snapshot_manifest
→ stg_webserver_log_hit
→ stg_wc_log_hit
→ event_log_raw
→ canonical_events
→ canonical_behavior_events
```

This structure is not simply file parsing.

It exists to preserve:

```text
File Provenance
+
Source Identity
+
Behavior Event Lineage
```

through the entire ingestion process.

---

# raw_snapshot_manifest

`raw_snapshot_manifest` is not a row-level log table.

It is a file-level provenance layer that manages:

```text
which source files were collected
```

Its primary responsibilities include:

* source file discovery
* file lineage registration
* source_gen_run_id linkage
* replay/reprocessing reproducibility

Core meaning:

```text
raw_snapshot_manifest
=
File Provenance Layer
```

It is the starting point of the entire downstream lineage chain.

---

# stg_webserver_log_hit

`stg_webserver_log_hit` is the staging layer that materializes W3C source logs into structured rows.

Primary responsibilities:

* W3C parsing
* source row materialization
* scenario identity propagation
* line-level provenance preservation

Source log:

```text
IP - - [timestamp] "GET /..." ...
```

↓

Structured staging row:

```text
ip
ts
method
uri
http_status
cookie
user_agent
```

Additional lineage metadata:

```text
scenario_id
scenario_name
source_generation_scenario
source_gen_run_id
```

is preserved together.

One of the most important architectural principles is:

```text
requested scenario
≠
source generation scenario
```

Example:

```text
scenario_name = source_identity_drift
source_generation_scenario = baseline
```

Meaning:

```text
baseline journey preservation
+
runtime anomaly injection
```

Therefore the collection layer is not simply ingestion.

It is:

```text
Scenario-aware Collection
```

---

# stg_wc_log_hit

`stg_wc_log_hit` is a collector-compatible operational staging layer.

It transforms:

```text
raw source collection
→ collector-compatible operational collection
```

Primary responsibilities:

* visitor/session normalization
* cookie metadata extraction
* identity stitching preparation
* behavior event preparation

Key operational identities include:

```text
pcid
sid
uid
journey_id
cart_id
order_id
payment_id
delivery_id
```

At this stage, raw access logs evolve into:

```text
Operational Behavior Collection
```

---

# event_log_raw

`event_log_raw` is the layer that transforms:

```text
web hit
→ behavior event raw
```

Primary responsibilities:

* event-level raw normalization
* behavior event lineage preservation
* provenance preservation before canonical transformation

Meaning:

```text
event_log_raw
=
Event-level Provenance Layer
```

The architecture intentionally separates:

```text
collector parsing issues
```

from:

```text
canonical transformation issues
```

Therefore the following structure is preserved:

```text
stg_wc_log_hit
→ event_log_raw
→ canonical_events
```

This enables:

* replay
* debugging
* provenance tracing
* parsing validation

across the ingestion pipeline.

---

# source_gen_run_id

One of the most important lineage keys in the architecture is:

```text
source_gen_run_id
```

Its purpose is to identify:

```text
which source generation execution
produced this row
```

This becomes critical because the architecture supports:

* multi-scenario smoke tests
* replay
* pilot runs
* long-term backfill

Meaning:

```text
same date
same scenario
```

may be executed repeatedly.

Therefore:

```text
scenario_name only
```

cannot be treated as the primary lineage authority.

Instead:

```text
source_gen_run_id
=
primary lineage authority
```

is the core design principle.

---

# Architecture Principles

The current ingestion architecture is built on the following principles.

---

## Collection ≠ Simple Ingestion

---

## Collection = Provenance Preservation

---

## Preserve Scenario Identity Propagation

---

## Preserve Source-level Anomaly Provenance

---

## Preserve Behavior Event Lineage

---

# Final Definition

The current ingestion layer is not a conventional ingestion pipeline.

More precisely, it is a:

```text
Scenario-aware
Operational Behavior Collection Architecture
```

that preserves:

```text
Source Provenance
+
Identity Propagation
+
Behavior Event Lineage
+
Replay Reproducibility
```

as an:

```text
Operational Reliability Collection Foundation
```

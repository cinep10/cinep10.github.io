
# Analyzer + Validation + Drift + Risk 전체 파이프라인 구조 정리

## End-to-End Batch Pipeline

```mermaid
flowchart TD
    A[weblog-sim / raw log] --> B[parse_webserver_log_fast.py]
    B --> C[load_tsv_to_db_v2.py]
    C --> D[stg_webserver_log_hit]
    D --> E[collector_a_v2.py]
    E --> F[stg_wc_log_hit]

    F --> G[analyzer_b_v4.py]
    G --> H[metric_value_hh]
    G --> I[metric_value_day]
    G --> J[stg_ds_metric_hh]
    G --> K[stg_ds_metric_hh_wide]

    H --> L[validation_layer_runner_v2.py]
    I --> L
    L --> M[validation_result]
    L --> N[validation_summary_day]

    H --> O[metric_drift_analysis_db_v7.R]
    I --> O
    O --> P[metric_drift_result_r]
    O --> Q[metric_drift_result]

    N --> R[risk_score_runner_v2.py]
    P --> R
    R --> S[data_risk_score_day]

    S --> T[Grafana]
    N --> T
    P --> T
```

## 핵심 체크 포인트

### Analyzer
- `metric_value_hh` 날짜별 row count
- `collector_event_count >= page_view_count`
- `raw_event_count >= collector_event_count`

### Validation
- fail count
- warn count
- mapping quality rules

### Drift
- warn/alert count by date
- high drift metrics by hour

### Risk

# Data Lineage MVP

```mermaid
flowchart LR
    A[stg_wc_log_hit] --> B[metric_value_hh]
    A --> C[metric_value_day]
    B --> D[validation_result]
    B --> E[validation_summary_day]
    B --> F[metric_drift_result]
    B --> G[metric_drift_result_r]
    D --> H[data_risk_score_day]
    E --> H
    F --> H
    G --> H
    H --> I[Grafana Dashboard]
```

## 설명
- `stg_wc_log_hit`: raw/staging log source
- `metric_value_hh`, `metric_value_day`: semantic metric layer
- `validation_*`: logical consistency checks
- `metric_drift_result*`: statistical anomaly signals
- `data_risk_score_day`: operational control metric
- `Grafana`: observability surface
- 일별 risk_score
- warn/alert 전환일

# Data Reliability Architecture

## End-to-End System

```mermaid
flowchart TD
    A[Weblog Simulation\nfinance_bank profile] --> B[Raw Log File]
    B --> C[parse_webserver_log.py]
    C --> D[TSV]
    D --> E[load_tsv_to_db.py]
    E --> F[stg_webserver_log_hit]

    F --> G[collector_a.py]
    G --> H[collector layer tables]

    F --> I[analyzer_b_v4.py]
    I --> J[metric_value_hh]
    I --> K[metric_value_day]
    I --> L[stg_ds_metric_hh]
    I --> M[stg_ds_metric_hh_wide]

    J --> N[validation_layer_runner_v2.py]
    K --> N
    N --> O[validation_result]
    N --> P[validation_summary_day]

    J --> Q[metric_drift_analysis_db_v7.R]
    K --> Q
    Q --> R[metric_drift_result_r]
    Q --> S[metric_drift_result]

    J --> T[ml_feature_drift_psi.py]
    T --> U[ml_feature_drift_result]

    P --> V[risk_score_runner_v2.py]
    R --> V
    V --> W[data_risk_score_day]

    P --> X[risk_score_engine_v2.py]
    R --> X
    U --> X
    X --> Y[data_risk_score_day_v2]

    W --> Z[Grafana Dashboard]
    Y --> Z
```

## Control Flow

```mermaid
flowchart LR
    A[Raw Event Volume] --> B[Validation]
    B --> C[Statistical Drift]
    C --> D[Risk Score]
    D --> E[Alerting / Dashboard]
```

## ML Input Reliability View

```mermaid
flowchart TD
    A[Raw Event / Web Log] --> B[Metric Layer]
    B --> C[Feature Candidates]
    C --> D[Validation Rules]
    C --> E[Feature Drift PSI]
    D --> F[Trusted Input Data]
    E --> F
    F --> G[Model Training / Scoring]
```


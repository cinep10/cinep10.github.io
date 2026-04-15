# Data Reliability Platform — v0.2

## Overview

v0.2 connects streaming-based reliability signals
to a full pipeline including ML prediction and AI-based explanation.

While v0.1 focused on rule-based detection,
v0.2 builds a structured pipeline for learning, prediction, and interpretation.

---

## System Flow

Source / Scenario  
→ Stream Ingestion  
→ Pre-ML Signal  
→ Risk Signal  
→ Truth  
→ Feature  
→ ML  
→ AI  

---

## Core Components

### Source / Scenario Layer
- Synthetic log generation  
- Scenario-based anomaly design  
- Controlled data distribution  

---

### Stream Processing Layer
- Kafka-based ingestion  
- Streaming event processing  

---

### Pre-ML Signal Layer
- Completeness  
- Duplicate  
- Ordering  
- Latency  

---

### Risk Signal Layer
Transforms metrics into operational risk signals.

---

### Truth Layer
Creates supervised learning labels.

---

### Feature Layer
Constructs structured inputs for ML.

---

### ML Layer
- Classification (anomaly type)  
- Regression (risk severity)  
- Prediction storage  

---

### AI Layer
- Incident summary  
- Action recommendation  

---

### Observability
- Stream risk monitoring  
- ML prediction tracking  
- AI output visualization  

---

## Design Principles

### Rule → ML → AI
Rule-based signals are not replaced by ML,
but transformed into learnable structures.

---

### ML uses structured signals, not raw logs
Improves interpretability and stability.

---

### AI acts as an explanation layer
Transforms outputs into human-readable insights.

---

## Key Outcomes

- Structured streaming anomaly signals  
- ML-ready dataset construction  
- Prediction pipeline  
- AI explanation layer  

---

## Limitations

- Stream-focused architecture  
- Limited explainability  
- No domain expansion yet  

---

## One-line Summary

v0.2 extends data reliability into a streaming-based,
ML-integrated, and explainable system.

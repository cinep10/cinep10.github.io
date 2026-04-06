# ML Data Reliability

This portfolio presents the ML and AI extension layers built on top of a Data Reliability system.

In this architecture, ML and AI are not standalone models.  
They operate as upper layers that consume signals generated from the Data Reliability Pipeline.

The overall structure is as follows:

```text
Data Reliability  
→ ML (State Classification)  
→ AI (Interpretation & Action)
```

---

## System Role

The role of this system is clear:

- Transform complex data signals into a single state
- Interpret that state in a human-readable form
- Connect the result to actionable operations

This system completes the following flow:

```text
Signal → Classification → Interpretation → Action
```

---

## 1. ML Layer

The ML Layer classifies the current system state based on data reliability signals.

- [/portfolio/ml-data-reliability/ml-layer](/portfolio/ml-data-reliability/ml-layer)

### Responsibilities

- Classify system state: normal / warning / alert
- Provide probability-based predictions
- Enable automated risk state evaluation

### Characteristics

- Uses reliability signals instead of raw data
- Designed for explainability
- Threshold-based operational classification

### Key Concept

ML does not perform anomaly detection directly.  
It classifies the system state based on already interpreted signals.

---

## 2. AI Layer

The AI Layer interprets ML outputs and reliability signals  
and transforms them into operational insights and actions.

- [/portfolio/ml-data-reliability/ai-layer](/portfolio/ml-data-reliability/ai-layer)

### Responsibilities

- Generate incident summaries
- Provide technical explanations
- Recommend operational actions

### Characteristics

- Integrates ML predictions, root cause, and risk signals
- Produces natural language explanations
- Generates actionable recommendations

### Key Concept

AI is not a decision-making layer.  
It is an interpretation and operational translation layer.

---

## Relationship with Data Reliability

This system is not an independent ML project.

It is tightly coupled with the Data Reliability pipeline:

- Data Reliability → generates signals  
- ML → classifies state  
- AI → interprets and recommends actions  

ML and AI act as **extension layers** of Data Reliability.

---

## System Characteristics

This system is defined by:

- Reliability-driven feature engineering
- Explainable classification structure
- Probability-based decision logic
- Failure-safe AI design (fallback support)
- Operationally oriented architecture

---

## One-line Definition

ML Data Reliability =  
A system that classifies and interprets data reliability signals into operational actions

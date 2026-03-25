# How to Validate Batch Output Data: A Data Reliability Approach

## 1. Problem: "Batch Success ≠ Data Reliability"

In many production systems, we often assume:

> If the batch job succeeds and output files are generated, the data is valid.

However, in reality, the following issues occur frequently:

- File exists but is empty (0 byte)
- Header is missing or broken
- Row count drops significantly
- Unknown values suddenly increase
- Invalid or forbidden values are not filtered
- Inconsistency between main and supporting datasets

In other words:

> **Batch success does not guarantee data reliability.**

---

## 2. Approach: From Validation to Data Reliability Control

Instead of simple validation, we design a **Data Reliability Control framework**.

### Core Flow
```text
File existence check
→ Structure validation
→ Data completeness check
→ Day-over-day change detection
→ Invalid / abnormal value detection
→ Final decision (OK / WARN / FAIL)
```

The key question is:

> **Can we trust this data for downstream usage?**

---

## 3. Risk-Based Design

This is not just a checklist — it is a **risk-driven control system**.

### Key Risk Categories

| Risk Type | Description |
|----------|-------------|
| Availability Risk | File not generated |
| Completeness Risk | Missing data |
| Structural Risk | Broken schema or format |
| Drift Risk | Sudden change compared to previous day |
| Quality Risk | Unknown or invalid values |

---

## 4. Decision Model (OK / WARN / FAIL)

A simple and practical decision model is critical in operations.

| Status | Meaning |
|-------|--------|
| OK | Safe to proceed |
| WARN | Needs manual review |
| FAIL | Immediate action required |

### Priority Rule
```text
OK < WARN < FAIL
```

- Any FAIL → Final FAIL
- No FAIL but WARN exists → Final WARN
- All OK → Final OK

---

## 5. Minimum Validation Checks (Practical)

### 1. File existence

```bash
[ -f file ] && [ -s file ]
```

### 2. Structure validation
```bash
grep '^#Field:' file
```

### 3. Row count
```bash
awk '!/^#/ {count++} END {print count}'
```

### 4. Change detection
```bash
drop_pct = (previous - current) / previous
```

### 5. Invalid / unknown value detection
```bash
grep -i 'unknown'
```
These five checks alone significantly improve production stability.

---

## 6. Key Design Principles
### 1. Use absolute + relative checks together
- Absolute values alone are not enough
- Day-over-day comparison is critical

### 2. Use escalation-based final status
```text
OK → WARN → FAIL
```

### 3. Always persist results

At minimum, store:

- Per-file validation result
- Final status summary
- Historical log (append-only)
- Alert message

---

## 7. What This System Really Is

This is not just a validation script.

> It is a Data Reliability Control System.

Instead of asking:

- "Was the batch successful?"

We ask:

> **"Is this data trustworthy?"**

---

## 8. Extensibility

This approach is not limited to log data.

It can be extended to:

- Financial data validation
- Settlement / transaction systems
- ML feature validation
- Data pipeline observability
- Privacy-sensitive data validation

---

## 9. Conclusion

Batch processing should not be evaluated by execution success.

> **It should be evaluated by data reliability.**

## One-line Summary

> A batch is not successful when it runs —
> it is successful when its data can be trusted.


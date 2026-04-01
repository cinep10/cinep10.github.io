# Semantic Mapping Layer

## Raw Log를 “행동 의미”로 변환하는 계층

---

## 1. 개요

데이터 파이프라인에서 Raw Log는 단순 문자열의 집합이다.

예를 들어:

```
GET /loan/apply.do
GET /auth/otp.do
GET /card/apply.do
GET /transfer/confirm.do
```

이 상태에서는 시스템이 다음을 알 수 없다.

* 조회인지, 시작인지, 제출인지
* 인증 이벤트인지
* funnel의 어느 단계인지

즉, 이 상태의 데이터는 분석이나 의사결정에 직접 사용할 수 없다.

---

## 2. 목적

Semantic Mapping Layer의 목적은 다음과 같다.

> Raw Log를 “의미 있는 행동 이벤트”로 변환하는 것

이 레이어가 없으면:

* raw url은 단순 문자열에 불과하고
* funnel은 정의되지 않으며
* validation / drift / risk는 의미 없는 숫자가 된다

---

## 3. 전체 흐름

```
raw url / method / event
    ↓
event_mapping
    ↓
semantic event 변환
    ↓
funnel_stage / event_type / category 부여
    ↓
mapping coverage 계산
    ↓
validation / risk / action에 반영
```

실제 처리 흐름:

```
stg_webserver_log_hit
    ↓
event_mapping 적용
    ↓
mapped / unmapped 분리
    ↓
mapping_coverage_day 생성
```

---

## 4. event_mapping의 역할

event_mapping은 단순 테이블이 아니라 **룰 기반 사전(dictionary)**이다.

예시:

| url             | method | event_name        | funnel_stage | category |
| --------------- | ------ | ----------------- | ------------ | -------- |
| /auth/otp.do    | GET    | otp_request       | auth         | login    |
| /loan/apply.do  | GET    | loan_apply_start  | start        | loan     |
| /loan/submit.do | POST   | loan_apply_submit | submit       | loan     |

이 매핑을 통해:

```
/auth/otp.do → 인증 이벤트
/loan/apply.do → 신청 시작
/loan/submit.do → 신청 제출
```

로 해석된다.

---

## 5. 왜 중요한가

### 5.1 Funnel 정의의 시작점

funnel 분석은 다음 질문에서 시작된다.

> 시작 대비 제출 비율은 얼마인가?

이를 위해서는 반드시 다음 구조가 먼저 정의되어야 한다.

```
url → event → funnel_stage
```

---

### 5.2 Validation 품질 기준

mapping이 없으면:

* unknown event 증가
* unmapped url 증가
* funnel step 누락

결과적으로 validation warning이 지속 발생한다.

---

### 5.3 Risk Score 입력

mapping coverage는 Risk 계산에 직접 사용된다.

```
mapping 불완전
→ coverage 감소
→ mapping_score 증가
→ risk_score 증가
```

즉 mapping은 단순 보조 정보가 아니라 **Risk 계산의 핵심 입력값**이다.

---

### 5.4 Action Engine 연결

예:

```
funnel_distortion + low coverage
→ mapping 문제 가능성 높음
→ action: mapping_fix
```

즉 action 판단에도 직접적인 영향을 준다.

---

## 6. Mapping Coverage

핵심 지표:

```
mapping_coverage = mapped_events / total_events
```

예시:

| dt         | total  | mapped | unmapped | coverage |
| ---------- | ------ | ------ | -------- | -------- |
| 2026-03-05 | 100000 | 98000  | 2000     | 0.98     |
| 2026-03-06 | 120000 | 90000  | 30000    | 0.75     |

---

### 해석

coverage 감소는 단순 오류가 아닐 수 있다.

* 신규 URL 유입
* 서비스 구조 변경
* 이벤트 정의 누락

즉, 이는 **구조 변화의 신호**일 수 있다.

---

## 7. mapped vs unmapped

### mapped

* 룰에 의해 의미가 부여된 이벤트

### unmapped

* 정의되지 않은 이벤트

예:

* 신규 페이지
* 테스트 endpoint
* 오타
* 구조 변경

---

### 핵심 의미

unmapped 증가는 다음을 의미할 수 있다.

```
단순 누락 ❌
구조 변화 신호 ⭕
```

---

## 8. 다른 레이어와의 연결

### Validation

* unmapped 증가 → validation warning 증가

### Drift

* 특정 시점 unmapped 급증 → drift처럼 보일 수 있음

### Risk

* coverage 감소 → risk 상승

### Root Cause

* funnel_distortion + low coverage
  → mapping issue 가능성

### Action

* mapping_fix 추천

---

## 9. 운영 방식

### 평상시

* mapping_coverage 자동 모니터링

### 이상 발생 시

* unmapped 상위 path 분석
* 신규 path 확인
* 필요한 경우 mapping 추가

---

### 운영 원칙

```
매일 수정 ❌
필요할 때만 보강 ⭕
```

---

## 10. 자동화 vs 수동

### 자동화 가능 영역

* coverage 계산
* unmapped 탐지
* risk 반영

### 자동화 불가능 영역

* 이벤트 의미 정의
* funnel 단계 설계
* 비즈니스 해석

---

### 핵심

```
탐지는 자동화 가능
의미 정의는 반드시 사람이 필요
```

---

## 11. 설계 요약

Semantic Mapping Layer는:

* Raw Log를 Business Event로 변환하고
* 그 변환 품질을 Coverage로 관리하며
* Validation, Drift, Risk, Action의 기반을 제공하는 계층이다

---

## Summary

Semantic Mapping Layer is the foundation of data interpretation.

Without this layer:

* metrics lose meaning
* validation becomes unreliable
* risk becomes misleading

---

## One-line Definition

Semantic Mapping Layer =
raw log를 business event로 번역하고,
그 번역 품질을 coverage로 관리하는 계층

---

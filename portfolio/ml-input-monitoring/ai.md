좋습니다.
이건 그냥 “정리” 수준이 아니라 포트폴리오에서 한 단계 위로 보이게 만드는 핵심 문서입니다.
👉 목표는 “ML 프로젝트”가 아니라 **“AI 기반 데이터 신뢰성 플랫폼 설계자”**로 보이게 만드는 것입니다.

아래는 그대로 블로그/포트폴리오에 들어갈 수 있는 **AI 버전 아키텍처 문서 (기술 상세본)**입니다.

⸻

🧠 AI 기반 데이터 신뢰성 플랫폼 아키텍처 (AI Version)

1. 설계 철학

본 시스템은 단순한 데이터 품질 검증 시스템이 아니라,
데이터 이상 탐지 → 원인 분석 → 대응 추천까지 자동화하는 AI 기반 Observability 플랫폼을 목표로 설계되었다.

핵심 방향은 다음과 같다:
	•	Rule 기반 검증 → ML 기반 예측 → AI 기반 의사결정으로 확장
	•	데이터 품질을 “정적 검증”이 아닌 동적 리스크 관리 문제로 정의
	•	Explainability (설명 가능성)를 시스템의 핵심 축으로 설계

⸻

2. 전체 아키텍처 (AI 관점)

[DATA LAYER]
Raw Log → ETL → stg_webserver_log_hit

        ↓

[METRIC LAYER]
collector / analyzer
→ metric_value_day

        ↓

[SEMANTIC LAYER]
event_mapping → mapping_coverage_day

        ↓

[VALIDATION + DRIFT]
validation_result
metric_drift_result
time_anomaly / correlation_anomaly

        ↓

[RISK ENGINE (Pre-ML)]
risk_score_day_v4 ⭐
→ data_risk_score_day_v4

        ↓

[ROOT CAUSE ENGINE ⭐]
root_cause_result

        ↓

[ACTION ENGINE]
data_reliability_action_day

        ↓

--------------------------------------

[ML FEATURE LAYER]
ml_feature_vector_day

        ↓

[ML MODEL]
ml_risk_model_v3 (multi-class classifier)

        ↓

[ML PREDICTION]
ml_prediction_result

        ↓

[AI EXPLAINABILITY ⭐]
SHAP / Feature Contribution

        ↓

[AI DECISION ENGINE ⭐]
Action Recommendation Model

        ↓

--------------------------------------

[SCENARIO SIMULATION ⭐]
scenario_injector
scenario_experiment_runner

        ↓

[OBSERVABILITY]
Grafana Dashboard


⸻

3. Layer별 상세 설계

⸻

3.1 DATA LAYER

역할
	•	로그 기반 원천 데이터 생성 및 적재
	•	시뮬레이션 환경 포함

구성
	•	weblog_sim (시뮬레이터)
	•	parser / loader
	•	stg_webserver_log_hit

특징
	•	재현 가능한 데이터 환경
	•	시나리오 테스트를 위한 기반

⸻

3.2 METRIC LAYER ⭐

역할
	•	로그를 KPI/지표로 변환

주요 테이블
	•	metric_value_day

생성 지표 예시
	•	daily_active_users
	•	page_view_count
	•	conversion funnel
	•	auth_success_rate

특징
	•	모든 분석의 기준 데이터
	•	downstream 전체 영향

⸻

3.3 SEMANTIC / MAPPING LAYER

역할
	•	raw 이벤트 → 의미 있는 비즈니스 이벤트로 변환

구성
	•	event_mapping
	•	mapping_coverage_day

기능
	•	URL → event name 매핑
	•	funnel stage 정의
	•	unmapped 비율 측정

중요성

👉 데이터 의미 계층 (semantic layer)
👉 이후 ML feature 품질에 직접 영향

⸻

3.4 VALIDATION + DRIFT LAYER

Validation
	•	null / missing
	•	range check
	•	rule 기반 이상 탐지

Drift
	•	distribution shift (R 기반)
	•	baseline 대비 변화 탐지

Structural anomaly
	•	time anomaly
	•	correlation anomaly

⸻

3.5 RISK ENGINE (Pre-ML) ⭐ 핵심

구성
	•	risk_score_day_v4_runner.py

입력
	•	validation_result
	•	metric_drift_result
	•	anomaly 결과

출력
	•	data_risk_score_day_v4

특징
	•	rule + 통계 기반 리스크 통합
	•	deterministic (설명 가능)

⸻

3.6 ROOT CAUSE ENGINE ⭐ (Explainability 핵심)

역할
	•	어떤 metric이 문제인지 분석

출력
	•	root_cause_result

포함 정보
	•	top contributing metrics
	•	영향도 (contribution score)
	•	causal link

👉 실제 운영에서 가장 중요한 레이어

⸻

3.7 ACTION ENGINE

역할
	•	이상 발생 시 대응 방안 제시

출력
	•	data_reliability_action_day

예시
	•	“auth_fail 급증 → 로그인 API 점검”
	•	“funnel drop → 결제 단계 확인”

⸻

3.8 ML FEATURE LAYER

역할
	•	ML 입력 데이터 생성

테이블
	•	ml_feature_vector_day

구성 feature
	•	metric
	•	validation 결과
	•	drift score
	•	risk score
	•	scenario 정보

⸻

3.9 ML MODEL LAYER

모델
	•	ml_risk_model_v3 (multi-class classifier)

목표
	•	risk 상태 예측
	•	normal
	•	warning
	•	alert

현재 특징
	•	초기: fallback 모델
	•	현재: supervised 학습 가능 상태

⸻

3.10 ML PREDICTION LAYER

출력
	•	ml_prediction_result

포함 정보
	•	predicted_label
	•	prob_alert
	•	actual_risk_score

⸻

3.11 AI EXPLAINABILITY ⭐

기술
	•	SHAP (Shapley value)

역할
	•	특정 날짜의 anomaly 이유 설명

출력
	•	feature contribution
	•	explanation 결과

⸻

3.12 AI DECISION ENGINE ⭐

목표
	•	Rule 기반 action → AI 추천으로 확장

입력
	•	predicted risk
	•	root cause
	•	scenario context

출력
	•	action recommendation ranking

⸻

3.13 SCENARIO SIMULATION ⭐ 핵심

구성
	•	scenario_injector
	•	scenario_experiment_runner
	•	scenario_matrix

지원 시나리오
	•	funnel_break
	•	auth_failure
	•	campaign_spike
	•	weather_drop
	•	mixed_incident

특징
	•	anomaly 재현 가능
	•	ML 학습 데이터 생성 핵심

⸻

4. 데이터 흐름 (AI 중심)

로그
 → metric
 → validation + drift
 → risk (rule)

 → root cause
 → action

 → feature vector
 → ML model
 → prediction

 → explainability (SHAP)
 → AI recommendation

 → Grafana


⸻

5. 시스템 특징 (핵심 차별점)

1. End-to-End 구조
	•	데이터 수집 → 이상 탐지 → 대응까지 자동화

2. Explainability 중심 설계
	•	root cause + SHAP 기반 설명

3. Scenario 기반 학습
	•	실제 anomaly 재현
	•	supervised learning 가능

4. Hybrid 구조
	•	Rule + ML + AI 혼합

⸻

6. 현재 상태

영역	상태
Pre-ML	운영 가능
ML	1차 완료 단계
AI	설계 완료, 일부 미구현


⸻

7. 향후 확장 방향

1. Label 고도화
	•	intensity 기반 multi-class

2. AI Explainability 강화
	•	SHAP production 적용

3. Recommendation AI
	•	action 자동 추천 모델

4. Drift-aware ML
	•	자동 재학습

⸻

8. 한 줄 정리 (포트폴리오용 핵심)

👉
“데이터 품질 검증을 넘어, 이상 탐지–원인 분석–대응까지 연결한 Explainable AI 기반 데이터 신뢰성 플랫폼을 설계 및 구현”

⸻

원하시면 다음 단계로:

👉 이 아키텍처를 **다이어그램 (Mermaid / PPT용)**으로
👉 또는 면접용 설명 스크립트 (3분 / 10분 버전)

까지 만들어드릴게요.
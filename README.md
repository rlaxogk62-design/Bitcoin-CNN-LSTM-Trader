# 🚀 Bitcoin 15-Min Scalping Predictor: CNN-LSTM Hybrid Model

## 📌 Overview (프로젝트 개요)
이 프로젝트는 **딥러닝(Deep Learning)과 강화학습(Reinforcement Learning)을 활용한 완전 자동화 비트코인 퀀트 트레이딩 시스템 구축**이라는 장기 목표의 첫 번째 단계로, 단기 수급과 모멘텀을 파악하여 **'다음 15분 뒤의 가격 방향성'**을 예측하는 AI 모델입니다.

초기에는 거시경제 지표 및 통화정책의 흐름이 자산 시장에 미치는 장기적 영향을 반영하여 일봉(Daily) 기준의 예측 모델을 기획했습니다. 그러나 오버나이트 리스크를 최소화하고, 노이즈가 적은 순수 수급(Micro-structure)에 집중하기 위해 **업비트(Upbit) 15분봉 기반의 단기 스캘핑/스윙 예측 모델로 아키텍처를 전면 리팩토링**하였습니다.

## 🛠 Tech Stack (기술 스택)
*   **Data Pipeline:** `pyupbit` (업비트 API)
*   **Machine Learning:** `TensorFlow` / `Keras` (CNN-LSTM Hybrid)
*   **Data Processing:** `Pandas`, `NumPy`, `Scikit-learn` (StandardScaler, Sliding Window)
*   **Visualization:** `Matplotlib`

## 🧠 Model Architecture (모델 구조)
단순한 2차원 시계열 분석을 넘어, 과거의 흐름을 3차원 텐서로 묶어 기억력을 극대화한 **Sliding Window 기법**을 적용했습니다.

1.  **Feature Engineering:** 가격 등락률(Return), 거래량(Volume), 변동성(Volatility), RSI(14) 지표를 정규화(Scaling)하여 사용.
2.  **Sliding Window:** 과거 3시간(15분 캔들 12개)의 수급 흐름을 하나의 Window 묶음으로 생성.
3.  **Conv1D (CNN):** 캔들 2개 단위의 커널(Kernel)이 이동하며 단기적인 급등락 및 거래량 폭발 패턴을 초고속으로 특징 추출.
4.  **LSTM & Dropout:** CNN이 추출한 특징을 넘겨받아 3시간 전체의 수급 트렌드를 기억하며, Dropout(0.2)을 통해 금융 데이터 특유의 노이즈로 인한 과적합(Overfitting) 방지.

## 💡 Trouble Shooting & Insights (핵심 문제 해결 과정)

**1. 거시경제 지표의 지연성 한계 극복**
초기 기획 시 나스닥 지수 및 환율 등의 매크로 지표를 포함하였으나, 실시간 단기 트레이딩에서는 일 1회 업데이트되는 지표들이 오히려 예측력을 떨어뜨리는 노이즈로 작용함을 발견했습니다. 이에 거시 지표를 과감히 제거하고, 해당 15분 동안 쏟아진 **'실제 거래량(Volume)'**과 캔들의 꼬리 길이를 의미하는 **'변동성(Volatility)'**을 핵심 피처로 대체하여 모델의 반응 속도를 극대화했습니다.

**2. 글로벌 평균가(Yahoo Finance)에서 실 체결가(Upbit)로의 파이프라인 전환**
글로벌 평균 데이터는 거시적 분석에 적합하지만 실전 매매의 호가와는 괴리가 발생했습니다. 또한 코랩(Colab) 환경에서의 글로벌 API(ccxt 등) 접속 차단(HTTP 451) 문제를 해결하기 위해, 한국 시장의 김치 프리미엄이 반영된 `pyupbit` 실시간 파이프라인으로 전면 교체하여 데이터의 정합성과 실전 활용도를 높였습니다.

## 📊 Result & Visualization (실행 결과)
*(여기에 코랩에서 출력된 '과거 10시간 실제 차트 + 다음 15분 점선 및 별 모양 예측' 결과 그래프 이미지를 캡처해서 끌어다 놓으세요)*

딥러닝 모델은 단순히 % 수치를 내뱉는 것을 넘어, 실전 매매에 직관적으로 사용할 수 있도록 `현재 가격 × (1 + 예측 %)` 역산출 로직을 적용하여 최종 예상 목표가(KRW)를 차트 위에 시각화하도록 구현했습니다.

## 🚀 Next Step (향후 발전 계획)
현재의 15분봉 예측 모델(방향성 예측)을 환경(Environment)으로 삼아, 최적의 매수/매도/관망 타이밍을 스스로 결정하는 **강화학습(RL) 에이전트 개발 및 연동**을 진행할 예정입니다.

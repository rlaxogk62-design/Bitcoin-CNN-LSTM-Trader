🚀 Bitcoin 15-Min Scalping Predictor: CNN-LSTM Hybrid Model
📌 Overview (프로젝트 개요)
이 프로젝트는 딥러닝(Deep Learning)과 강화학습(Reinforcement Learning)을 활용한 '완전 자동화 비트코인 퀀트 트레이딩 시스템 구축'이라는 장기 목표의 첫 단추임. 단기 수급과 모멘텀을 파악해 '다음 15분 뒤의 가격 방향성'을 예측하는 AI 모델을 개발함.

처음에는 거시경제 지표나 통화정책 흐름이 자산 시장에 미치는 장기적 영향을 반영하려고 일봉(Daily) 기준의 예측 모델을 기획했음. 하지만 오버나이트 리스크를 줄이고, 노이즈가 적은 순수 수급(Micro-structure)에 집중하기 위해 업비트(Upbit) 15분봉 기반의 단기 스캘핑/스윙 예측 모델로 아키텍처를 전면 리팩토링함.

🛠 Tech Stack (기술 스택)
Data Pipeline: pyupbit (업비트 API)

Machine Learning: TensorFlow / Keras (CNN-LSTM Hybrid)

Data Processing: Pandas, NumPy, Scikit-learn (StandardScaler, Sliding Window)

Visualization: Matplotlib

🧠 Model Architecture (모델 구조)
단순한 2차원 시계열 분석을 넘어, 과거의 흐름을 3차원 텐서로 묶어 기억력을 극대화한 Sliding Window 기법을 적용함.

Feature Engineering: 가격 등락률(Return), 거래량(Volume), 변동성(Volatility), RSI(14) 지표를 정규화(Scaling)하여 피처로 씀.

Sliding Window: 과거 3시간(15분 캔들 12개)의 수급 흐름을 하나의 Window 묶음으로 만듦.

Conv1D (CNN): 캔들 2개 단위의 커널(Kernel)이 이동하면서 단기적인 급등락 및 거래량 폭발 패턴을 초고속으로 잡아냄.

LSTM & Dropout: CNN이 뽑아낸 특징을 넘겨받아 3시간 전체의 수급 트렌드를 기억하게 함. 동시에 금융 데이터 특유의 노이즈 때문에 발생하는 과적합(Overfitting)을 막기 위해 Dropout(0.2)을 걸어둠.

💡 Trouble Shooting & Insights (문제 해결 과정 및 인사이트)
1. 거시경제 지표의 지연성 한계 극복
처음 기획할 땐 나스닥 지수나 환율 같은 매크로 지표도 넣었음. 근데 실시간 단기 트레이딩에서는 하루에 한 번 업데이트되는 이런 지표들이 오히려 예측력을 떨어뜨리는 노이즈가 된다는 걸 깨달음. 그래서 거시 지표는 과감하게 다 빼버리고, 해당 15분 동안 터진 '실제 거래량(Volume)'과 캔들의 꼬리 길이를 의미하는 '변동성(Volatility)'을 핵심 피처로 대체해 모델 반응 속도를 극대화함.

2. 글로벌 평균가(Yahoo Finance)에서 실 체결가(Upbit)로 파이프라인 전환
글로벌 평균 데이터(yfinance)는 거시적 분석엔 좋지만, 실전 매매 호가와는 괴리가 컸음. 게다가 코랩(Colab) 환경에서 글로벌 API(ccxt 등) 접속이 차단되는(HTTP 451) 이슈도 겪음. 이를 해결하기 위해 한국 시장의 김치 프리미엄이 그대로 반영되는 pyupbit 실시간 파이프라인으로 전면 교체함. 결과적으로 데이터 정합성과 실전 활용도를 훨씬 높일 수 있었음.

📊 Result & Visualization (실행 결과)
(여기에 코랩에서 출력된 '과거 10시간 실제 차트 + 다음 15분 점선 및 별 모양 예측' 결과 그래프 이미지를 캡처해서 끌어다 놓을 것)

딥러닝 모델이 덜렁 % 수치만 뱉고 끝나는 게 아니라, 실전 매매에서 직관적으로 쓸 수 있도록 신경 씀. 현재 가격 × (1 + 예측 %) 역산출 로직을 직접 짜서 최종 예상 목표가(KRW)를 차트 위에 바로 띄워주게 시각화까지 구현함.

🚀 Next Step (향후 발전 계획)
현재 완성된 15분봉 예측 모델(방향성 예측)을 일종의 환경(Environment)으로 세팅하고, 최적의 매수/매도/관망 타이밍을 스스로 결정하는 강화학습(RL) 에이전트를 개발해 연동하는 것이 다음 목표임.

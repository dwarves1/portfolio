[← 프로젝트 목록](./README.md) | [← 홈으로](../README.md)

# 📈 Deep RL 기반 HFT 마켓 메이킹 시스템

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Gymnasium](https://img.shields.io/badge/Gymnasium-RL_Env-blue?style=flat-square)
![WebSocket](https://img.shields.io/badge/WebSocket-Realtime-green?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

> **GitHub** : [dwarves1/hft-rl-market-making](https://github.com/dwarves1/hft-rl-market-making)

---

## 배경 & 문제 정의

**마켓 메이킹(Market Making)** 은 매수/매도 호가를 동시에 제시해 유동성을 공급하고 스프레드 수익을 얻는 전략입니다. 고빈도 거래(HFT) 환경에서는 수백 밀리초 단위의 의사결정이 요구되며, 전통적인 규칙 기반 전략은 빠르게 변하는 시장 미시구조에 적응하기 어렵습니다.

### 핵심 질문

> **"강화학습 에이전트가 오더북 데이터만으로 최적의 마켓 메이킹 전략을 스스로 학습할 수 있는가?"**

---

## 시스템 구성

```
hft-rl-market-making/
├── hft_env.py              # 커스텀 HFT Gymnasium 환경
├── feature_extractor.py    # 오더북 특징 추출기
├── train.py                # 에이전트 학습 루프
├── dashboard_server.py     # 실시간 대시보드 백엔드 (WebSocket)
└── dashboard_ui.html       # 실시간 대시보드 프론트엔드
```

---

## 핵심 구성 요소

### 1. 커스텀 HFT 환경 (`hft_env.py`)

OpenAI Gymnasium 인터페이스를 준수하는 커스텀 거래 환경.

**상태 공간 (State Space)**
- 오더북 레벨별 호가 & 잔량
- 현재 포지션, 미실현 PnL
- 캔들 기반 가격 모멘텀 지표

**행동 공간 (Action Space)**
- 매수/매도 호가 스프레드 설정
- 주문 수량 결정
- 포지션 청산 트리거

**보상 함수 (Reward Function)**
- 실현 PnL + 재고 위험 패널티
- 과도한 포지션 보유 시 음의 보상

### 2. 특징 추출기 (`feature_extractor.py`)

원시 오더북 데이터를 에이전트가 학습 가능한 형태로 변환.

- 오더북 불균형(Order Book Imbalance) 계산
- 가중 중간가(Weighted Mid-price) 산출
- 롤링 윈도우 기반 변동성 추정

### 3. 학습 파이프라인 (`train.py`)

**PPO (Proximal Policy Optimization)** 기반 에이전트 학습.

- Actor-Critic 아키텍처
- Clipped surrogate objective로 안정적 학습
- 에피소드별 성과 로깅 (PnL, 샤프 비율, 최대 낙폭)

### 4. 실시간 대시보드

WebSocket 기반 실시간 모니터링.

| 차트 | 내용 |
|------|------|
| 캔들차트 | OHLC + 에이전트 매수/매도 시점 표시 |
| PnL 곡선 | 누적 손익 실시간 추적 |
| 포지션 현황 | 현재 보유 포지션 & 위험 노출 |
| 오더북 뎁스 | 매수/매도 잔량 분포 시각화 |

---

## 성과 지표

학습된 에이전트를 백테스트 환경에서 평가:

| 지표 | 설명 |
|------|------|
| 누적 PnL | 총 실현 + 미실현 손익 |
| 샤프 비율 (Sharpe Ratio) | 위험 대비 수익률 |
| 최대 낙폭 (MDD) | 최대 손실 구간 |
| 거래 빈도 | 에피소드당 평균 주문 횟수 |

---

## 기술적 도전 & 해결

### 재고 위험 (Inventory Risk) 관리

한쪽 방향으로 포지션이 쏠릴 경우 큰 손실이 발생. 보상 함수에 **재고 패널티 항** 을 추가해 에이전트가 중립 포지션을 선호하도록 유도.

### 비정상성(Non-stationarity) 문제

금융 시계열 데이터는 분포가 시간에 따라 변함. 롤링 정규화와 적응형 학습률 스케줄러로 대응.

---

[← 프로젝트 목록](./README.md) | [🛡️ Project 1 보기](./project1.md)

[← 프로젝트 목록](./README.md) | [← 홈으로](../README.md)

# 🛡️ Face Anti-Spoofing & 비대면 본인인증 고도화 시스템

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111+-009688?style=flat-square&logo=fastapi&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![InsightFace](https://img.shields.io/badge/InsightFace-ArcFace-blue?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

> **GitHub** : [dwarves1/face-anti-spoofing](https://github.com/dwarves1/face-anti-spoofing)

---

## 배경 & 문제 정의

국내 인터넷전문은행 시장은 연평균 30% 이상 성장하며, 비대면 채널이 핵심 접점이 되었습니다. 그러나 이 구조는 **명의 도용(Identity Spoofing)** 과 **계정 탈취(ATO)** 에 대한 취약점을 구조적으로 확대합니다.

2023년 금감원 통계에 따르면 비대면 금융 사기는 전년 대비 **41% 증가**, 그 진입점의 상당 부분이 취약한 본인인증 단계입니다.

### 이 프로젝트가 해결하는 두 가지 질문

> 1. **지금 카메라 앞에 있는 사람이 실제로 살아 있는가? (Liveness)**
> 2. **그 사람이 신분증 속 인물과 동일인인가? (Face Matching)**

---

## 핵심 기능

### 1. Liveness Detection (라이브니스 탐지)

인쇄된 사진, 모니터 화면, 딥페이크 영상 등 **프레젠테이션 공격(Presentation Attack)** 을 탐지.

- SOTA 딥러닝 모델 적용 (MobileNetV2 기반 Binary Classifier)
- 실시간 웹캠 스트림에서 프레임 단위 추론
- 공격 유형별 탐지: Print Attack, Replay Attack, 3D Mask

### 2. Face Matching (얼굴 매칭)

신분증 사진과 실시간 캡처 얼굴의 **동일인 여부를 정량적으로 판별**.

- **ArcFace (InsightFace)** 임베딩으로 512차원 특징 벡터 추출
- 코사인 유사도 기반 매칭 스코어 계산
- FAR/FRR 트레이드오프 기반 임계값 설정

### 3. FastAPI 기반 REST API 서버

- `/verify` 엔드포인트: 라이브니스 + 매칭 통합 판정
- 비동기 처리로 고성능 응답
- Swagger UI 자동 문서화

---

## 기술적 의사결정

### FAR vs FRR 트레이드오프

| 지표 | 설명 | 비즈니스 영향 |
|------|------|-------------|
| FAR (False Acceptance Rate) | 공격자를 실제 사용자로 오인 | 보안 사고 → 규제 리스크 |
| FRR (False Rejection Rate) | 실제 사용자를 거부 | UX 저하 → 이탈률 증가 |

금융 서비스 특성상 **FAR 최소화에 가중치**를 두되, 임계값을 조정하여 FRR이 허용 수준 이내에서 유지되도록 설계.

---

## 기존 방식 대비 개선

| 기존 방식 | 본 시스템 |
|-----------|-----------|
| 신분증 사진 단순 업로드 | 실시간 라이브니스 탐지 + 얼굴 매칭 |
| 규칙 기반 필터 (높은 오류율) | SOTA ArcFace 임베딩 기반 정량적 유사도 |
| 수동 심사 의존 | 자동화 1차 스크리닝으로 심사 효율화 |
| 정적 임계값 | FAR/FRR 연동 동적 임계값 |

---

## 프로젝트 구조

```
face-anti-spoofing/
├── main.py              # FastAPI 앱 진입점
├── face_handler.py      # 얼굴 탐지 & 매칭 로직
└── README.md
```

---

[← 프로젝트 목록](./README.md) | [📈 Project 2 보기](./project2.md)

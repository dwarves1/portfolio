[← 프로젝트 목록](./README.md) | [← 홈으로](../README.md)

# 🤖 Daily AI Insight — GPT 기반 AI 뉴스 자동 큐레이션

![Next.js](https://img.shields.io/badge/Next.js-14-000000?style=flat-square&logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI_GPT--4o--mini-412991?style=flat-square&logo=openai&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)

> **GitHub** : [dwarves1/daily-ai-insight](https://github.com/dwarves1/daily-ai-insight)

---

## 배경 & 문제 정의

AI 분야는 매일 수백 건의 뉴스가 쏟아집니다. TechCrunch, OpenAI Blog, MIT Technology Review 등 주요 매체를 일일이 확인하는 것은 시간 비용이 크고, 중요도 판단 자체도 어렵습니다.

### 핵심 질문

> **"GPT가 매일 아침, AI 세계에서 가장 중요한 뉴스만 골라 읽기 좋은 형태로 제공할 수 있는가?"**

---

## 시스템 구성

```
daily-ai-insight/
├── frontend/                  # Next.js 14 앱
│   ├── app/                   # App Router
│   ├── components/            # React 컴포넌트
│   └── lib/                   # Supabase 클라이언트 등 유틸
├── backend/
│   ├── agent.py               # RSS 수집 + GPT 큐레이션 메인 로직
│   └── requirements.txt
├── database/                  # PostgreSQL 스키마 (Supabase)
└── .github/workflows/         # GitHub Actions 자동화
```

---

## 핵심 구성 요소

### 1. 백엔드 에이전트 (`agent.py`)

RSS 피드를 수집하고 GPT-4o-mini로 중요도를 분석하는 자동화 파이프라인.

**수집 소스**
- TechCrunch AI 섹션
- OpenAI Blog
- MIT Technology Review

**처리 흐름**
1. `feedparser`로 최신 RSS 항목 수집
2. GPT-4o-mini에 전체 기사 목록 전달 → 중요도 기준 TOP 10 선별
3. 각 기사에 대해 3줄 요약 생성
4. Supabase PostgreSQL에 저장

### 2. 프론트엔드 (`Next.js 14`)

타이포그래피 중심 다크 UI. 이미지 없이 텍스트와 그라디언트만으로 구성한 카드 디자인.

| 기능 | 설명 |
|------|------|
| 날짜별 필터링 | 날짜 선택으로 과거 큐레이션 조회 |
| 태그 필터링 | 주제별 분류 (모델, 규제, 연구 등) |
| 반응형 레이아웃 | 모바일 ~ 데스크탑 최적화 |

### 3. 자동화 (`GitHub Actions`)

별도 서버 없이 GitHub Actions만으로 완전 자동화.

- **스케줄**: 매일 오전 7시 (KST, UTC+9)
- **실행**: Python 에이전트 실행 → DB 저장 → 프론트엔드 자동 반영
- **배포**: Vercel (프론트엔드), GitHub Actions (백엔드 스케줄러)

---

## 기술적 도전 & 해결

### 비용 최적화
GPT-4 대신 **GPT-4o-mini**를 선택해 API 비용을 최소화하면서도 충분한 큐레이션 품질 확보. 프롬프트 설계를 통해 한 번의 API 호출로 TOP 10 선별 + 요약을 동시에 처리.

### 서버리스 아키텍처
별도 백엔드 서버 운영 없이 **GitHub Actions를 스케줄러**로 활용. 인프라 비용 0원으로 완전 자동화 파이프라인 구현.

### 타이포그래피 UI 설계
이미지 의존 없이 텍스트·그라디언트만으로 정보 밀도 높은 카드 UI 구현. 뉴스 콘텐츠 자체가 디자인 요소가 되도록 설계.

---

## 기술 스택 요약

| 영역 | 기술 |
|------|------|
| Frontend | Next.js 14, TypeScript, Tailwind CSS, Lucide React |
| Backend | Python 3.11+, Feedparser, OpenAI API (GPT-4o-mini) |
| Database | PostgreSQL (Supabase) |
| Automation | GitHub Actions |
| Deployment | Vercel (Frontend), GitHub Actions (Scheduler) |

---

[← 프로젝트 목록](./README.md) | [📈 Project 2 보기](./project2.md)

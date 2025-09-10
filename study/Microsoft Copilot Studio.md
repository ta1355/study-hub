# Microsoft Copilot Studio 개념 정리

> 이 문서는 Microsoft Copilot Studio에서 에이전트/봇을 구성할 때 필요한 핵심 개념과 활용 방법을 요약한 기술 문서입니다.

---

## 📌 목차

1. [엔터티 (Entity)](#-엔터티-entity)
2. [변수 (Variables)](#-변수-variables)
3. [슬롯 채우기 (Slot Filling)](#-슬롯-채우기-slot-filling)
4. [조건문 (Condition)](#-조건문-condition)
5. [메시지 노드 및 변형](#-메시지-노드-및-변형)
6. [토픽 간 이동 및 리디렉션](#-토픽-간-이동-및-리디렉션)
7. [Power Automate 통합](#-power-automate-통합)
8. [HTTP 요청 노드](#-http-요청-노드)
9. [생성형 AI 오케스트레이션](#-생성형-ai-오케스트레이션)
10. [대화형 부스팅 시스템](#-대화형-부스팅-시스템)
11. [추가 팁 및 실전 적용](#-추가-팁-및-실전-적용)

---

## 🧠 엔터티 (Entity)

사용자의 문장에서 **의미 있는 정보(숫자, 색상, 제품명 등)** 를 추출하는 구조화된 데이터 단위입니다.

- ### ✔️ 종류

  - **사전 구축 엔터티**: 숫자, 날짜, 색상 등 기본 제공
  - **사용자 지정 엔터티**: 도메인 또는 비즈니스 전용 (예: 제품 유형, 오류 코드)

- ### 🧩 부가 기능

  - **스마트 일치(Smart Matching)**: 오타 자동 수정 및 유사어 자동 인식
  - **동의어(Synonyms)**: 다양한 표현을 하나의 엔터티로 연결 가능

- ### 🔎 예시
  > “레드 커피 머신 50개 주문할게요”
  - `50` → 숫자 엔터티
  - `레드` → 색상
  - `커피 머신` → 제품명 (사용자 지정 가능)

---

## 📦 변수 (Variables)

엔터티 또는 사용자 입력 등을 저장하여 **대화 흐름 제어 및 조건 처리**에 사용됩니다.

- ### ✔️ 변수 종류

  - **시스템(System)**: 날짜, 시간 등 시스템 자동 생성
  - **주제(Topic)**: 현재 토픽에서만 사용 가능한 변수
  - **전역(Global)**: 모든 토픽에서 공유 가능한 변수

- ### 🔄 변수 예시
  - `{Topic.OrderNumber}`
  - `{Global.CustomerAction}`

---

## 🔄 슬롯 채우기 (Slot Filling)

- 사용자의 발화에서 엔터티 값을 **자동 추출**하거나 **질문을 통해 값 확보**
- 이미 변수에 값이 있으면 **질문 건너뛰기 가능** (더 자연스러운 UX)

---

## 🔀 조건문 (Condition)

- 조건을 통해 분기된 대화 흐름 구성
- 예:

  ```PowerFx
  =Global.CustomerAction = 'Cancel'
  ```

- 조건 유형:
  - 값 일치
  - 값 없음 (Empty)
  - 참/거짓 (Boolean)

---

## 💬 메시지 노드 및 변형

- 사용자에게 메시지를 표시
- **메시지 변형(Variation)** 기능으로 자연스러운 응답 가능
  - 한 노드에 최대 15개 문장 등록 가능
  - 랜덤 출력됨

---

## 🧭 토픽 간 이동 및 리디렉션

- 하나의 주제에서 **다른 주제 호출 또는 루프 가능**
- 예: 주문 취소 → `OrderCancellation` 토픽 호출

---

## 🔗 Power Automate 통합

- Microsoft Power Platform과 연결하여 **외부 시스템과의 연동 자동화**
- 예시 활용:
  - SharePoint / Dataverse에서 정보 조회
  - Outlook으로 알림 발송
  - Excel에서 계산 자동 수행
  - 타 API와 데이터 통신

---

## 🌐 HTTP 요청 노드

- API 연동을 위한 **HTTP GET/POST 요청 구성 가능**
- 외부 서비스에서 데이터를 가져와 사용자에게 응답에 포함 가능

```yaml
- kind: HttpRequest
  method: POST
  url: https://api.example.com/orders
  headers:
    Authorization: Bearer {Topic.Token}
  body:
    orderNumber: { Topic.OrderNumber }
```

---

## 🤖 생성형 AI 오케스트레이션

- GPT 기반 응답 생성
- 사용자의 입력 의도에 따라 **자동으로 도구/토픽 선택 및 실행 순서 계획**
- 예: 다중 의도 → 주문 확인 + 재고 조회 → 두 토픽 병렬 실행

---

## 🚀 대화형 부스팅 시스템

- 생성형 AI가 **참조 자료 기반 답변**을 생성하도록 도와주는 기능
- 설정 위치: `생성형 답변 사용` 항목
- 활용 예: PDF/링크 기반의 정보 자동 응답

---

## 🔧 추가 팁 및 실전 적용

- PDF 참조 자료 업로드 시:

  - 최대한 **간결하고 섹션이 잘 정리된 문서** 사용 권장
  - 읽기 어려운 스캔본이나 복잡한 표는 피할 것

- SharePoint 연동 시:

  - SharePoint 목록을 **Power Automate 플로우**를 통해 API처럼 사용 가능
  - 예: 목록에서 주문번호로 행 검색 → Copilot으로 응답 전달

- 복잡한 데이터 처리나 모델 호출은:
  - Power Automate + Azure Function 등의 연계 사용 고려

---

## 📚 참고 링크

- [Microsoft Copilot Studio Docs](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Microsoft Copilot 공식 소개](https://www.microsoft.com/en-us/microsoft-copilot/)

---

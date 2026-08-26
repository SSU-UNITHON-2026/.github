<div align="center">

<img src="./assets/noboss-logo.png" width="100" alt="NOBOSS 로고" />

# NOBOSS

### 독촉하는 사람 없이 굴러가는 팀플

> 팀 프로젝트 설정부터 역할 분배, 진행 관리, 최종 정리까지<br />
> AI와 함께 효율적으로 운영하는 팀 프로젝트 협업 서비스

<a href="https://noboss-fe.vercel.app/">
  <img src="https://img.shields.io/badge/%F0%9F%94%97%20%EC%84%9C%EB%B9%84%EC%8A%A4%20%EB%B0%94%EB%A1%9C%EA%B0%80%EA%B8%B0-NOBOSS-4F6FE7?style=for-the-badge&logoColor=white" alt="서비스 바로가기" />
</a>
<a href="https://noboss-api.kusitms.xyz/swagger-ui/index.html">
  <img src="https://img.shields.io/badge/%EB%B0%B1%EC%97%94%EB%93%9C%20API-Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black" alt="백엔드 API 명세" />
</a>

</div>

---

## 💡 NOBOSS가 해결하는 문제

팀 프로젝트에서는 역할과 일정이 계속 바뀌지만, 이를 정리하고 독촉하는 일은 특정 팀원에게 집중됩니다.

NOBOSS는 프로젝트 문맥을 이해하는 AI를 통해 반복되는 관리 업무를 줄이고, 팀원이 실제 작업과 의사결정에 집중할 수 있도록 돕습니다.

- 자연어로 프로젝트와 업무를 입력
- AI가 역할과 단계별 할 일을 구조화
- 마감 임박 업무와 진행 상황을 한눈에 확인
- AI 변경 제안은 사용자가 승인한 경우에만 반영

> AI가 임의로 데이터를 변경하지 않습니다. 모든 변경은 구조화된 제안과 사용자의 승인 이후에 실행됩니다.

<br />

## 📱 주요 기능

### 1. ⚙️ 팀 프로젝트 설정

<img src="./assets/noboss-01.png" alt="팀 프로젝트 설정 화면" width="100%" />

<br />

### 2. 👥 역할 분배

<img src="./assets/noboss-03.png" alt="역할 분배 화면" width="100%" />

<br />

### 3. 📊 팀플 대시보드

<img src="./assets/noboss-04.png" alt="팀플 대시보드 화면" width="100%" />

<br />

### 4. 📝 팀플 요약 및 정리

<img src="./assets/noboss-05.png" alt="팀플 요약 및 정리 화면" width="100%" />

<p align="center"><strong>진행 중인 모든 팀플의 마감·진행률·지연 위험을 한 곳에서 한눈에 파악할 수 있습니다.</strong></p>

<br />

## 🤖 자연어 기반 프로젝트 관리

```text
사용자
"다음 주 금요일까지 사용자 인터뷰 5명 해야 해"
        ↓
AI
Task 생성에 필요한 단계 · 담당자 · 마감일 추출
        ↓
사용자
구조화된 변경 제안 확인 및 승인
        ↓
NOBOSS
백엔드 재검증 후 프로젝트에 반영
```

승인 대기 제안은 최신 1개만 유지합니다. 새로운 제안이 생성되면 직전 제안은 `SUPERSEDED` 상태가 되고, 최신 `PENDING` 제안만 승인할 수 있습니다.

<br />

## 🔧 AI 연동 구조

```mermaid
sequenceDiagram
    actor User as 사용자
    participant FE as NOBOSS Frontend
    participant BE as NOBOSS Backend
    participant AI as AI Server
    participant DB as PostgreSQL

    User->>FE: 자연어 메시지 입력
    FE->>BE: 프로젝트 문맥과 메시지 전달
    BE->>DB: Project · Task · Pending 조회
    BE->>AI: 사용자 메시지와 현재 문맥 전달
    AI-->>BE: 구조화된 Action Proposal
    BE->>DB: Proposal 저장
    BE-->>FE: 제안 및 승인 필요 여부 반환
    User->>FE: 제안 승인
    FE->>BE: 승인된 제안 적용 요청
    BE->>BE: ActionType · Proposal 재검증
    BE->>DB: 허용된 변경만 반영
    BE-->>FE: 적용 결과 반환
```

| 서버의 역할 | 설명 |
| --- | --- |
| 문맥 구성 | 현재 프로젝트, 전체 업무, 직전 대기 제안을 조회해 AI에 전달합니다. |
| 응답 검증 | AI 응답을 허용된 ActionType과 필수 필드로 검증합니다. |
| 안전한 실행 | 승인된 Proposal만 도메인 변경 메서드를 통해 반영합니다. |
| 중복 방지 | 메시지 잠금 조회로 동일 제안의 중복 반영을 방지합니다. |

<br />

## 🗂️ 프로젝트 둘러보기

| Repository | Description |
| --- | --- |
| [NoBoss-BE](https://github.com/NoBoss-team/NoBoss-BE) | Spring Boot 기반 백엔드 API와 AI Action 처리 |
| [NoBoss-FE](https://github.com/NoBoss-team/NoBoss-FE) | NoBoss 웹 프론트엔드 |
| [NoBoss-AI](https://github.com/NoBoss-team/NoBoss-AI) | 자연어 이해와 구조화된 Action Proposal 생성 |

### 주요 API

전체 요청·응답 스펙은 [Swagger API 명세](https://noboss-api.kusitms.xyz/swagger-ui/index.html)에서 확인할 수 있습니다.

```text
POST   /api/v1/projects
GET    /api/v1/projects/{projectId}
GET    /api/v1/projects/{projectId}/tasks
POST   /api/v1/projects/{projectId}/messages
POST   /api/v1/projects/{projectId}/messages/{messageId}/apply
```

<br />

## 👥 팀원

<table align="center">
  <tr>
    <th align="center">Backend</th>
    <th align="center">AI</th>
    <th align="center">Frontend</th>
  </tr>
  <tr>
    <td align="center"><img src="https://github.com/kmg22.png" width="140" alt="김민경" /></td>
    <td align="center"><img src="https://github.com/dddyoung2.png" width="140" alt="김도영" /></td>
    <td align="center"><img src="https://github.com/sunwoo07.png" width="140" alt="박선우" /></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/kmg22">김민경</a></td>
    <td align="center"><a href="https://github.com/dddyoung2">김도영</a></td>
    <td align="center"><a href="https://github.com/sunwoo07">박선우</a></td>
  </tr>
</table>

<br />

<div align="center">

### 독촉은 줄이고, 협업은 선명하게.

</div>

<p align="center">
  <sub>Make it useful. Make it kind.</sub>
</p>
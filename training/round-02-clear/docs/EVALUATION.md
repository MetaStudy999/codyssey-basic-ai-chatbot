# B7-1 Round 02 — Evaluation

> **현재 Mission ID:** B7-1  
> **미션명:** 웹 기반 AI 챗봇 서비스 개발 프로젝트  
> **Round:** Round 02  
> **Repository:** `MetaStudy999/codyssey-basic-ai-chatbot`

## 문서 성격과 기준

이 문서는 기존 Round 01의 `training/round-01-clear/docs/evaluation-qa.md`에 정리된 **Evaluation Q&A Reference**를 Round 02 수행·평가 준비용 체크리스트로 정식화한 문서다.

중요:

- 이 문서는 **제2기 공식 평가 원문을 새로 발견하거나 복원한 문서가 아니다.**
- 제2기 현재 Mission PDF와 Term Project 평가 기준이 항상 최우선이다.
- Round 01의 Q&A는 설명·검증 준비를 위한 Reference로 사용한다.
- Round 01의 과거 PASS/Evidence를 Round 02의 실제 PASS/Evidence로 대신하지 않는다.
- 실제 Runtime, Verification, Evidence가 확보된 항목만 Round 02 평가 준비 완료로 표시한다.

평가 준비 연결:

```text
Evaluation Preparation
→ Requirement
→ Implementation
→ Verification
→ Evidence
→ Evaluation Explanation
```

답변은 가능하면 다음 구조로 준비한다.

```text
WHAT(무엇)
→ WHY(왜)
→ HOW(어떻게)
→ VERIFY(어떻게 검증했는가)
→ LIMITATION(한계는 무엇인가)
```

---

## 항목 1 — 전체 서비스 흐름과 실제 동작

- [ ] Browser form → FastAPI route → session user 확인 → input validation → same-user 최근 대화 조회 → server-side AI API → answer 수신 → DB 저장 → HTML Q/A 확인의 전체 요청 흐름을 본인 구현 기준으로 설명할 수 있는가?
- [ ] 외부 배포 URL에서 `signup → login → chat → AI answer → log view`가 끝까지 정상 동작하는지 실제로 검증할 수 있는가?
- [ ] 외부 배포 환경에서 Secret 설정, DB persistence, platform logs를 함께 확인할 수 있는가?

### 설명 준비

핵심 흐름:

```text
Browser
→ FastAPI
→ 인증된 사용자 확인
→ 입력 검증
→ 사용자별 Context 조회
→ Server-side AI API
→ 응답 수신
→ DB 저장
→ Browser 출력
```

---

## 항목 2 — 인증(Authentication)과 접근 제어(Access Control)

- [ ] 인증(Authentication)이 “사용자가 누구인지 확인하는 과정”이고 접근 제어(Access Control)가 “그 사용자가 어떤 기능을 사용할 수 있는지 결정하는 과정”이라는 차이를 설명할 수 있는가?
- [ ] 로그인하지 않은 사용자가 `/chat`, `/logs` 같은 보호 경로에 접근할 때 어떤 방식으로 차단되는지 설명할 수 있는가?
- [ ] Session Cookie에서 현재 사용자를 찾는 흐름을 설명할 수 있는가?
- [ ] 다른 사용자의 대화가 섞이지 않도록 Query가 현재 Session의 `user.id`에 묶여 있는 이유와 동작을 설명할 수 있는가?
- [ ] `/logs`가 외부 `user_id` 입력이 아니라 현재 Session 사용자 기준으로 조회되어야 하는 이유를 설명할 수 있는가?

---

## 항목 3 — Password와 Session Token 보호

- [ ] Plain Password를 저장하지 않는 이유를 설명할 수 있는가?
- [ ] Random Salt + PBKDF2-HMAC-SHA256을 사용한 Password Hash 저장 흐름을 설명할 수 있는가?
- [ ] 로그인 시 같은 Salt/Iteration 조건으로 Hash한 뒤 Constant-time Compare를 사용하는 흐름을 설명할 수 있는가?
- [ ] Browser에는 Random Session Token을 주고 DB에는 SHA-256 Token Hash만 저장하는 이유를 설명할 수 있는가?
- [ ] DB가 유출되었을 때 Active Session Token 자체가 노출되는 위험을 줄이기 위한 설계임을 설명할 수 있는가?

---

## 항목 4 — AI API와 Secret 보호

- [ ] AI API 호출을 Browser JavaScript가 아니라 Server에서 수행하는 이유를 설명할 수 있는가?
- [ ] API Key를 Browser로 보내지 않고 Server Environment에서 읽는 이유를 설명할 수 있는가?
- [ ] 질문·답변 전체 또는 API Key를 운영 로그에 남기지 않아야 하는 이유를 설명할 수 있는가?
- [ ] 운영 로그에는 사용자 ID, 입력 길이 등 필요한 Metadata 중심으로 기록하는 이유를 설명할 수 있는가?

---

## 항목 5 — Context 유지와 사용자 데이터 격리

- [ ] 같은 `user_id`의 최근 6개 Q/A를 시간순으로 복원해 현재 질문과 함께 AI API에 전달하는 Context 전략을 설명할 수 있는가?
- [ ] 모든 과거 대화를 무한히 넣지 않고 최근 Context를 제한하는 이유를 Context 크기와 비용 관점에서 설명할 수 있는가?
- [ ] 사용자별 Query 조건 때문에 서로 다른 사용자의 대화가 섞이지 않는 흐름을 실제 코드 또는 DB 조회 기준으로 보여줄 수 있는가?

---

## 항목 6 — 입력 검증과 AI API 장애 처리

- [ ] 질문을 Trim한 뒤 빈 값 또는 1000자 초과 입력을 AI API 호출 전에 차단하는 이유를 설명할 수 있는가?
- [ ] 잘못된 입력을 400 응답으로 안내하는 흐름을 설명할 수 있는가?
- [ ] AI API의 HTTP / Network / Timeout / Response Format 오류를 `AIServiceError`와 같은 서비스 오류로 변환하는 이유를 설명할 수 있는가?
- [ ] AI API 장애가 발생해도 Process 전체가 종료되지 않고 Route가 오류를 처리하여 사용자에게 안내하도록 한 이유를 설명할 수 있는가?
- [ ] AI API 실패 상황에서 502 응답과 사용자 안내가 어떻게 연결되는지 설명할 수 있는가?

---

## 항목 7 — DB 저장과 운영 로그

- [ ] DB 로그의 최소 추적 필드인 `user_id`, `created_at`, `question`, `answer`의 역할을 설명할 수 있는가?
- [ ] 어떤 사용자가 언제 어떤 질문을 했고 어떤 답을 받았는지 추적할 수 있도록 이 필드를 저장하는 이유를 설명할 수 있는가?
- [ ] 운영 로그에서 최소 다음 Event를 구분할 수 있는가?
  - `request received`
  - `AI call`
  - `AI response received` 또는 `AI call failed`
  - `DB save success` 또는 `DB save failed`
- [ ] 실제 Runtime에서 주요 Event가 로그에 기록되는지 검증할 수 있는가?

---

## 항목 8 — SQLite 선택 이유와 확장 한계

- [ ] Round 01 Reference에서 SQLite를 선택한 이유를 설치 단순성, SQL/관계/영속성 확인 관점에서 설명할 수 있는가?
- [ ] Multi-instance Production 환경에서는 Shared Persistent DB가 필요할 수 있다는 한계를 설명할 수 있는가?
- [ ] Managed DB로 확장할 수 있다는 방향과, 현재 SQLite 구조와의 차이를 설명할 수 있는가?

---

## 항목 9 — 팀 협업 Evidence

- [ ] Branch, PR, Team Member Commit 기준은 실제 협업 행동의 Evidence라는 점을 설명할 수 있는가?
- [ ] AI가 미리 만든 코드나 문서 Commit을 팀원별 실제 작업으로 환산하면 안 되는 이유를 설명할 수 있는가?
- [ ] Round 02에서 실제 팀 활동이 요구되는 경우, 실제 Branch / Commit / PR / Review Evidence를 제시할 수 있는가?

---

## 항목 10 — Round 02 실제 검증

다음은 Round 02에서 실제로 다시 확인한다.

- [ ] Signup 동작
- [ ] Login 동작
- [ ] 보호 Route 접근 제어
- [ ] Chat 입력 검증
- [ ] AI API 정상 응답
- [ ] AI API 실패 처리
- [ ] 사용자별 Context 격리
- [ ] DB 저장
- [ ] Log View
- [ ] 운영 로그
- [ ] Secret/API Key 비노출
- [ ] 외부 배포 End-to-End 동작
- [ ] 실제 팀 협업 요구가 있다면 관련 Evidence 확보

---

## 평가 설명 준비

각 핵심 항목에 대해 다음 세 가지 길이의 답변을 준비한다.

### 10초 답변
핵심 결론과 구현 위치를 한두 문장으로 설명한다.

### 30초 답변
`WHAT → WHY → HOW → VERIFY` 순서로 핵심 구현과 검증을 설명한다.

### 1분 답변
`WHAT → WHY → HOW → VERIFY → LIMITATION` 순서로 설계 이유, 구현, 실제 검증, 한계와 확장 방향까지 설명한다.

---

## Evidence 연결표

Round 02 실제 수행 시 아래 표를 채운다.

| 평가 영역 | 구현 위치 | Verification | Evidence | 상태 |
|---|---|---|---|---|
| 전체 요청 흐름 | TBD | TBD | TBD | ⬜ |
| 인증·접근 제어 | TBD | TBD | TBD | ⬜ |
| Password 보호 | TBD | TBD | TBD | ⬜ |
| Session Token 보호 | TBD | TBD | TBD | ⬜ |
| AI API·Secret 보호 | TBD | TBD | TBD | ⬜ |
| Context 유지·사용자 격리 | TBD | TBD | TBD | ⬜ |
| 입력 검증 | TBD | TBD | TBD | ⬜ |
| AI 장애 처리 | TBD | TBD | TBD | ⬜ |
| DB 저장 | TBD | TBD | TBD | ⬜ |
| 운영 로그 | TBD | TBD | TBD | ⬜ |
| SQLite 설계·한계 | TBD | TBD | TBD | ⬜ |
| 팀 협업 | TBD | TBD | TBD | ⬜ |
| 외부 배포 | TBD | TBD | TBD | ⬜ |

---

## 최종 Evaluation Ready 조건

다음 조건을 만족하기 전에는 Round 02 `EVALUATION READY`로 표시하지 않는다.

- [ ] 제2기 현재 Mission PDF와 Term Project 요구사항을 다시 확인했다.
- [ ] 공식 요구사항과 이 문서의 Evaluation Preparation을 구분할 수 있다.
- [ ] Round 02 실제 Runtime을 수행했다.
- [ ] 핵심 기능 Verification이 PASS했다.
- [ ] Round 02 실제 Evidence가 연결되어 있다.
- [ ] Secret, Token, Password, Private Key가 Evidence에 노출되지 않았다.
- [ ] 핵심 설계를 자기 말로 설명할 수 있다.
- [ ] 실제 협업 요구가 있다면 실제 협업 Evidence가 존재한다.
- [ ] 외부 배포가 요구된다면 실제 배포 End-to-End 흐름을 검증했다.
- [ ] 모의평가에서 핵심 질문에 답할 수 있다.

---

## 출처

정식화 Source:

`training/round-01-clear/docs/evaluation-qa.md`

Source의 14개 Evaluation Q&A를 Round 02 평가 준비 구조로 재배치했다. Source에서 지원하지 않는 내용을 공식 평가기준으로 추가하지 않는다.

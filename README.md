> **2026-10-25 신규 미션 작성 경로(B7-1):** [`training/round-04-agent/`](training/round-04-agent/README.md) — 신규 코드는 `training/round-04-agent/04-src/`에만 작성합니다. 기존 Round 01~03 및 루트의 내용은 과거 수행·참고 이력으로 보존합니다. **현재 새 Round는 PLAN_ONLY / NOT_STARTED이며 사용자 개발 승인 전 코드 실행·수정·배포 금지**입니다. 중앙 기준: [15개 미션 신규 실행 허브](https://github.com/MetaStudy999/codyssey-basic/tree/main/training/round-04-agent).

# Codyssey Basic B7-1 — Web AI Chatbot

## 구분

- 필수 Term Project (REQUIRED)
- Round: **R01 — CLEAR**
- Reference Build: **CORE READY**
- Runtime Mission: ⬜ NOT STARTED

## 시작 위치

`training/round-01-clear/BEGINNER-GUIDE.md`

## 공식 원본

- `b7-1-mission.pdf`
- `b7-1-mission.md`

현재 저장소에는 별도 공식 Evaluation 파일이 없습니다. Mission 요구사항 자체를 최종 검증 기준으로 사용합니다.

## Reference 핵심

FastAPI + Jinja2 + SQLite 기반으로 회원가입/로그인, 로그인 사용자 전용 AI Chat, 서버 측 AI API 호출, 최근 사용자 대화 Context, 사용자별 Conversation 저장/조회, 운영 로그와 오류 처리를 준비합니다. 실제 팀 branch/PR/팀원별 10회 이상 유의미한 commit과 외부 배포는 Runtime에서 증빙합니다.

## R01 흐름

`Mission → 인증 → AI API → Context → DB/Logs → 팀 협업 → 외부 배포 → Evidence → CLEAR`

실제 AI API, 팀 협업 기록, 외부 URL, Runtime Evidence가 완료되기 전에는 `✅ CLEAR`로 변경하지 않습니다.

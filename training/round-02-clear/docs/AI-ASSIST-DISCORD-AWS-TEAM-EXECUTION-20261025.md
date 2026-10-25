# B7-2 — Discord + 개인 AWS 기반 AI Assist 팀 프로젝트 고도화 실행계획
**계획 상태:** APPROVED_DIRECTION / RUNTIME_NOT_VERIFIED  
**작성일:** 2026-10-25 KST  
**공식 미션:** B7-2 웹 기반 AI 챗봇 서비스 고도화 프로젝트 (선택 Term Project)  
**선행 미션:** B7-1 웹 기반 AI 챗봇 서비스 개발 프로젝트 (필수)  
**개발환경:** Agent Codyssey MCP Expansion V4.0 / `AC-MASTER-V4.0-20261024`  
**원칙:** 기존 공식 B7-2 요구와 저장소, Round 02 기준을 바꾸지 않고 재사용한다.

## 1. 구현 목표와 선행조건
B7-1의 실제 검증된 FastAPI 챗봇을 기반으로, **사용자별 대화 권한·게시판 REST CRUD·풀스택 UI·외부 AWS 배포**를 완성한다. Discord는 팀 요구사항/상태/리뷰를 AI Assist와 연결하는 협업 인터페이스이며, B7-2 공식 웹서비스와 증빙을 대신하지 않는다.

B7-1에 연결된 계획: [B7-1 AI Assist 팀 협업 계획](https://github.com/MetaStudy999/codyssey-basic-ai-chatbot/blob/plan/b7-ai-assist-discord-aws-20261025/training/round-02-clear/docs/AI-ASSIST-DISCORD-AWS-TEAM-EXECUTION-20261025.md)

## 2. 공식 B7-2 필수 검증 경로
1. **인증:** 회원가입, 비밀번호 해시, 로그인 토큰(JWT 등), 로그아웃, 비로그인 핵심 기능 접근 거부.
2. **사용자별 AI 대화:** `User → ChatSession → Message` 소유권 보장; 다른 사용자의 메시지/세션은 403/404; 서버에서만 AI API 호출.
3. **게시판:** 목록/상세/작성/수정/삭제 REST API, 작성자 외 수정·삭제 차단.
4. **풀스택 UI:** Frontend→FastAPI→DB/AI API의 실제 통신. 프론트는 로그인 상태·대화 세션·게시판을 표시.
5. **외부 AWS 배포:** URL에서 회원가입 → 로그인 → 채팅 → 게시판 동작 확인; HTTPS·환경변수/비밀정보 점검.
6. **기술 문서:** ERD(엔터티 관계도), API 명세(경로·메서드·입출력·상태코드), 시스템 구조도, README·환경변수 예제.
7. **협업:** `feature → develop → main` 및 실제 Pull Request(PR, 병합 요청) 기록; 각 팀원이 의미 있는 커밋 10회 이상, 역할·기여 문서가 Git 이력과 일치해야 함.
8. **제출·발표:** GitHub, 외부 URL, 실측 검증/스크린샷, 발표 PPT/PDF 및 발표 대본. 2기 미션 PDF가 최우선이다.

## 3. AI Assist가 Discord에서 수행할 지원 업무
```text
팀원: /assist 또는 /review 또는 /status 또는 /decision
  → Discord 앱/봇의 서명 검증·채널 제한
  → 권한 검증 후 요청·작업 ID 기록
  → AI Assist: 요구사항·결정·담당 업무·검증 항목 분류
  → ChatGPT/Agent Codyssey MCP/Codex: 허용된 개발과 로컬 시험 지원
  → 기존 GitHub 미션 저장소: Issue/기능 브랜치/PR/증빙
  → 팀 책임자의 리뷰·병합/배포 승인
  → Discord 상태·검증 근거·다음 조치 응답
```
Discord가 전송한 일반 텍스트는 **비신뢰 입력**이다. 명령 삽입·프롬프트 인젝션을 포함할 수 있으므로, 자동 셸 실행이나 Git merge/클라우드 배포의 직접 권한으로 취급하지 않는다.

**초기 최소 기능:** 팀원이 명시적으로 호출하는 슬래시 명령의 요청/작업 상태 반환만. 전체 채널 메시지나 개인 DM 자동 수집은 팀 동의·관리자 승인 및 필요한 권한을 갖추기 전까지 하지 않는다. ChatGPT가 현재 Discord에 직접 참여·상주하고 있다는 주장 금지.

## 4. 개인 AWS 계정 운영 제약
- 최소한의 개인 AWS 리소스만 사용. B7-1과 동일한 인증·비밀정보·배포 기준을 재사용하고 필요 시 리소스 공유 여부를 설계한다.
- 새 AWS 리소스(EC2, API Gateway, Lambda, DB 등) 유료 생성/변경·IAM 확대·배포는 Owner 명시 승인 전 **DEFERRED**.
- 예산 알림은 비용 상한을 강제로 보장하지 않으므로 비용 경보, 조회·제거 책임자, 리전·리소스 목록을 기록한다.
- 실서비스 비밀키는 서버 환경변수/비밀 저장소에 보관. Discord Bot token/AI API key/AWS 자격증명을 GitHub·Discord 메시지·ChatGPT 증빙에 게시하지 않는다.

## 5. 역할과 책임
| 실제 팀 역할 | 실제 기여·검증 | AI Assist 담당 지원 |
|---|---|---|
| 팀장/PM | 업무 승인·의견 조정·완료 승인 | 회의 요약·이슈 추적 |
| 인증/백엔드 | 로그인·보안·REST API | 코드 초안·시험 |
| AI/DB | LLM API·세션/메시지 저장·데이터 소유권 | 설계·회귀시험 |
| 프론트엔드 | 대화/게시판 UI, 실제 UX 시험 | UI 코드 지원 |
| DevOps/QA | 개인 AWS 배포·보안·비용·증빙 | 사전 점검·오류 분석 |
팀 인원에 따라 겸임 가능. AI가 만든 커밋을 팀원 본인의 유의미한 커밋으로 거짓 기록하지 않으며, 각 팀원의 이해·검토·기여를 평가 증빙과 연결한다.

## 6. 단계별 실행·완료 게이트
| 단계 | 할 일 | 완료 증거 | 현재 |
|---|---|---|---|
| P0 | B7-1 검증된 기준·채팅 기능 확보 | 공식 B7-1 요구와 테스트 | NOT_VERIFIED |
| P1 | 두 사용자 인증/대화 소유권 | 사용자 교차 접근 403/404 E2E | NOT_VERIFIED |
| P2 | 게시판 작성자 소유권·CRUD | API/브라우저/DB 검증 | NOT_VERIFIED |
| P3 | Discord AI Assist 최소 업무 흐름 | 허가된 Slash Command → 상태 회신 | DEFERRED (앱/봇 설정) |
| P4 | PR 협업과 팀원 실기여 | GitHub 이벤트·브랜치·실제 커밋 | NOT_VERIFIED |
| P5 | AWS 배포·실제 URL·비용 검증 | 공개 URL E2E, 비용/리소스 근거 | DEFERRED (Owner 승인) |
| P6 | ERD/API/슬라이드/평가 | 제출물·실제 시연·모의평가 | NOT_VERIFIED |

## 7. 일정·우선순위
2026-10-25 **17:00 KST 이전**에는 기존 MCP 핵심 연결을 정리하고, **17:00 이후 예정된 B1-1 실증과 슬라이드**를 우선한다. B7-1·B7-2 팀 실증은 팀의 실제 기여 시간 및 Discord/AWS 사용 승인이 확보되는 대로 단계적으로 착수한다. 내부 초압축 B7-2 목표 18시간은 예측치일 뿐 공식 120시간이나 평가 요건 축소·CLEAR 보장이 아니다.

**원칙:** 미션 수행 도중 필요한 MCP 기능만 보완. 기존 68개 MCP 후보를 모두 완성할 때까지 B7 수행을 중단하지 않는다. 단, 배포/인증/팀 협업 등의 공식 요구는 실제 증거 없이 PASS/CLEAR 선언하지 않는다.

## 8. 현 상태
`B7_2_TEAM_EXECUTION_PLAN=DOCUMENTED`; `DISCORD_BOT_LIVE=NOT_VERIFIED`; `AWS_PERSONAL_DEPLOYMENT=DEFERRED`; `B7_2_RUNTIME=CLEAR_NOT_VERIFIED`.

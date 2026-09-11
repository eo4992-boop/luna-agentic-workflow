# Luna v2 — 새 프로젝트 시작

새 프로젝트를 Luna Agentic Workflow v2로 시작할 때 사용하는 표준 진입점이다.

## 한마디로 시작하기

새 ChatGPT 대화에서 프로젝트 GitHub 저장소 URL과 함께 다음 한 줄만 입력한다.

> **Luna v2로 이 저장소를 시작해줘: `https://github.com/<owner>/<repo>`**

Workflow 저장소를 명시해야 하는 환경에서는 다음 한 줄을 사용한다.

> **Luna v2로 이 저장소를 시작해줘: `https://github.com/<owner>/<repo>` — Workflow: `https://github.com/eo4992-boop/luna-agentic-workflow`**

## 시작 시 ChatGPT가 해야 할 일

1. 이 저장소의 `WORKFLOW.md`를 읽고 Luna v2 규칙을 적용한다.
2. 대상 프로젝트 저장소의 현재 GitHub 상태를 확인한다.
3. 프로젝트의 `AGENTS.md`, `README`, 기여 규칙, 스킬/자동화 규칙, CI 설정 등 기존 규칙을 먼저 확인한다.
4. `CONTEXT → INTENT → SPEC → PLAN` 순서로 진행한다.
5. 결과를 바꿀 수 있는 불확실성만 사용자에게 질문한다.
6. 구현 전에 승인 또는 결정이 필요한 사항을 명확히 한다.
7. 구현 후 `5-A Gate`를 실행하고, 실패하면 구현 단계로 제한된 횟수만큼 되돌아간다.
8. `5-B Independent Review`를 별도로 수행한다. 구현자가 스스로 완료 판정을 독점하지 않는다.
9. 검증 근거가 있는 결과만 완료로 선언한다.
10. 중요한 결정과 새 규칙은 프로젝트의 지속 가능한 Context에 반영한다.

## 새 프로젝트에서 중요한 원칙

- 원본 `agentic-workflow-playbook`을 매번 다시 분석하지 않는다.
- Claude Code 전용 절차를 다시 가져오지 않는다.
- Luna v2는 방법론이고, 실제 프로그램은 별도의 프로젝트 저장소에서 관리한다.
- 기존 프로젝트의 규칙과 구조를 함부로 Luna 규칙으로 덮어쓰지 않는다.
- 저장소에 이미 존재하는 작업을 보존하고, 현재 상태를 확인한 뒤 변경한다.
- 테스트나 검증을 실제로 실행하지 않았다면 실행했다고 말하지 않는다.

## 프로젝트 초기화 후

필요한 경우 대상 프로젝트에 `LUNA.md` 또는 `.luna/`와 같은 프로젝트 전용 Context를 만들 수 있다. 단, Luna v2가 특정 디렉터리 구조를 모든 프로젝트에 강제하지는 않는다.

Luna v2의 상세 규칙은 `WORKFLOW.md`가 최종 기준이다.

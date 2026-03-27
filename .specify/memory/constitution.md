<!--
SYNC IMPACT REPORT
==================
Version change: (template) → 1.0.0
Action: Initial constitution — all placeholders replaced with project-specific content.

Modified principles: N/A (first version)

Added sections:
  - Context (project-level metadata)
  - I. 추적 가능성 (Traceability)
  - II. 역할 기반 접근 (RBAC)
  - III. TC 수명주기 (TC Lifecycle)
  - IV. 실행 상태 (Execution Workflow)
  - V. 버저닝 (Versioning)
  - VI. 확장성과 단순성 (Scalability & Simplicity)
  - VII. 연동 표준화 (Integration Standards)
  - Security & Local Data
  - Governance

Removed sections: N/A

Templates requiring updates:
  ✅ .specify/templates/plan-template.md — Constitution Check 섹션이 동적 참조 방식으로 호환됨. 수정 불필요.
  ✅ .specify/templates/spec-template.md — Key Entities·Functional Requirements 섹션이 수명주기·RBAC 원칙 수용 가능. 수정 불필요.
  ✅ .specify/templates/tasks-template.md — Phase 2(Foundational)에 RBAC·버저닝 태스크 포함 가능. 수정 불필요.

Deferred TODOs: 없음
-->

# tcmanager Constitution

## Context

- **제품**: tcmanager — 테스트케이스(TC) 관리 도구
- **팀**: 품질관리팀 약 15명
- **구현**: Python, 로컬 실행을 전제로 한 구성
- **연동**: Jira, Bitbucket, Confluence, Teams (공식 REST API만 사용).
  연동이 실패하거나 비활성화되어도 로컬 핵심 기능은 정상 동작해야 함.
- **규모 가정**: TC 수는 수백~수만 개까지 확장 가능한 구조를 전제로 함.

## Core Principles

### I. 추적 가능성 (Traceability)

모든 TC는 Jira·Confluence와 식별 가능한 방식으로 연결해야 한다 (최소 연결 단위는 스펙에서 명시).
TC 및 메타데이터의 모든 변경은 이력으로 보존해야 한다.
삭제는 소프트 딜리트를 기본으로 한다.
영구 삭제는 관리자 승인 하에만 허용하며, 삭제 사유와 승인자를 이력에 기록한다.

### II. 역할 기반 접근 (RBAC)

역할은 작성자 / 검토자 / 실행자 / 관리자 네 가지로 구분한다.
각 역할의 구체적 권한은 스펙에서 정의한다.
승인·검토 규칙을 만족하지 않는 TC 수정은 허용하지 않는다.

### III. TC 수명주기 (TC Lifecycle)

TC 원본의 등록·승인 흐름은 아래 전이만 허용한다:

```
초안 → 검토 → 승인 → (활성) → 소프트 딜리트
```

임의 상태 점프는 허용하지 않는다.
승인되지 않은 TC는 실행 세트에 포함할 수 없다.

### IV. 실행 상태 (Execution Workflow)

실행 세트 내 TC별 실행 흐름은 아래 전이만 허용한다:

```
실행 전 → 실행 중 → Pass / Fail / Blocked / Skip
```

실행 결과는 반드시 해당 실행에 사용된 TC 버전과 연결해 보관해야 한다.

### V. TC 품질 기준 (TC Quality)

모든 TC는 전제조건·단계·기대결과를 각각 하나 이상 포함해야 한다.
비어 있거나 미작성인 채로 저장·승인할 수 없다 (필드 형식·최소 길이 등은 스펙에서 정의한다).

### VI. 버저닝 (Versioning)

TC 내용(전제조건·단계·기대결과) 변경 시 새 버전을 생성해야 한다.
이전 버전은 읽기 전용으로 보존하며, 수정·삭제는 허용하지 않는다.
오탈자·서식만 고치는 경우에는 동일 버전 덮어쓰기를 허용한다
("의미 변경"과의 구분 규칙은 스펙에서 정의한다).
실행 세트는 스프린트·릴리즈·목적 등 필요에 따라 자유롭게 묶어 구성할 수 있다.

### VII. 확장성과 단순성 (Scalability & Simplicity)

초기에는 수백 개 규모로 시작하되, 수만 개까지 성장할 수 있는 구조를 처음부터 설계해야 한다.
로컬 실행 환경에 맞게 외부 서버 의존성을 최소화한다.
기능 추가 전 기존 핵심 흐름(수명주기·실행)을 먼저 안정화한다.
과도한 최적화나 복잡도 도입은 명확한 근거 없이 허용하지 않는다.

### VIII. 연동 표준화 (Integration Standards)

Jira·Bitbucket·Confluence·Teams 연동은 공식 REST API만 사용한다.
연동은 선택적 기능이며, 연동 실패·비활성화 시에도 로컬 핵심 기능은 영향을 받지 않아야 한다.

## Security & Local Data

API 키·토큰·비밀번호 등 비밀은 소스에 포함하지 않는다.
환경 변수 또는 OS가 권장하는 안전한 저장 방식을 따른다.
로컬에 저장되는 데이터는 손실·손상에 대비한 백업/복구 전략을 문서화한다
(최소: 백업 주기·보관 위치 원칙).

## Governance

이 컨스티튜션은 모든 스펙·계획·구현에 우선한다.
원칙 변경은 관리자 승인 후 문서화하며, 하위 산출물(spec, plan, tasks)에 영향을 전파한다.
모든 PR·리뷰에서 컨스티튜션 준수 여부를 확인해야 한다.
버전 규칙: 원칙 삭제·재정의는 MAJOR, 원칙 추가는 MINOR, 문구 수정은 PATCH.

**Version**: 1.0.0 | **Ratified**: 2026-03-27 | **Last Amended**: 2026-03-27

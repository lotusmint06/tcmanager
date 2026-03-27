# CLAUDE.md

이 파일은 Claude Code(claude.ai/code)가 이 저장소에서 작업할 때 참고하는 가이드입니다.

## 프로젝트 개요

**tcmanager**는 테스트케이스 관리 툴입니다. 현재 기획/설계 단계로, **SpecKit 0.4.3** — Claude Code 및 Cursor와 통합된 명세 기반 개발 프레임워크를 사용합니다.

## SpecKit 워크플로우

기능 개발은 커스텀 슬래시 명령어를 통한 구조화된 파이프라인으로 진행됩니다.

**Claude Code** (`.claude/commands/`), **Cursor** (`.cursor/commands/`) 모두 동일한 명령어를 지원합니다:

```
/speckit.specify <기능 설명>   # 기능 명세 생성/수정 (spec.md)
/speckit.clarify               # 모호한 요구사항 식별 및 해소
/speckit.plan                  # 구현 계획 생성 (plan.md)
/speckit.tasks                 # 우선순위별 태스크 분해 (tasks.md)
/speckit.analyze               # 산출물 간 일관성 검토
/speckit.checklist <요구사항>  # 검증 체크리스트 생성
/speckit.implement             # 의존성 순서대로 태스크 실행
/speckit.taskstoissues         # 태스크를 GitHub 이슈로 변환
```

실행 순서: `specify` → `clarify`(선택) → `plan` → `tasks` → `implement`

## 기능 디렉토리 구조

각 기능은 `specs/` 하위에 별도 디렉토리를 가집니다:

```
specs/
└── 001-기능명/
    ├── spec.md     # 기능 명세
    ├── plan.md     # 구현 계획
    └── tasks.md    # 우선순위별 태스크 목록
```

새 기능 브랜치 및 명세 디렉토리 생성:

```bash
.specify/scripts/bash/create-new-feature.sh <설명>
```

## 브랜치 네이밍

순차 번호 방식 (`.specify/init-options.json` 현재 설정):
- `001-기능명`, `002-다른기능`, `003-버그수정` 형식

## 명세 구조

**spec.md** — 사용자 시나리오, 기능 요구사항(FR-001...), 주요 엔티티, 성공 기준
**plan.md** — 기술 접근 방식, 의존성, 프로젝트 구조, 컨스티튜션 준수 검토
**tasks.md** — 단계별 태스크: 설정 → 기반 구축(모든 스토리 선행 조건) → 사용자 스토리(P1/P2/P3) → 마무리

태스크 우선순위: P1(MVP 필수) → P2(중요) → P3(있으면 좋음). `[P]` 표시 태스크는 병렬 실행 가능.

## 프로젝트 컨스티튜션

`.specify/memory/constitution.md`에 프로젝트 수준의 원칙과 제약 사항을 정의합니다. 계획 수립 단계에서 이를 준수하는지 반드시 검토해야 합니다.

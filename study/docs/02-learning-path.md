# 학습 경로

예상 총 시간: 약 10시간. 선행 관계 순서대로 정렬.

```text
슬래시 명령 → 스킬 → 훅 ─┐
메모리 → 서브에이전트 ────┼→ 플러그인 → 고급 기능
MCP ─────────────────────┘
체크포인트 (독립)          CLI (마지막)
```

## Phase 1: 기초 다지기 (약 2시간)

- [ ] **슬래시 명령 / 커스텀 스킬** — 심화 · [튜토리얼](../../01-slash-commands/README.md) · [노트](lessons/01-custom-skills.md)
  - 중점: SKILL.md frontmatter, `$ARGUMENTS`/`$0`, `` !`command` ``, `@file`
  - 완료 기준: 인자와 동적 컨텍스트를 쓰는 스킬을 직접 만들 수 있다
- [ ] **메모리** — 심화 · [튜토리얼](../../02-memory/README.md)
  - 중점: 7개 위치가 루트부터 이어 붙는 방식, `.claude/rules/`, 경로별 규칙, `@import` (최대 깊이 4), Auto Memory `MEMORY.md`
  - 완료 기준: 디렉터리별 모듈화된 규칙을 두고 로드 순서를 설명할 수 있다
- [ ] **체크포인트** — 처음부터 · [튜토리얼](../../08-checkpoints/README.md)
  - 중점: Esc+Esc, `/rewind`, 옵션 6가지, 한계 (Bash로만 바꾼 파일과 서브에이전트 수정은 추적 안 됨)
  - 실습: 실험적 변경 후 되돌리기
  - 완료 기준: 되돌릴 수 있다는 확신으로 실험할 수 있다

## Phase 2: 자동화 심화 (약 1.5시간)

- [ ] **스킬** — 심화 · [튜토리얼](../../03-skills/README.md)
  - 중점: `context: fork` + `agent`, `disable-model-invocation` vs `user-invocable`, 번들 리소스 (`scripts/`, `references/`, `assets/`)
  - 완료 기준: 서브에이전트에서 실행되는 스킬을 만들 수 있다
- [ ] **훅** — 심화 · [튜토리얼](../../06-hooks/README.md)
  - 중점: 훅 타입 5가지, 스킬 frontmatter 훅 (세션 끝까지 유지), `onFailure: "block"`, JSON 출력
  - 완료 기준: prompt 기반 Stop 훅과 스킬 frontmatter 훅을 만들 수 있다

## Phase 3: 외부 연동 (약 2.5시간)

- [ ] **MCP** — 처음부터 · [튜토리얼](../../05-mcp/README.md)
  - 중점: `claude mcp add`, 전송 방식 (`http` 권장, `stdio`), 환경 변수 확장
  - 실습: GitHub MCP 추가 후 PR 조회
  - 완료 기준: MCP로 외부 서비스의 실시간 데이터를 조회할 수 있다
- [ ] **서브에이전트** — 처음부터 · [튜토리얼](../../04-subagents/README.md)
  - 중점: `.claude/agents/*.md` 형식, 기본 제공 에이전트, tools/model/permissionMode
  - 실습: code-reviewer 서브에이전트 만들고 위임 테스트
  - 완료 기준: Claude가 코드 리뷰를 내 에이전트에 맡긴다

## Phase 4: 마스터리 (약 4시간)

- [ ] **CLI** — 처음부터 · [튜토리얼](../../10-cli/README.md)
  - 중점: `claude -p` 파이프, `--output-format json`, `-c`/`-r`
  - 실습: 파일을 `claude -p`에 파이프로 넘겨 JSON으로 받기
  - 완료 기준: 스크립트에서 Claude를 비대화형으로 쓸 수 있다
- [ ] **플러그인** — 처음부터 · [튜토리얼](../../07-plugins/README.md)
  - 중점: `plugin.json` 구조, 플러그인 구성 요소, 마켓플레이스 설치
  - 실습: `claude plugin install <plugin>@claude-plugins-official` 후 구성 요소 살펴보기
  - 완료 기준: 플러그인과 개별 구성 요소 중 언제 무엇을 쓸지 판단할 수 있다
- [ ] **고급 기능** — 처음부터 · [튜토리얼](../../09-advanced-features/README.md)
  - 중점: 플래닝 모드 (`/plan`, Shift+Tab), 권한 모드 6가지, `/effort`, `ultrathink`
  - 실습: 플래닝 모드로 기능 설계 후 구현
  - 완료 기준: 계획과 구현 모드를 자연스럽게 오갈 수 있다

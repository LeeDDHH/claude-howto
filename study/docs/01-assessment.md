# 실력 평가 결과 & 약점

## 평가 기록

| 날짜 | 방식 | 점수 | 레벨 |
|------|------|------|------|
| 2026-11-04 | 심층 평가 | 4/20 | Level 1 — 초급 |

## 스킬 프로필 (2026-11-04)

| 기능 영역 | 점수 | 숙련도 | 상태 |
|-----------|------|--------|------|
| 슬래시 명령 | 1/2 | 기초 | 복습 |
| 메모리 | 1/2 | 기초 | 복습 |
| 스킬 | 1/2 | 기초 | 복습 |
| 훅 | 1/2 | 기초 | 복습 |
| MCP | 0/2 | 없음 | 학습 |
| 서브에이전트 | 0/2 | 없음 | 학습 |
| 체크포인트 | 0/1 | 없음 | 학습 |
| 고급 기능 | 0/3 | 없음 | 학습 |
| 플러그인 | 0/2 | 없음 | 학습 |
| CLI | 0/2 | 없음 | 학습 |

## 할 줄 아는 것

- 커스텀 슬래시 명령 / 스킬 생성 (SKILL.md, `.claude/commands/`)
- 프로젝트 + 개인 메모리 설정 (CLAUDE.md, `~/.claude/CLAUDE.md`)
- 자동 호출 스킬 설치 및 사용
- PreToolUse / PostToolUse 훅 설정

## 약점

### 복습 필요 (기초는 앎, 고급 기능 미사용)

| 영역 | 아직 안 해본 것 |
|------|-----------------|
| 슬래시 명령 | 동적 컨텍스트 (`` !`command` ``), 인자 (`$ARGUMENTS`, `$0`/`$1`), `@file` 참조 |
| 메모리 | 메모리 7개 위치의 로드 순서, `.claude/rules/`, 경로별 규칙, `@import` |
| 스킬 | `disable-model-invocation`, `user-invocable`, `context: fork` + `agent` |
| 훅 | prompt/HTTP 훅, 스킬 frontmatter 훅, JSON 출력 (`updatedInput`, `systemMessage`) |

### 처음부터 학습 필요 (미사용)

| 영역 | 핵심 |
|------|------|
| 체크포인트 | Esc+Esc, `/rewind`, 되돌리기 옵션 6가지 |
| MCP | `claude mcp add`, 외부 서비스 연결 |
| 서브에이전트 | `.claude/agents/*.md`, 작업 위임 |
| CLI | `claude -p`, `--output-format json`, `-c`/`-r` |
| 플러그인 | `.claude-plugin/plugin.json`, 마켓플레이스 설치 |
| 고급 기능 | 플래닝 모드, 권한 모드 6가지, `/effort` |

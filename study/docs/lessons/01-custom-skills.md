# 01. 커스텀 스킬 만들기

- 튜토리얼: [01-slash-commands/README.md](../../../01-slash-commands/README.md)
- 시작일: 2026-11-04
- 완료일:
- 상태: 진행 중

## 목표

- SKILL.md 구조와 frontmatter 필드를 이해한다.
- 인자(`$ARGUMENTS`, `$0`/`$1`)를 받는 스킬을 만든다.
- 동적 컨텍스트(`` !`command` ``)와 `@file` 참조를 쓴다.
- 호출 방식(`disable-model-invocation`, `user-invocable`)을 제어한다.

## 핵심 개념

### 스킬 위치

| 위치 | 범위 |
|------|------|
| `~/.claude/skills/<name>/SKILL.md` | 개인. 모든 프로젝트에서 사용 |
| `.claude/skills/<name>/SKILL.md` | 프로젝트. 팀과 공유 |
| `.claude/commands/<name>.md` | 레거시 명령. 아직 동작하지만 스킬 권장 |

디렉터리 이름이 곧 `/명령` 이름이 된다 (`name` 필드로 바꿀 수 있음).

### Frontmatter 주요 필드

| 필드 | 역할 | 기본값 |
|------|------|--------|
| `name` | `/name`으로 호출 | 디렉터리 이름 |
| `description` | Claude가 언제 쓸지 판단하는 근거 | 본문 첫 줄 |
| `argument-hint` | 자동완성에 보이는 인자 힌트 | 없음 |
| `allowed-tools` | 권한 확인 없이 쓸 도구 | 상속 |
| `model` | 사용할 모델 | 상속 |
| `disable-model-invocation` | `true`면 사용자만 호출 (Claude 자동 호출 X) | `false` |
| `user-invocable` | `false`면 `/` 메뉴에서 숨김 (Claude만 호출) | `true` |
| `context` | `fork`면 격리된 서브에이전트에서 실행 | 없음 |
| `agent` | `context: fork`일 때 에이전트 타입 | `general-purpose` |

### 본문에서 쓰는 문법

| 문법 | 의미 | 예 |
|------|------|-----|
| `$ARGUMENTS` | 인자 전체 | `/fix-issue 123` → `123` |
| `$0`, `$1` | 개별 인자 | `/review-pr 456 high` → `$0`=456, `$1`=high |
| `` !`cmd` `` | 프롬프트 전송 **전** 셸 실행, 출력 삽입 | `` !`git status` `` |
| `@path` | 파일 내용 삽입 | `@src/app.js` |

## 실습

### 실습 1: 가장 단순한 스킬 — `/hello`

```bash
mkdir -p ~/.claude/skills/hello
```

`~/.claude/skills/hello/SKILL.md`:

```markdown
---
name: hello
description: Greet the user and summarize what this project is about
---

Greet me in Korean, then summarize this project in 2 sentences.
```

확인: 새 세션에서 `/hello` 실행.

결과:

### 실습 2: 인자 받기 — `/explain`

```markdown
---
name: explain
description: Explain a file or concept at a chosen level
argument-hint: [target] [level]
---

Explain $0 for a $1-level developer, in Korean, in under 10 lines.
```

확인: `/explain 01-slash-commands/README.md beginner`

결과:

### 실습 3: 동적 컨텍스트 — `/git-snapshot`

```markdown
---
name: git-snapshot
description: Summarize current git state, optionally focusing on a path
argument-hint: [path]
allowed-tools: Bash(git *)
disable-model-invocation: true
---

## Context

- Branch: !`git branch --show-current`
- Status: !`git status --short`
- Recent commits: !`git log --oneline -5`

## Your task

Summarize the repo state in 3 bullet points.
If a path was given, focus only on changes under: $ARGUMENTS
```

확인:

1. `/git-snapshot` → Claude가 git 도구 호출 없이 바로 요약하는가?
2. `/git-snapshot scripts/` → 해당 경로에 집중하는가?
3. "git 상태 알려줘"라고 말만 하면 → 이 스킬이 **자동 실행되지 않아야** 정상 (`disable-model-invocation: true`)

결과:

## 막힌 점 & 해결

| 문제 | 원인 | 해결 |
|------|------|------|
|      |      |      |

## 완료 기준 체크

- [ ] 실습 1: 기본 스킬 호출 성공
- [ ] 실습 2: `$0`/`$1` 인자 동작 확인
- [ ] 실습 3: 동적 컨텍스트 삽입 확인
- [ ] 실습 3: 자동 호출 차단 확인

## 한 줄 요약

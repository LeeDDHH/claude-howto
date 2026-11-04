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

결과: 성공 (2026-11-04). `/` 메뉴에 `hello`가 나타났고, 실행하니 한국어 인사와 프로젝트 요약 2문장이 나왔다. 실행 시 `Base directory for this skill: ~/.claude/skills/hello`가 함께 주입되는 것도 확인했다.

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

결과: 성공 (2026-11-04). 본문이 `Explain 01-slash-commands/README.md for a beginner-level developer, ...`로 치환되어 전달됨 → `$0`=파일 경로, `$1`=`beginner` 확인. 인자는 공백 기준으로 나뉜다.

- `argument-hint`: `/expl` 입력 시 명령 목록에 `[target] [level]` 힌트 표시.
- 수준 비교: `beginner`는 쉬운 말로 개념 위주, `advanced`(`/explain 01-slash-commands advanced`)는 호출 제어·`allowed-tools`·훅 실행 순서 등 내부 동작 위주 → `$1`이 결과를 바꾼다.
- 대상(`$0`)에 파일 대신 디렉터리를 줘도 동작한다.

### 실습 3: 동적 컨텍스트 — `/git-snapshot`

```markdown
---
name: git-snapshot
description: Summarize current git state, optionally focusing on a path
argument-hint: [path]
allowed-tools: Bash(git branch --show-current), Bash(git status --short), Bash(git log --oneline -5)
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

> 보안 메모: 튜토리얼 예시의 `Bash(git *)`는 `git push --force`, `git reset --hard`, `git clean -fdx`까지 확인 없이 허용한다. `allowed-tools`는 실제로 쓰는 명령만 정확히 적는다 (최소 권한). `Bash(git branch *)`도 `git branch -D`를 허용하므로 정확한 명령 매칭이 가장 안전하다.

확인:

1. `/git-snapshot` → Claude가 git 도구 호출 없이 바로 요약하는가?
2. `/git-snapshot scripts/` → 해당 경로에 집중하는가?
3. "git 상태 알려줘"라고 말만 하면 → 이 스킬이 **자동 실행되지 않아야** 정상 (`disable-model-invocation: true`)

결과: 모두 성공 (2026-11-04).

1. `` !`...` `` 자리에 실제 git 출력이 삽입됨. Claude는 Bash 도구를 호출하지 않고 바로 요약. 좁힌 `allowed-tools`로도 정상 동작.
2. `/git-snapshot study/` → `$ARGUMENTS`가 `study/`로 치환되어 요약이 `study/`에 집중. 단, `$ARGUMENTS`는 지시문만 바꾸고 셸 출력은 그대로다. 출력까지 거르려면 `` !`git status --short -- $ARGUMENTS` ``처럼 명령 안에서 써야 하고 `allowed-tools`도 맞춰야 한다.
3. "git 상태 알려줘" → 스킬이 실행되지 않고 Claude가 Bash 도구로 직접 `git status` 실행. `disable-model-invocation: true` 스킬은 Claude가 볼 수 있는 스킬 목록에 아예 나타나지 않는다 (`hello`, `explain`은 목록에 있음).

## 막힌 점 & 해결

| 문제 | 원인 | 해결 |
|------|------|------|
|      |      |      |

## 완료 기준 체크

- [x] 실습 1: 기본 스킬 호출 성공
- [x] 실습 2: `$0`/`$1` 인자 동작 확인
- [x] 실습 3: 동적 컨텍스트 삽입 확인
- [x] 실습 3: 자동 호출 차단 확인

## 한 줄 요약

SKILL.md = frontmatter(이름·설명·권한·호출 제어) + 본문 프롬프트. `$0`/`$ARGUMENTS`로 인자, `` !`cmd` ``로 실행 전 컨텍스트 주입, `allowed-tools`는 최소 권한으로.

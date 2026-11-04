# 실습 프로젝트

각 Phase를 마친 뒤 배운 것을 묶어서 써보는 프로젝트.

## 1. 안전한 실험 놀이터

- 주제: 체크포인트 + 훅
- 시점: Phase 1~2 완료 후
- 목표: 위험한 Bash 명령(`rm -rf` 등)을 막는 PreToolUse 훅을 걸고, 코드를 마음껏 바꾼 뒤 `/rewind`로 되돌린다.
- 단계:
  - [ ] 연습용 빈 프로젝트 만들기
  - [ ] `.claude/settings.json`에 Bash 명령 검증 훅 추가 (exit 2로 차단)
  - [ ] 훅이 위험 명령을 막는지 확인
  - [ ] 여러 파일을 수정한 뒤 Esc+Esc로 코드만 되돌리기 / 대화만 되돌리기 비교
- 완료 기준: 훅이 차단하고, rewind로 원하는 시점에 정확히 돌아간다.

## 2. PR 리뷰 봇

- 주제: MCP + 서브에이전트
- 시점: Phase 3 완료 후
- 목표: GitHub MCP로 PR을 가져와 code-reviewer 서브에이전트에게 리뷰를 맡긴다.
- 단계:
  - [ ] GitHub MCP 서버 추가 (`claude mcp add`)
  - [ ] `.claude/agents/code-reviewer.md` 작성 (읽기 전용 도구만 허용)
  - [ ] "PR #N 리뷰해줘" 요청 시 위임되는지 확인
- 완료 기준: PR 데이터를 MCP로 가져오고 리뷰는 서브에이전트가 수행한다.

## 3. CI 변경 요약 스크립트

- 주제: CLI + 스킬
- 시점: Phase 4 완료 후
- 목표: `claude -p --output-format json`으로 변경 사항을 요약하는 스크립트에 커스텀 스킬을 연결한다.
- 단계:
  - [ ] 변경 요약용 스킬 작성 (동적 컨텍스트로 `git diff` 주입)
  - [ ] `git diff | claude -p ... --output-format json` 스크립트 작성
  - [ ] 결과 JSON을 `jq`로 파싱
- 완료 기준: 스크립트 한 번 실행으로 구조화된 요약을 얻는다.

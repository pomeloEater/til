---
title: Claude Code의 3가지 빌딩 블록 — Agent, Skills, Hooks
date: 2026-03-24
tags: [ai, llm, claude-code, agent, skills, hooks]
---

## 무엇을 배웠나

Claude Code에서 자동화 워크플로우를 구성하는 핵심 개념 3가지.

---

## 왜 필요했나 (context)

SHP Checker를 나중에 Claude Code 기반으로 자동화하거나,
커스텀 Skills로 패키징하고 싶어서 개념을 정리함.

---

## 핵심 내용

### Agent

큰 작업을 Claude가 스스로 쪼개서 순서대로 실행하는 구조.
lead agent가 서브태스크를 할당하고 결과를 합친다.

```
"shp 파일 검증하고 리포트 만들어줘"
↓
Claude가 스스로 판단:
1. 파일 읽기
2. 검증 실행
3. 리포트 생성
```

사람이 중간에 개입하지 않아도 Claude가 순서를 결정함.

---

### Skills

프롬프트 + 컨텍스트 + 코드를 하나의 패키지로 묶는 방법.
`SKILL.md` 파일로 정의하고, Claude가 요청에 맞을 때 자동으로 사용.

저장 위치:
- 글로벌: `~/.claude/skills/`
- 프로젝트: `프로젝트/.claude/skills/`

```
~/.claude/skills/
└── shp-validator/
    └── SKILL.md   ← "SHP 검증할 때는 이렇게 해줘" 작성
```

Anthropic 제공 pre-built Skills: PowerPoint, Excel, Word, PDF 등
→ 커스텀 Skills도 직접 만들 수 있음.

---

### Hooks

Agent 루프의 특정 시점에 결정론적 코드를 실행하는 장치.

Anthropic이 정의한 훅:
- `PreToolUse` → 툴 호출 전 실행 (차단 가능)
- `PermissionRequest` → 권한 요청 시 실행 (허용/거부 가능)

활용 예시:
- 툴 호출 전 로그 남기기
- Claude 알림을 Slack으로 푸시
- 매 세션 시작 시 특정 스크립트 자동 실행

---

## 세 개 관계 정리

```
Agent   → 큰 작업을 스스로 쪼개서 실행하는 구조
Skills  → "이런 작업은 이렇게 해줘" 재사용 가능한 패키지
Hooks   → Agent가 툴을 쓰기 전/후에 개발자가 끼어드는 지점
```

---

## 삽질 포인트

Skills는 Claude가 자동으로 언제 쓸지 인식하는 게 아직 불안정하다는 후기가 있음.
→ 글로벌 `Claude.md`에 Skills 목록을 명시하는 workaround가 있음.

---

## 참고 링크

- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- https://platform.claude.com/docs/en/agent-sdk/overview

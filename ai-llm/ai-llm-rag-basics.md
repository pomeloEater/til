---
title: LLM 기초 개념과 RAG 구조 이해
date: 2026-03-23
tags: [ai, llm, rag, embedding, transformer]
---

## 무엇을 배웠나

LLM이 동작하는 핵심 개념들과, RAG가 왜 "문서 전체를 넣지 않는" 구조인지.

**핵심 개념 연결:**
```
Embedding  → 텍스트를 벡터(좌표)로 변환
벡터DB     → 의미 기반 검색 엔진
Retrieval  → 관련 조각만 뽑아오기
Transformer→ 토큰 단위 처리, Context Window 한계 있음
RAG        → 이걸 다 합친 구조
```

---

## 왜 필요했나 (context)

SHP Checker v3에서 사내 매뉴얼 기반 AI 설명 기능을 붙이려면 RAG가 필요하다.
RAG를 쓰려면 Embedding / 벡터DB / Retrieval 개념을 먼저 이해해야 했음.

---

## 핵심 내용

### Embedding
텍스트를 숫자 벡터로 변환하는 것.
→ GIS 비유: 서울을 `(37.5, 126.9)`로 표현하듯, 텍스트를 고차원 공간의 좌표로 표현.
→ 의미가 비슷한 단어는 벡터 공간에서 가까운 위치에 놓인다.

### Transformer / Context Window
LLM의 기본 구조. 텍스트를 토큰 단위로 처리.

```
입력 텍스트
↓
Token으로 쪼개기   ("sewer pipe" → ["sewer", "pipe"])
↓
Embedding 변환     (각 토큰을 숫자 벡터로)
↓
Attention 계산     (단어들 간의 관계 파악)
↓
출력 생성          (다음에 올 단어 예측)
```

⚠️ Context Window = 한 번에 처리할 수 있는 토큰 수 제한.
문서 전체를 넣으면 토큰 초과 or 비용 폭탄 → RAG가 필요한 이유.

### RAG 흐름
```
[사전 준비]
문서 전체 → chunking → Embedding → 벡터DB 저장

[질문할 때]
질문 → Embedding 변환 → 벡터DB 유사 검색 → 관련 조각 3~5개만 LLM에 전달 → 답변 생성
```

### Fine-tuning
특정 도메인에 맞게 모델을 추가 학습시키는 것.
비용·데이터 많이 필요 → 지금 단계에서 우선순위 낮음.

---

## 삽질 포인트

특별한 삽질 없음. 개념 정리 세션.

---

## 앞으로 적용할 것 (SHP Checker 로드맵)

```
v1: 검증 결과 리포트 출력        ← 지금 만들 것
v2: AI가 에러 원인 설명해줌      ← LLM API 연동
v3: 사내 매뉴얼 기반으로 설명    ← RAG 적용
```

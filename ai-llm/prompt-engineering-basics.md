---
title: 프롬프트 엔지니어링 핵심 기법
date: 2026-03-25
tags: [ai, llm, prompt-engineering, chain-of-thought, few-shot]
---

## 무엇을 배웠나

LLM에게 더 좋은 결과를 얻기 위한 프롬프트 작성 기법들.
"적절하게 질문하는 방법"을 포함한 더 큰 개념.

---

## 왜 필요했나 (context)

SHP Checker v2에서 AI가 에러 원인을 설명할 때
프롬프트를 어떻게 짜느냐에 따라 결과 품질이 달라짐.
Fine-tuning 없이도 프롬프트만으로 충분히 성능을 높일 수 있다.

---

## 핵심 기법 (Anthropic 공식 순서)

### 1. Be clear and direct — 명확하게 지시하기
모호하게 쓰지 말고 원하는 걸 구체적으로 명시.

```
❌ "shp 파일 봐줘"
✅ "shp 파일의 CRS, null geometry, 중복 PK를 검사하고
    각 항목별로 문제 개수를 리포트해줘"
```

### 2. Multishot prompting — 예시 주기
3~5개의 다양한 예시를 포함하면 Claude가 정확히 원하는 걸 파악함.
예시가 많을수록 복잡한 태스크에서 성능이 좋아짐.

```
예시 1: CRS 오류 → 이런 설명
예시 2: null geometry → 이런 설명
→ 패턴을 학습해서 새 오류도 비슷하게 설명함
```

### 3. Chain of Thought (CoT) — 생각하게 하기
"단계별로 생각해줘"를 붙이면 복잡한 문제에서 정확도가 올라감.

```
"shp 검증 결과를 분석하고, 단계별로 원인을 설명해줘"
```

### 4. XML 태그 사용
긴 입력에서 구조를 잡을 때 효과적.
Claude가 어디가 데이터고 어디가 지시인지 명확히 구분함.

```xml
<validation_result>
  CRS: EPSG:4326
  Null geometry: 2
</validation_result>
위 결과의 원인과 해결 방법을 설명해줘.
```

### 5. System prompt로 역할 부여
System prompt에 역할을 설정하면 Claude의 행동과 톤이 집중됨.
한 문장만으로도 차이가 남.

```python
system="You are a GIS data validation expert."
```

---

## SHP Checker v2 적용 계획

```
System prompt → "GIS 전문가로서 한국어로 설명해줘"
CoT           → "원인과 해결 방법을 단계별로"
XML 태그      → 검증 결과를 구조화해서 컨텍스트로 넣기
Multishot     → 에러 유형별 설명 예시 2~3개 포함
```

---

## 삽질 포인트

없음. 개념 정리 세션.

---

## 참고 링크

- https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview

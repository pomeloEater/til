---
title: ML과 LLM은 어떻게 구성되는가
date: 2026-03-24
tags: [ai, ml, llm, deep-learning, transformer, rlhf]
---

## 무엇을 배웠나

ML 전체 구조와 LLM이 그 안에서 어떻게 위치하는지.
그리고 LLM을 구성하는 핵심 기술들.

---

## 왜 필요했나 (context)

Claude Code / LLM API를 쓰다 보니 "ML이랑 LLM이 어떻게 다른가"가 긴가민가했음.
AI 공부의 큰 그림을 잡기 위해 정리.

---

## ML의 구성

ML은 크게 학습 방식으로 나뉜다.

### 지도학습 (Supervised Learning)
정답 레이블이 있는 데이터로 학습.
```
입력: 집 크기, 위치  →  출력: 집값
입력: 이메일 텍스트  →  출력: 스팸/정상
```

### 비지도학습 (Unsupervised Learning)
정답 없이 데이터의 패턴을 스스로 찾음.
```
입력: 고객 구매 데이터  →  출력: 비슷한 고객끼리 묶기 (클러스터링)
```

### 강화학습 (Reinforcement Learning)
보상/패널티를 통해 스스로 전략을 학습.
→ AlphaGo, 그리고 요즘 LLM 훈련(RLHF)에도 쓰임.

### 딥러닝 (Deep Learning)
위 방식들을 신경망(Neural Network)으로 구현한 것.
ML의 하위 기술이고, LLM은 여기서 나왔다.

---

## LLM의 구성

### Transformer
LLM의 뼈대. 토큰 단위로 처리하고 Attention으로 단어 간 관계를 파악.
→ 2026-03-23 TIL 참고.

### Pre-training
엄청난 양의 텍스트(인터넷, 책 등)로 "다음 단어 예측"을 반복 학습.
→ 이 단계에서 언어 패턴, 지식, 문맥을 흡수.

### Fine-tuning / RLHF
Pre-training된 모델을 사람 피드백으로 다듬는 과정.
"이 답변이 더 좋아요"를 반복 → 사람이 원하는 방향으로 조정.
→ Claude, ChatGPT가 자연스럽게 대화하는 이유.

### Inference
학습이 끝난 모델이 실제로 답을 생성하는 과정.
API 호출 1번 = Inference 1번.

---

## 전체 그림

```
ML
├── 지도학습
├── 비지도학습
├── 강화학습 (RLHF로 LLM에도 쓰임)
└── 딥러닝
    └── Transformer
        └── LLM
            ├── Pre-training  (언어 패턴 학습)
            ├── Fine-tuning   (도메인 특화)
            ├── RLHF          (사람 피드백으로 다듬기)
            └── Inference     (실제 답변 생성)
```

---

## GIS 연결 포인트

- 위성 이미지에서 도로 자동 감지 → ML (딥러닝, 이미지 분류)
- SHP Checker 에러 설명 → LLM (텍스트 생성)
- 둘 다 AI지만 쓰는 기술이 다르다.

---

## 삽질 포인트

없음. 개념 정리 세션.

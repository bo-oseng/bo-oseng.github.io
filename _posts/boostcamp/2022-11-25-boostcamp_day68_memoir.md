---
layout: single

title: 부스트캠프 AI 10주차(Day-68) 회고록, DKT 대회 - (8) LightGCN + Transformer

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, LightGCN, Transformer]

toc: true
---

## Day 68

# DKT 대회 모델링

## LightGCN

[NGCF](https://bo-oseng.github.io/boostcamp/boostcamp_NGCF_paper_review/) 와 거의 유사한 구조의 모델이다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/203943345-3ad12ad1-3f5a-48e0-8ef9-47f413a074e5.png">

 
### NGCF와 다른점
NGCF에서 간략화를 조금 시켰더니 오히려 성능이 올랐다는게 LightGCN의 핵심이다.
 + Messagge Construction
   + NGCF에서 Laplacian norm 텀이 있었는데 오히려 이걸 없애버렸다.
 + Messagge Aggregation
   + NGCF에서 layer별로 Concat후 활용했는데 weighted sum으로 간략화 시켰다.

### Predition

Predition 단계에서는 다음 두가지 Embedding이 활용된다.

+ User Embedding
+ Item Embedding

각각의 User와 Item에 맞게 Embedding을 Lookup하여 Weighted Sum 한 결과를 활용한다.


## Transformer

BERT의 핵심 키워드는 Attention으로 Attention을 활용해 기존의 RNN의 모델이 가진 입력 데이터끼리의 관계성을 끊을 수 있게 되었다.

그리고 Positional Embedding을 활용해 Sequential함을 유지시켰다.

## LightGCN + Transformer

### 배경
Transformer의 입력 데이터에 LightGCN으로 학습된 user-item interaction의 정보를 담고 있는데 Embedding을 추가 시키려고 한다.

Transformer의 Warm up 개념을 활용한다는 느낌이다.

### LightGCN Custom
기존의 LightGCN은 모든 User에 대한 모든 Item들의 Interaction을 모두 예측하게끔 학습시킨다.

다시 말해 User1, Item3의 Interaction 예측을 학습할때 Lookup한 Item3 벡터와  User6, Item3의 Interaction 예측을 학습할때 Lookup한 Item3는 동일한 Embedding의 같은 부분을 Lookup해 학습 하게 되므로 결국 Item Emedding 자체에 유저끼리 간섭이 생길거라고 예상된다.

그래서 이 부분을 개선해 각 User 한명당 모든 Item에 대한 Embedding을 학습 시킨후 각각을 Transformer의 입력으로 쓰려고 구상중이다.

### 추가실험 예정

+ Transforemr에 어떻게 LightGCN의 Embedding을 추가할지 성능 비교 실험.
  + 기존의 Transformer에 랜덤한 값으로 들어가는 Item에 대한 Embedding들을 그대로 두고 Feature를 새롭게 하나 추가해 LightGCN의 Emedding을 넣은것
  + Item에 대한 Embedding을 모두 LightGCN이 학습한 Embedding으로 교체한것.
+ LightGCN Custom과 기존 LightGCN 성능 비교 실험.
  + 주관적인 판단으로 LightGCN Custom이 더 좋을거 같다는 생각이 들고 성능 비교를 통해 검증할 예정이다.

## Appendix

### 피어섹션 & 의문점
---
layout: single

title: 부스트캠프 AI 10주차(Day-71) 회고록, DKT 대회 - (9) LightGCN + Transformer

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, LightGCN, Transformer]

toc: true
---
## Day 71

# DKT 대회 모델링

## LightGCN + Transformer 이어서

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204238061-dd1cc8e6-9dd9-433f-998d-083f838e5971.png">

이런식으로 모델을 설계 하던 중 깨달은 것이 있다.

<br>

<img width="60%" alt="image" src="https://user-images.githubusercontent.com/94548914/204238604-e86b0a10-1eb9-48e5-ac62-6d9f6523f425.png">

LightGCN이 기존의 방법들 보다 Embedding의 군집화를 잘 이루는 이유가 중 하나가 바로 위와 같은 High-order Connectivity 구조를 통한다는 이유가 있는데 각 User당 아이템을 임베딩하면 사실상 LightGCN을 통한 Embedding의 의미가 사라진다고 판단되었다.


## LightGCN + Transformer 구체적인 방법

1. LightGCN을 학습 중 AUC가 가장 높은 지점에서의 학습된 Embedding을 numpy의 save를 활용해 저장한다.
2. Transformer 모델 빌드 중 미리 저장한 LightGCN의 Embeddding을 불러온다.
3. 기존의 LightGCN은 (Total User + Total Item)의 임베딩이다. 이중 아이템에 대한 embedding만을 쓸 수 있게 item의 개수만큼 Embedding.weight에서 슬라이싱 한다.
4. from_pretrianed를 활용해 Transformer의 Item Embedding을 선언할 때 랜덤한 값이 아닌 LightGCN으로 한번 학습된 Embedding을 넣어준다.
   
결론. Trasnformer의 warm up의 개념으로 LightGCN의 Embedding을 넣어준다.

## 성능

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/204239216-144ccf33-821f-4571-b908-959e293a61c5.png">

## 추가실험 예정

+ Transforemr에 어떻게 LightGCN의 Embedding을 추가할지 성능 비교 실험.
  + 기존의 Transformer에 랜덤한 값으로 들어가는 Item에 대한 Embedding들을 그대로 두고 Feature를 새롭게 하나 추가해 LightGCN의 Emedding을 넣은것
  + Item에 대한 Embedding을 모두 LightGCN이 학습한 Embedding으로 교체한것.
+ LightGCN + Transformer Custom과 기존 LightGCN 성능 비교 실험.
  + 주관적인 판단으로 LightGCN Custom이 더 좋을거 같다는 생각이 들고 성능 비교를 통해 검증할 예정이다.
+ EDA를 하다보면 성능에 유저의 경향성은 거의 없고, 아이템의 경향성에 영향이 많은것으로 판단된다. 허나 앞서 생각했던것 처럼 LightGCN은 User에 의해 학습되는 부분이 꽤나 많고, 이는 성능에 노이즈로 작용할거 같다는 생각이 든다. 그래서 item간의 관계로만 그래프를 나타내는 SASRec를 알아보려한다.

## Appendix

### 피어섹션 & 의문점

내가 생각하는 LightGCN + Transformer을 팀원들에게 소개하고 토론하는 시간을 가져보았다.

RUC의 성능이 0.0036의 향상이 있었다. 꽤나 유의미한 향상이라고 판단되었고 파라미터 튜닝을 진행해보려고 한다.
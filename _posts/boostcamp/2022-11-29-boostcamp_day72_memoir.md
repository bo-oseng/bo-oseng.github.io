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

## LightGCN + Transformer 결과

하이퍼파라미터 튜닝을 통해 Light GCN의 Public AUC 0.7501를 달성했다.

그리고 이때 저장한 Embedding을 가져와 Transforemr에 넣는 Custom BERT에 대해서도 튜닝을 진행했다.

튜닝 후 Custom BERT의 Public ROC 성능은 0.732 이었다.

LightGCN 단독 모델보다 성능이 떨어짐을 관측했다.

원인에 대해 생각해보았고 두가지의 가설을 세웠다.

### 가설

1. 이 대회의 데이터는 유저의 경향성은 거의 없고, 아이템의 경향성에 영향이 많은것으로 판단된다. 앞서 생각했던것 처럼 LightGCN은 User에 의해 학습되는 부분이 꽤나 많고, 이는 본 대회의 데이터 특성상 성능에 노이즈로 작용할거 같다.
2. LightGCN은 구조상 train과 test 데이터 모두를 참고해 학습하나 Transformer는 그렇지 않았다. 여기서 오버피팅이 발생한 것이 아닐까?

오늘은 1번 가설에 대해 연구를 진행했다.

## MF모델 TSNE로 시각화

1번 가설이 타당한지 데이터에 대해서 좀 더 살펴보기 위해 간단하게 구현이 가능한 MF를 통해 시각화를 진행했다. 간단한 MF만으로도 vaild의 AUC가 77.79%로 매우 준수한 성능을 냈고 MF도 어느정도의 representaion을 나타내는 군집화를 이룬다고 추측할 수 있다.

### User에 대해서 피벗 후 MF
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204490110-34097f40-f475-4d42-a901-03a436262011.png">

User 당 아이템들의 임베딩을 tSNE을 활용해 축소한 뒤 시각화를 진행해봤다.  

User끼리 어느정도 뭉쳐 있다는 느낌도 들었지만, 너무 퍼진 형태로 존재한다고 판단되었다.

### Item에 대해서 피벗 후 MF

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204490598-f645f555-5fc8-4aed-8ca2-c75aced8a3ac.png">

User 당 아이템들의 임베딩을 tSNE을 활용해 축소한 뒤 시각화를 진행해봤다.  
Item끼리는 확실히 군집화를 이루었다고 판된된다.

<br>

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204491508-5b7ba2b9-4cdf-4950-884e-81e53f2a2efd.png">  

추가적으로 assementID 중 공통적으로 발견되는 문자열을 기준으로 카테고리를 만들어 그루핑을 한 뒤 시각화를 해봤다. 그리고 매우 유의미한 직관을 얻을 수 있었다.  

User에 대해 tSNE와 Item에 대한 tSNE의 차이를 통해 1번 가설을 어느정도 뒷받쳐 준다고 생각한다. 즉 lightgcn의 user에 대한 학습이 오히려 성능에 노이즈로 작용할 수 있을거라고 판단된다. 또한 본 대회는 아이템에 대한 feature engineering이 성능에 아주 critical 할 것으로 예상된다.

### lightGCN embedding Plot

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204494596-5d5eeb4e-360e-47fb-b841-273d7ce72d92.png">


lightGCN의 Embedding중 아이템 Embedding에 대해서 시각화를 진행했다.lightGCN도 어느정도 군집화가 이루어 진듯하게 보인다.   


<br>

마찬가지로 assementID 중 공통적으로 발견되는 문자열을 기준으로 카테고리를 만들어 그루핑을 한 뒤 시각화를 해봤다.

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204494772-c503eac5-5be4-4b35-868c-8ab12270c98e.png">


카테고리가 내가 임의로 만든 class라 완벽히 신뢰할만한 정보는 아니지만  카테고리를 기준으로는 오히려 MF보다 representation이 떨어진듯 하게 보인다.
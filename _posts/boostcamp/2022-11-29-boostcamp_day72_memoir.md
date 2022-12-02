---
layout: single

title: 부스트캠프 AI 10주차(Day-71) 회고록, DKT 대회 - (10) LightGCN + Transformer

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

앞서 생각했던것 처럼 LightGCN은 User에 의해 학습되는 부분이 꽤나 많고 내가 짰던 모델의 흐름 대로라면 사실상 User의 임베딩을 쓰지 않게 된다. 때문에 Item 임베딩만 따로 떼어 보는 경우에는 오히려 User 정보의 간섭이 Item 임베딩의 representaion에 노이즈로 작용하는 것 같다.


## MF모델 TSNE로 시각화

가설이 타당한지 데이터에 대해서 좀 더 살펴보기 위해 간단하게 구현이 가능한 MF를 통해 시각화를 진행했다. 간단한 MF만으로도 vaild의 AUC가 77.79%로 매우 준수한 성능을 냈고 MF도 어느정도의 representaion을 나타내는 군집화를 이룬다고 추측할 수 있다.

### User에 대해서 피벗 후 MF
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204490110-34097f40-f475-4d42-a901-03a436262011.png">

User 당 아이템들의 임베딩을 tSNE을 활용해 축소한 뒤 시각화를 진행해봤다.  


### Item에 대해서 피벗 후 MF

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204490598-f645f555-5fc8-4aed-8ca2-c75aced8a3ac.png">

User 당 아이템들의 임베딩을 tSNE을 활용해 축소한 뒤 시각화를 진행해봤다.  
Item끼리는 확실히 User에 비해 군집화를 잘 이루었다고 판된된다.

<br>

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/204491508-5b7ba2b9-4cdf-4950-884e-81e53f2a2efd.png">  

추가적으로 assementID 중 공통적으로 발견되는 문자열을 기준으로 카테고리를 만들어 그루핑을 한 뒤 시각화를 해봤다. 그리고 매우 유의미한 직관을 얻을 수 있었다.  

User에 대해 tSNE와 Item에 대한 tSNE의 차이는 가설을 어느정도 뒷받쳐 준다고 생각한다. 즉 내가 설계한 모델은 lightgcn의 item 임베딩들의 user에 대한 학습이 오히려 성능에 노이즈로 작용할 수 있을거라고 판단된다.  

또한 본 대회는 아이템에 대한 feature engineering이 성능에 아주 critical 할 것으로 예상된다.

## 추가

멘토님에게 질문해 본 결과 지금처럼 gcn따로 transformer 따로의 구조가 아닌, End to End로 바꾸는게 해보라고 조언 해주셨다. Feature Engineering을 한 뒤 다시 진행 해보려고한다.
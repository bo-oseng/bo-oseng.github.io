---

layout: single

  

title: 부스트캠프 AI 5주차(Day-29) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록]

  

toc: true

  

---

## Day 29

+ Graph Neural Network
  
  + Graph를 사용하는 이유
    1. 관계, 상호작용과 같은 추상적인 개념을 다루기에 적합.
       + 소셜 네트워크, 바이러스 확산, 유저–아이템 소비 관계 등.
    2. Non-Euclidean Space의 표현 및 학습이 가능.
       + 우리가 흔히 다루는 이미지, 텍스트, 정형 데이터는 격자 형태로 표현 가능하다. 그러나 SNS 데이터, 분자(molecule) 데이터 등은 non-Euclidean space로 다루는 것이 알맞다.
  
  + Naïve Approach
    + 그래프 및 피쳐 데이터를 인접 행렬로 변환하여 MLP에 사용하는 방법
      + 노드가 계속 증가하면 InputLayer가 계속 증가하며 모델의 복잡도와 증가하고 연산량도 증가하는 문제가 있음.
      + 입력데이터가 Sparse 해지는 문제가 있음.
      + 순서의 정보가 없던 row 성분들을 Input Layer에 입력을 순차적으로 주게 되므로 나중에 입력 순서가 달라질 때 의미가 변할 수 있다는 문제가 있음.
  
  + Graph Convolution Network(GCN)
    + Naïve Approach의 한계를 보완.
    + Local connectivity : 물리적으로 이웃한 입력들을 함께 활용.
    + Shared weights : 같은 Convolution의 커널(필터)를 활용.
    + Multi-layer : 컨볼루션 Layer를 여러개 사용해 더 멀리 있는 입력들까지 참고해 활용.
  

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/196107124-77bc7e01-2224-4bae-9802-a3883db397d0.png">

<br>

+ Neural Graph Collaborative Filtering
  
  + 배경
    + 처음 GCN이 추천시스템을 풀기 좋은 모델이란 것을 제시함.
    + 신경망을 적용한 기존 CF 모델(ex. MF)는 유저벡터 아이템벡터를 임베딩한 후 내적해 표현한다 → 유저 아이템 상호작용을 "임베딩 단계"에서 접근하지 못함.
    + Latent factor 추출을 interaction function에만 의존하므로 "sub-optimal"한 임베딩을 사용함. (부정확한 추천의 원인이 될 수 있음, 최적이 아님)
  
  + High-order Connectivity
  
    + Collaborative Signal이 많아질수록 기존의 그래프로는 모든 상호작용을 표현하기엔 한계가 존재함.

        <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/196109798-ca43f217-86d9-4af8-a00b-9e4bcd87161d.png">
    
    + u<sub>1</sub>과 u<sub>2</sub>는 i<sub>2</sub>를 통해 상호작용한다. u<sub>2</sub>의 i<sub>4</sub> i<sub>5</sub> 가 u<sub>1</sub>에 전달된다. u<sub>3</sub>의 i<sub>4</sub>가 u<sub>1</sub>에 전달되고 u<sub>2</sub>의 i<sub>4</sub>도 u<sub>1</sub>에 전달되므로 i<sub>4</sub>가 추천될 확률이 더 높아진다.
  
    <br>

    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/196139877-3dff8ea0-74ce-41bd-9568-86546917a02c.png">

  + MF는 임베딩 벡터로 바로 인터렉션을 구하는 반면 NGCF는 임베딩 레이어에서 임베딩벡터를 생성하고, 임베딩 전파 레이어(GNN)를 통해 전파시킨다.
  
  + 임베딩 레이어(Embedding Layer): 유저-아이템의 초기 임베딩 제공.
    + 기존의 MF, Neural CF 모델에서는 임베딩이 곧바로 interaction function에 입력됨 NGCF에서는 이 임베딩을 GNN 상에서 전파시켜 'refine'함.
  
  + 임베딩 전파 레이어(Embedding Propagation Layer): high-order connectivity 학습.
  
    + 유저-아이템의 collaborative signal을 담을 'message'를 구성하고 결합하는 단계.
      + Message Construction: 유저-아이템 간 affinity를 고려할 수 있도록 메시지를 구성. (weight sharing)
    <img width="50%" alt="Message Construction" src="https://user-images.githubusercontent.com/94548914/196141319-fc4572e2-0fc2-4811-9dc0-a7030285c9e6.png">
      + Message Aggregation: !의 이웃 노드로부터 전파된 message들을 결합하면 1-hop 전파를 통한 임베딩 완료.
    <img width="50%" alt="Message Aggregation" src="https://user-images.githubusercontent.com/94548914/196141328-ae8e615b-5d5c-4aeb-b9dc-d5ed185696fb.png">
      + Higher-order Propagation
        + l 개의 임베딩 전파 레이어를 쌓으면, 유저 노드는 l-차 이웃으로부터 전파된 메시지 이용 가능.
    <img width="50%" alt="Higher-order Propagation" src="https://user-images.githubusercontent.com/94548914/196141942-a687a0d1-d0b9-4439-9bff-3f2f7088470e.png">

    

  + 유저-아이템 선호도 예측 레이어(Prediction Layer): 서로 다른 전파 레이어에서 refine된 임베딩 concat.
    + L차 까지의 임베딩 벡터를 concatenate하여 최종 임베딩 벡터를 계산한 후, 유저-아이템 벡터를 내적하여 최종 선호도 예측값 계산.
    <img width="50%" alt="image" src="https://user-images.githubusercontent.com/94548914/196142321-0ff9cb72-865d-462b-ad0b-30a1a0344cbf.png">
  + 임베딩 전파 레이어가 많아질수록 모델의 추천 성능 향상.
    + 다만, 레이어가 너무 많이 쌓이면 overfitting 발생 가능.
    + 실험 결과, 대략 L = 3 ~ 4일 때 가장 좋은 성능을 보임.
  + MF 보다 더 빠르게 수렴하고 recall 도 높음
    + Model Capacity가 크고 임베딩 전파를 통해 representation power가 좋아졌기 때문
  + MF와 비교하여 유저-아이템이 임베딩 공간에서 더 명확하게 구분됨 (레이어가 많아질수록 명확해짐)

<br>

+ LightGCN
  + 배경
    + Recommender System with GNN
    + GCN의 가장 핵심적인 부분만 사용하여, 더 정확하고 가벼운 추천 모델을 제시함.
    + Light Graph Convolution: 이웃 노드의 임베딩을 가중합 하는 것이 convolution의 전부, 학습 파라미터와 연산량이 감소.
    + Layer Combination: 레이어가 깊어질수록 강도가 약해질 것이라는 아이디어를 적용해 모델을 단순화 함.
  
<img width="80%" alt="GCN" src="https://user-images.githubusercontent.com/94548914/196143440-e0f3eb93-2aac-4055-b564-ce922420085e.png">
<img width="80%" alt="Light GCN" src="https://user-images.githubusercontent.com/94548914/196143448-57adf435-341c-49b4-85e0-76eee7fbdfc4.png">

  + LightGCN Propagation Rule (vs. NGCF)
    + feature transformation이나 nonlinear activation를 제거하고 가중합으로 GCN 적용.
    + 연결된 노드만 사용하였기 때문에 self-connection이 없음.
    + 학습 파라미터는 0번째 임베딩 레이어에서만 존재.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/196144978-ef0df99b-fc2c-4024-b565-e10a6d225c3e.png">

  + LightGCN Model Prediction (vs. NGCF)
    + k-층으로 된 레이어의 임베딩을 각각 α<sub>k</sub>배 하여 가중합으로 최종 임베딩 벡터 계산.
      + α<sub>k</sub>는 k-층임베딩벡터의가중치로,하이퍼파라미터혹은학습파라미터둘다가능.(논문에서는 (K+1)<sup>-1</sup> 사용)
      + 층이 늘어날수록 점점 들어드는 가중치. (층이 깊어질수록 임베딩의 시그널이 낮아질 것이라 가정)

<img width="80%" alt="LightGCN Model Prediction" src="https://user-images.githubusercontent.com/94548914/196144854-47cbdd30-3568-4a25-b148-8d3c21e9b87d.png">

  + 결과 및 요약
    + 학습을 통한 손실 함수와 추천 성능 모두 NGCF보다 뛰어남.
    + training loss가 낮으면서 추천 성능이 좋다는 것은 모델의 Generalization Power가 크다는 것



<br>
<br>
<br>

+ GRU4Rec

  + 배경
    + Recommender System with RNN
    + Session based Recommender System
      + Session: 유저가 서비스를 이용하는 동안의 행동을 기록한 데이터
    + '지금' 고객이 원하는 상품을 추천하는 것을 목표로, 추천 시스템에 RNN을 적용한
    + Session이라는 시퀀스를 GRU 레이어에 입력하여 바로 다음에 올 확률이 가장 높은 아이템을 추천.
  
<img width="707" alt="image" src="https://user-images.githubusercontent.com/94548914/196147274-ea9ba89e-61eb-404f-bc3a-369a64f3f91d.png">

  + Input Layer
    + One-hot encoding된 session
    + (참고) 임베딩 레이어를 사용하지 않았을 때의 성능이 더 높음
  + GRU Layer
    + 시퀀스 상 모든 아이템들에 대한 맥락적 관계 학습
  + Output Layer
    + 다음에 골라질 아이템에 대한 선호도 스코어
  + 학습
    + Session Parallel Mini batches
      + 길이가 짧은 세션들이 단독 사용되어 idle 하지 않도록, 세션을 병렬적으로 구성하여 미니 배치 학습
    + Sampling on the output
      + 현실에서는 아이템의 수가 많기 때문에 모든 후보 아이템의 확률을 계산하기 어려움 따라서, 아이템을 negative sampling하여 subset만으로 loss를 계산함
      + 사용자가 상호작용을 하지 않은 아이템은 존재 자체를 몰랐거나 관심이 없는 것을 전제로 아이템의 인기가 높은데도 상호작용이 없었다면 사용자가 관심이 없는 아이템이라고 가정한다. → 인기에 기반한 Negative Sampling 제시.
  + 결과 및 요약
    + 특정 데이터셋에서 item-KNN 모델 대비 약 20% 높은 추천 성능 
    + GRU 레이어의 hidden unit이 클 때 더 좋은 추천 성능을 보임 (Cross Entropy Loss만 예외)

<br>
<br>
<br>

+ Context-aware Recommendation
+ Factorization Machine (FM)
+ Field-aware Factorization Machine (FFM)
+ Gradient Boosting Machine (GBM)

<br>
<br>
<br>

## 의문점
1. Collaborative의 정확한 느낌. Collaborative 환경? Collaborative 한 모델
   - 8강 퀴즈 중 아이템 정보를 사용하는 방법론을 고르라는 문제가 있엇는데 여기서 헤멘게 이 의문 떄문인거 같다.
2. GBM에서 회귀 문제에서는 예측값으로 residual을 그대로 사용하고, 분류 문제에서는 log(odds) 값을 사용함
   - 분류 문제에서는 0과 1 사이로 예측하는걸 실수로 표현하기 애매해서 log(odds) 값을 사용.
   - odds가 이전에 정리하려고 했던 logit 부분의 연장되는 개념인듯하다.

<br>
<br>
<br>

## 피어세션

- Collaborative의 정확한 느낌. Collaborative 환경? Collaborative 한 모델
- FFM의 필드 구성 중 Numerical Feature에서 descritize란?
- GRU4Rec 중 Session Parallel Mini batches 부분이 잘 이해가 안됐다
- SVD에서 MF와 차이가 Sparse한 데이터가 적다는 것인데 이 부분이 잘 이해가 안됐다.
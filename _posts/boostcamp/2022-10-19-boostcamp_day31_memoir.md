---
layout: single

  

title: 부스트캠프 AI 5주차(Day-31) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, Wide & Deep, DeepFM, Deep Interest Network, DIN, Behavior Sequence Transformer, BST]

  

toc: true
---


## Day 31

### CTR Prediction with Deep Learning

+ 배경
  + Click-Through Rate Prediction 이란?
  
    + 유저가 주어진 아이템을 클릭할 확률(probability)을 예측하는 문제
  
    + CTR 예측은 주로 광고에 사용되며 광고주 및 서비스 제공자들의 이익 창출에 사용됨 

    + CTR 예측이 정확해지는 것은 곧바로 매출 향상에 직결됨

  + 현실의 CTR 데이터를 기존의 선형 모델로 예측하는 데에는 한계가 있음

    + Highly sparse & super high-dimensional features

      + 데이터 자체가  high-dimensional features이라 너무 sparse하다.

    + Highly non-linear association between the features

      + 피처들간의 관계가 너무 높은 비선형성을 가지고 있다.

  + 이러한 데이터에 효과적인 딥러닝 기법들이 CTR 예측 문제에 적용되기 시작

### Wide & Deep Learning for Recommender Systems

+ 배경

  + 선형적인 모델(Wide)과 비선형적인 모델(Deep)을 결합하여 기존 모델들의 장점을 모두 취하고자 한 논문
  
  + 추천시스템에서 해결해야 할 두 가지 과제

    + Memorization: 함께 빈번히 등장하는 아이템 혹은 특성(feature) 관계를 과거 데이터로부터 학습

      + ex) 자주 나오는 데이터는 X(남자, 컴퓨터) →Y(1)와 같이 자주 등장 하면 암기.

    + Generalization: 드물게 발생하거나 전혀 발생한 적 없는 아이템/특성 조합을 기존 관계로부터 발견(=일반화)

      + ex) 새로운 데이터는 X(남자, 화장품) → Y 는 기존 데이터로 다루기 어려움.
  
+ 모델 구조
  
  + The Wide Component

    + Linear와 거의 비슷하나, 차이점은 Cross-Product 활용.
    
    + GeneralizedLinearModel

      + (“gender=female” = 1), (“language=en” = 1), (“# of installs” = 50), ...
    
      + $y = w^{T}x +b$

    + Cross-Product Transformation
      + (“gender=female” = 1) and (“language=en” = 1) → “AND(gender=female, language=en)” = 1
    
      + 서로 다른 변수 두개의 인터렉션을 표현하기위해 추가.
    
      + 본 논문에서는 편의상 두개의 주요 feature에 대해서만 연산.
    
      + $\phi _{k}\left( x\right) =\prod ^{d}_{i=1}x_{i}^{c_{ki}}, c_{ki}\in\{0, 1\}$

      + [Review – Polynomial Logistic Regression](https://bo-oseng.github.io/boostcamp/boostcamp_day29_memoir/#factorization-machines-fm)와 거의 유사함.
      + (n x n)만큼 학습 파라미터가 늘어나지만 표현할 수 있는 한계각 인터렉션의 한계가 명확하다.
  
  + The Deep Component

    + Feed-Forward Neural Network
    
      + 일반적으로 Neural Network

    + 3 layer로 구성되었으며, ReLU 함수를 사용

    + 연속형 변수는 그대로 사용하고, 카테고리형 변수는 피쳐 임베딩 후 사용
    
    + 마지막에 전체를 concat한 뒤 시그모이드를 통해 최종 결과를 예측
    

+ 모델 도식화
  
    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196716221-c4ff7910-4197-4b79-bdb6-939bdcbc5544.png">


    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196713871-f453dc4f-bf76-4708-a1a4-647a529c642c.png">

+ 모델 성능
  + Baseline인 Wide 모델과 Deep 모델은 각각 Offline, Online에서 서로 다른 양상을 보인다. 
  + Wide, Deep, Wide & Deep 모두 Offline 성능은 비슷함.
  + Wide & Deep 모델은 Offline, Online 모두 좋은 성능을 보임.

    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196717156-cb8cdf73-ffab-4ac0-bbfd-8595a3ce2435.png">

### DeepFM

+ 배경

  + Wide & Deep와 유사한 구조를 가지고 있음.

  + Wide & Deep 모델과 차이점은 두 요소(wide, deep)가 입력값을 공유하도록 한 end-to-end 방식의 모델.

  + 따로 feature enginering이 필요 없음.

  + 추천 시스템에서는 implicit feature interaction을 학습하는 것이 중요함
    +  식사 시간에 배달앱 다운로드 수 증가 (order-2 interaction), 식사 시간과 다운로드의 2개의 관계

    +  10대 남성은 슈팅/RPG게임을 선호 (order-3 interaction), 10대 남성 게임 장르 3개의 관계

   + 기존 모델들은 low-order나 high-order interaction 중 어느 한 쪽에만 강함

     + Wide & Deep 모델은 이 둘을 통합하여 문제를 해결함

     + Wide component에 feature engineering이 필요하다는 단점이 있음. (Cross Product)

     + FM을 wide component로 사용하여 입력값을 공유하도록 함
  
   + DeepFM = Factorization Machine + Deep Neural Network
    
+ DeepFM의 구조
  + FM Component
    + 기존의 [FM 모델](https://bo-oseng.github.io/boostcamp/boostcamp_day29_memoir/#factorization-machines-fm)과 완전히 동일한 구조
    + Order-2 feature interaction을 효과적으로 잡음

    <img width="100%" alt=" FM Component" src="https://user-images.githubusercontent.com/94548914/196719531-5253a074-8e17-485b-b191-542f3534f393.png">

  + Deep Component
    + 모든 feature들은 동일한 차원(k)의 임베딩으로 치환됨
    + 임베딩 된 입력값이 모두 Concat되어 DNN후 Sigimoid를 통해 결과를 예측함.
    + 이 때, 임베딩에 사용되는 가중치는 FM Component의 가중치와 동일함.
  
    <img width="100%" alt="Deep Componet" src="https://user-images.githubusercontent.com/94548914/196719541-73b50c36-0c31-42de-82c6-a9176adf3a46.png">

+ 모델 도식화
  
  <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196720731-a8b4085b-714a-4b0e-9eb7-a2abb6ba0d84.png">

+ 타 모델과의 비교

    <img width="50%" alt="image" src="https://user-images.githubusercontent.com/94548914/196721186-3fc2c93f-7e72-44aa-9b02-1d9324fc5114.png">

+ 모델 성능
  + 화웨이가 발표한 데이터셋과 open ctr dataset으로 유명한 criteo 데이터셋에 대해 우수한 성능을 보임.
<img width="50%" alt="image" src="https://user-images.githubusercontent.com/94548914/196721600-fd3dac21-9f7d-4030-8432-9a5b7ed99a31.png">


### Deep Interest Network (DIN)

+ 배경
  + User behavior feature를 처음 사용한 논문
  
  + 알리바바에서 낸 논문임.
  
  + 더 많은 user의 과거 행동 정보등의 다양한 feature를 모델에 사용하고 싶다.
  
  + 기존의 딥러닝 기반 모델들은 모두 유사한 Embedding & MLP 패러다임을 따름
  
    + 이러한 기존의 방식은 사용자의 다양한(diverse) 관심사를 반영할 수 없음
  
    + 유저가 서로 다른 카테고리에 관심사가 동시에 존재할 수 있음 
  
    + 특정 카테고리의 상품을 검색하여 보던 도중에 추천 목록에 있는 상품을 클릭할 때 유저가 해당 카테고리에 관심있음을 의미.

  + 사용자가 기존에 소비한 아이템의 리스트를 User Behavior Feature를 만들어, 예측 대상 아이템와 이미 소비한 아이템 사이의 관련성을 학습해 더 좋은 예측을 기대함.
  
+ User Behavior Feature
  
  + Multi-hot encoding
  
    + 유저가 과거에 했던 행동이 하나가 아니라 n개 존재할 수 있기 때문

+ 모델 구성
  
  +  Embedding Layer
  
     +  Spare한 Feature를 embedding
  
  +  Local Activation Layer
 
     +  Local Activation Unit

        +  예측 아이템과 과거에 소비했던 아이템들의 연관성을 학습함.

        + 후보 광고에 따라 과거 User Behavior에서 소비했던 광고들의 weight 크기가 달라짐

     +  Weighted Sum Pooling

        +  Activation Layer Weight로 가중합한 값을 출력
  
  <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196724398-be43300a-cca5-477b-a03b-5c260e064484.png">
  
  +  Fully-connected Layer 
     +  Sum Pooling의 출력과 다른 Feature들을 concat하고 MLP 학습을 함.

+ 모델 도식화
    
    <img width="100%" alt="image" src="https://user-images.githubusercontent.com/94548914/196726058-0826b6ef-e119-43a4-a215-fd493fc1ff6b.png">

+ 모델 성능

  + Dice는 해당 논문에서 새롭게 제시한 activation function임.

  + 기존의 알려진 CTR 에측에 비해 AUC 성능이 우수함.
    
    <img width="50%%" alt="image" src="https://user-images.githubusercontent.com/94548914/196726589-92b3950f-0049-45dc-bf55-38d5f8d0de78.png">

### Behavior Sequence Transformer (BST)

+ 배경

  + Transformer를 사용한 CTR 예측 논문
 
  + CTR 예측 데이터와 NLP 번역 데이터 간 공통점

    + 대부분 sparse feature로 구성되어 있음

    + low-와 high-order feature interaction이 모두 존재하여 비선형적 관계를 이룸

    + 문장의 순서가 중요하듯, 사용자의 행동 순서(user behavior sequence) 또한 중요함

      + 핸드폰을 구매한 뒤 핸드폰 케이스 상품들을 찾아보는 경향이 있음

  + NLP 분야 전반에서 강력한 성능을 보이는 Transformer 구조를 CTR 예측에도 적용해 볼 수 있음

    + 앞서, DIN에서도 Transformer의 attention 역할을 하는 local activation unit을 사용

  + Transformer
  
+ 전체구조
  
  + 입력을 집합이 아닌 Sequence까지 표현해 그대로 넣어줌
  
  + Transformer의 인코더 부분만 활용
  
  <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/196729158-92dddf0b-ea94-4239-a37b-c66f606bbb69.png">


+ Transformer Encoder Layer

    <img width="50%" alt="Transformer Encoder Layer" src="https://user-images.githubusercontent.com/94548914/196731247-b13f0ea2-8cc9-4cfe-9cc3-9e14c62b399a.png">

  + Multi-Head Self-Attention
    <img width="90%" alt="Multi-Head Self-Attention" src="https://user-images.githubusercontent.com/94548914/196731231-b0099d1b-e90a-400f-bad6-5c1e1b01e953.png">
  + Point-wise Feed-Forward Neural Networks
    <img width="90%" alt=" Point-wise Feed-Forward Neural Networks" src="https://user-images.githubusercontent.com/94548914/196731217-03150eb9-18ba-42b9-81c5-c457fa68ff5f.png">
  + Stacking the Self-Attention blocks
    <img width="90%" alt="Stacking the Self-Attention blocks" src="https://user-images.githubusercontent.com/94548914/196731238-c2bf67ca-00f2-4d5b-a0e3-4f1b9ab98273.png">
    


+ BST vs DIN
  
  + local activation layeràtransformer layer
  
  + user behavior featureàuser behavior sequence
  
+ BST vs Transformer
  
  + Dropout과 leakyReLU 추가
  
  + 레이어 1~4개 사용 (best: 1개)
  
  + Custom Positional Encoding
    + 원래 Transformer에서는 Postional Encoding을 sin과 cos으로 진행했다.
    + 현재 시간과 아이템을 소비한 물리적인 시간의 차이를 활용 $pos(v_{i}) = t(v_{t}) - t(v_{i})$
  
+ 모델 성능
  
  + Transformer는 CTR Prediction Task에서도 SOTA의 성능을 보임.
  
  + 단, Transformer 블록을 2개 이상 쌓을 때 오히려 성능이 감소
  
    + CTR 예측 Task의 sequence는 machine translation와 같은 NLP sequence보다는 덜 복잡한 것으로 보임.
  
    <img width="60%" alt="image" src="https://user-images.githubusercontent.com/94548914/196729786-e7dae864-c899-430f-8883-a563e8674cd3.png">

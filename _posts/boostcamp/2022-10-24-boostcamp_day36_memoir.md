---
layout: single

  

title: 부스트캠프 AI 6주차(Day-36) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, 추천 시스템 정의, Content Based Filtering, Collaborative Filtering, Offline Test, Onlie Test, Temporal Global, Random Split, User Split, K-Fold Cross Validation, Time Series Cross Validation, Content Based Filtering, Memory Based Collaborative Filtering, Model Based Collaborative Filtering]

  

toc: true
---

## Day 36

### 추천 시스템 정의

+ 정보 필터링 기술의 일종으로, 특정 사용자가 관심을 가질 만한 정보를 추천하는 것을 말한다.
  
+ 사용장의 행동 데이터와 이이템 데이터를 분석해 현재 사용자에게 가장 적절한 아이템을 추천하는 것을 말한다.

+ 추천 시스템의 목적?
  + 사용자가 정보를 수집하고 찾는 시간을 감소시킨다.
  + 사용자가 몰랐던 아이템을 발견해 선택의 폭을 넓혀 준다.
  + 개인화 된 추천을 통해 만족도를 극대화 시킨다.
  + 단기적 측면의 목적
    + 매출 증대
    + 새로운 콘텐츠의 발견
  + 장기적 측면의 목적
    + 계산량 감소
    + 유지 비용 절감
    + 비즈니스 가치 창출

+ Push and Pull(추천과 검색)
  + Push: 사용자가 관심을 가질 만한 정보를 시스템이 밀어내듯이 제공하는 것을 의미한다.
  + Pull: 검색처럼 사용자의 의도가 명확할 때 사용자의 의도에 맞는 항목을 찾고 추천하는 문제를 의미한다.
  + 대부분의 추천시스템의 Push 와 Pull의 중간에 위치한다. 비율을 잘 조절하는게 중요 개발 포인트이다.

+ 추천 시스템 개발 시 고려해야 할 사항
    1. 데이터
    2. Task 설정
    3. 목점함수 설계
    4. 랭킹

### 추천 시스템 분류 체계


<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197554110-61128554-7fe2-40b9-9b95-a8962b55f4b4.png">

+ Content Based Filtering
  + Vectorizer
  + Similarity
+ Collaborative Filtering
  + Memory Based
  + Model Based
+ Advanced
  + DeepLearning With Image/Text
  + Context Aware RecSys
  + Multi Armed Bandit

### 추천 시스템 평가 방법

+ Offline Test
  + 수집된 데이터를 Train, Valid, Test set 3가지로 분할하여 모델 성능 평가 시 활용한다.
  + 데이터 분할 전략에 따라서 시스템 성능 평가에 큰 영향을 줄 수 있으므로 적절한 분할 전략이 필요하다.
  + Leave One Last
    + 사용자당 마지막 구매를 Test set으로, 마지막에서 2번째를 Valid set으로 분할한다.
    + 장점: 학습 시 많은 데이터를 사용할 수 있다.
    + 훈련 중에 모델이 테스트 데이터 상호작용을 특징으로 학습할 가능성 있다.
    + <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197556044-3f92c8de-6390-4a09-9dee-c2699c8fefb9.png">

  + Temporal User
    + 시간을 이용한 분할 전략
    + Leave One last와 유사하다.
    + Data leakage 문제가 있다.
  + Temporal Global
    + 시간을 이용한 분할 전략
    + 각 유저 간에 공유되는 시점을 고정하여,특정 시점 이후에 이뤄진 모든 상호작용을 test set으로 분할한다.
    + 학습 및 검증에 사용할 수 있는 상호작용이 적은 문제가 있다.
    + 가장 권장되는 방법이나 도메인에 따라 다르다.

        <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197556956-c41a7c40-5e61-487d-aac0-388c1c628f22.png">

  + Random Split
    + 각 사용자 별로 interaction을 시간 순서에 관계없이 random 하게 아이템을 선택하여 분할한다.
    + 사용하기 쉽고 많은 train set을 얻을 수 있다.
    + Data leakage 문제가 발생할 수 있다.

    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197557445-72bccf7f-772b-461c-906f-e8b982aad53a.png">

  + User Split
    + User-free 모델에만 사용 가능하다.
    + 사용자가 겹치지 않게 사용자를 기준으로 분할한다.
    + Cold-start 문제에 대응하는 모델 생성이 가능한다.
    + Data leakage 문제가 있다.
    + 그러나 오히려 Cold Start를 위해 잘 쓰이지 않음(이유는 안 말해줌) -> 추가학습 필요.
  
    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197561410-3ba84160-a7b8-44b9-9172-7ec8e122a9b3.png">

  + K-Fold Cross Validation
    + 좀 더 일반화 하기 위한 방법이다.
    + 일반화와 폴드 앙상블을 활용해 더 높은 성능을 기대할 수 있으나 k개의 모델을 연산하므로 더 오래걸린다.
    + Train set 내에서 k개의 Fold로 분할한 뒤, 한 Fold씩 Validation set으로 활용하여 검증한다.

    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197560145-ee76a0fd-c573-4031-8ddc-8bbba90d7867.png">



  + Time Series Cross Validation
    + 추천시스템 데이터는 대부분 시간에 대해 dependency가 존재 하기 때문에 시간을 고려하지 않고 학습하면 data leakage가 발생한다.
    + 미래 데이터를 학습하여 과거를 예측하는 문제가 생긴다. 그러므로 Fold 분할 시 시간을 고려한다.
    + Validation 데이터가 각각 달라서 각각의 모델이 상이하고, train 데이터가 줄어든다는 단점이 있다.
  
    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197560664-716252c2-59c4-4734-adb8-6ae550520794.png">

    + 이를 조금 더 개선해 Val을 고정하고 train을 val시점에 맞춰 줄여가며 학습한다. Val이 동일하므로 쉽게 평가할 수 있고 조금 더 비슷한 모델들이 나옴

    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197560966-cd04aab4-e62d-4bc6-a9ee-9ad47d94e287.png">


  + 예측 알고리즘 평가 지표
    + 평점 예측: RMSE, MAE
    + 랭킹 문제: Precision@K, Recall@K, MAP@K, NDCG@K, Hit-rate
  + Offline Test의 한계점
    + 정확성 개선과 실제 시스템 성능 향상을 연관 짓기 어렵다.
    + 서비스 만족도를 높인다고 보기 어렵다.
  + 특성 평가 지표
    + 성공적인 추천시스템: 정확도 향상 + 흔하지 않으면서도 흥미롭고 다양한 아이템을 추천
    + Coverage: 전체 아이템 중 추천시스템이 추천하는 아이템의 비율
    + Novelty: 새로운 아이템 추천
    + Personalization: 개인화된 추천
    + Serendipity: 의외의 아이템 추천
    + Diversity: 얼마나 다양한 아이템이 추천되는지
  
+ Onlie Test
  + 이상적인 A/B는 동일한 유저이나 한 유저에거 동시에 두가지 제안은 불가능하므로 이를 위해 모든 면에서 최대한 비슷한 그룹으로 분할해야 한다.
  + 유저를 두 그룹으로 분할하여 각각 A안과 B안을 보여주고 평균을 비교한다.
  + 두 그룹의 특성이 모든면에서 비슷해야 한다.
  + 그룹이 나뉘는 시점도 비슷해야 한다.(시점에 따라 데이터가 변할 수 있으므로)
  + 트래픽(traffic)을 반으로 나눠서 사용한다.

### Content Based Filtering
+ 배경
  + Content: 아이템의 성질 및 특성을 말한다.
  + 사용자가 특정 아이템을 선호한다면 해당 아이템과 비슷한 콘텐츠를 가진 다른 아이템을 추천해주는 방식을 말한다.
    + ex) 딸기잼을 소비한 유저에게 블루베리 잼을 추천
    + ex) 읽은 작품과 비슷한 작품 추천
    + 최근 본 상품과 비슷한 상품 추천
+ 아이템 특성화
  + 아이템을 벡터화 시킬 수 있어야한다. 아이템 특성을 벡터 형태로 어떻게 표현 할지가 중요한 포인트이다.
  + One-hot encoding
    + 단점1. 차원이 많아지면 데이터가 sparse해진다.
    + 단점2. 텍스트 데이터의 경우 단어간 유사성이 반영되지 않아 왜곡 될 가능성이 있다.
  + Embedding
    + Sparse가 해소한다.
    + 단어간 유사성까지 벡터에 반영된다.
  + [TF-IDF](https://bo-oseng.github.io/boostcamp/boostcamp_week4_memoir/#day-23)
    + sklearn의 TF-IDF는 띄어쓰기를 기준으로 단어를 분할되므로 a, the, `, 단수, 복수 등의 문제가 생길 수 있음 → mapcap 같은 toknizer를 활용하자.
  + [Word2Vec](https://bo-oseng.github.io/boostcamp/boostcamp_week4_memoir/#day-24)
  
```
참고
Label Encoding: Label Encoding은 카테고리 피처를 코드형 숫자 값으로 변환하는 것입니다. 
추천시스템에서 Label Encoding은 단어 간의 거리가 가까워져 아이템의 특성이 왜곡 될 수 있어 사용하지 않는다.
```

+ 유사도
  + 유사성을 무엇으로 계산할지 정하는 것이 콘텐츠 기반 필터링의 핵심 과정이다.
  + 유클리드 유사도
    + 원 핫에서는 사용하기 어렵다.
  + 자카드 유사도
  + 코사인 유사도
  + 피어슨 상관계수
    + 선형관계에 대한 유사도이기에 값이 0이 나오면 그 의미를 알 수 없음

+ 콘텐츠 기반 추천의 장점 및 한계점
  + 장점
    + 다른 유저들의 데이터가 필요 없으므로 cold start와 sparsity의 문제가 없다.
    + 독특한 취향을 가진 유저에게도 적합한 추천이 가능하다.
    + 새로운 아이템이나 유명하지 않은 아이템도 추천이 가능하다.
  + 한계점
    + 적합한 피처(feature)를 뽑기 어렵다.
    + 유사한 아이템만 반복적으로 추천되어 다양성이 떨어진다.
    + 다른 유저의 데이터나 사용자의 평가를 반영하기 어렵다.

### Memory Based Collaborative Filtering

+ Collaborative Filtering
  + 나랑 비슷한 성향의 친구들이 읽은 책을 찾아본다
  + "나와 비슷한 취향의 사람들이 좋아한 것은 나도 좋아할 가능성이 높다”
  + 유저와 아이템의 “상호작용 정보”를 바탕으로 추천

+ User Based Collaborative Filtering
+ Item Based Collaborative Filtering
+ Memory Based Collaborative Filtering의 장점 및 한계점
  + 장점
    + 최적화나 훈련 과정이 필요 없다.
    + 접근 방식이 쉽다.
    + Item based Collaborative Filtering: 시간에 따라 유사도 변화가 적다.
  + 한계점
    + 유저와 아이템의 상호작용이 적은 경우 각 유저와 아이템의 유사도가 왜곡되어 성는이 떨어진다.
    + 희소한(sparse) 데이터의 경우 성능 저하된다.
    + User based Collaborative Filtering: 새로운 사용자의 경우 정보가 없어서 추천이 어려움(Cold-Start의 문제)
    + Item based Collaborative Filtering: 사용자가 아이템에 feedback한 정보가 많아야 한다.

### Model Based Collaborative Filtering
+ 배경
  +  Memory Based Collaborative Filtering의 문제점
     + 데이터 희소성(Sparsity)
     + 확장성(Scalability)
     + 사용자, 아이템 개수가 늘어나면 계산량이 기하급수적으로 늘어남

+ Model Based Collaborative Filtering 장점
  + 항목간 유사성 단순 비교가 아닌 데이터 패턴을 학습하여 추천 가능하다.
  + 사용자-아이템 관계의 잠재적 특성 및 패턴을 찾을 수 있다.
  + 학습 이후 서빙 속도가 빠름

+ Clustering
  
    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197551954-bf6f8e4a-7612-4d85-9a36-42e783475f7b.png">

  + 데이터를 군집화 하여 군집내의 다른 사용자가 선호하는 아이템을 추천한다.
  + 군집화 이후 Collaborative Filtering 사용을 통한 예측 정확도가 향상된다.
  + 군집 데이터를 추룰하여 아이템 선호드를 계산하고, 사전확률로 활용하여 BPR을 적용한다.
  + K-Means Clustering
    + 가장 가까운 중심점을 갖는 군집에 각 항목을 할당하는 과정을 반복하여 k개의 군집으로 항목들을 나누는 알고리즘
    1. 랜덤하게 초기 중심점을 배치한다.
    2. 각 데이터를 가장 가까운 중심점으로 할당한다.
    3. 모인 데이터에서 새로운 중심점 업데이트 한다.
    4. 더 이상 중심점이 업데이트 되지 않을 때까지 2와3을 반복한다.

    <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197551767-e9f5a9dd-4c30-4adc-bb72-e48b5d4d2460.png">

  + 군집화의 한계점
    + 데이터를 분할함에 따라 분할 된 데이터의 희소성 문제가 발생해 성능이 저하된다.
    + 군집개수등 파라미터를 직접 설정 해야한다.


## 의문점
- User Split에서 오히려 Cold Start를 위해 잘 쓰이지 않는 이유가 뭘까?
 
## 피어섹션

- User Split에서 오히려 Cold Start를 위해 잘 쓰이지 않는 이유가 뭘까?
- Temporal User/Global Split의 상호작용이 왜 적은지 논의.
- Hit-rate 구체적인 설명 논의.

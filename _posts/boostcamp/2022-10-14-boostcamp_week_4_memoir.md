---

layout: single

  

title: 부스트캠프 AI 4주차(Day-22 ~ Day-26) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록]

  

toc: true

  

---

## Day 22
+ 쉬는 날이었지만 Dive Into Deep Learning 17장을 팀원들과 리뷰했다.
  + 17.7 Sequence-Aware Recommender Systems
  + 17.8 Feature-Rich Recommender Systems
  + 17.9 Factorization Machines
  + 17.10 Deep Factorization Machines
  + 7.1 From Fully Connected Layers to Convolutions
  

<br>
<br>

## Day 23

+ 이고잉님의 깃허브 특강-1
  + git init
  + git checkout
  + HEAD와 MASTER(Main)
  + detached head
  + git branch
  + git merge
  
<br>


+ 추천 시스템 문제정의
  + 유저가 접할 수 있는 정보의 양이 다양해지며 정보를 찾는데 시간이 오래 걸림
  + Few Popular items → Long Tail Phenomenon

    <img width="50%" alt="image" src="https://user-images.githubusercontent.com/94548914/195973538-d9bade72-6a64-4c4a-ad16-ededde708739.png">

  + 사용 데이터
    + 유저 관련 정보
    + 아이템 관련 정보
    + 유저 - 아이템 상호작용 정보
      + Explicit Feedback
        + 유저에게 아이템에 대한 만족도를 직접 물어본 경우
      + Implicit Feedback
        + 유저가 아이템을 클릭하거나 구매한 경우

  + 특정 유저에게 적합한 아이템을 추천한다. or 특정 아이템에게 적합한 유저를 추천한다.  
    + 유저 - 아이템 상호 작용을 평가할 score 값이 필요하다.
    + 추천을 위한 score는 어떻게 구해지고 사용하는가?

  + 랭킹 또는 에측
    + 랭킹: 유저에게 적합한 아이템 Top K개를 추천하는 문제
      + 평가 지표: Precision@K, Recall@K, MAP@K, nDCG@K
    + 예측: 유저가 아이템을 가질 선호도를 정확하게 예측 (평점 or 클릭/구매 확률)
      + 평가 지표: MAE, RMSE, AUC

+ Offline Test
  + AP@K
      $$ \dfrac{1}{m}\sum ^{k}_{i=1} Precisiom@i $$
  + MAP@K
      $$ \dfrac{1}{ |{U}| }\sum ^{|{U}|}_{u=1} (AP@K)_{u} $$
  + NDCG

    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/195974225-efd4dda9-d537-46f2-8570-9749219d5415.png">


+ Onlie Test
  + 실제 추천 결과를 서빙하는 단계
  + 동시에 대조군(A)과 실험군(B)의 성능을 평가
    + Traffic을 50% 50%로 나눠 서빙 해봄
  + 모델 성는이 아닌 매출, CTR 등의 비즈니스 서비스 지표로 평가

+ 인기도 기반 추천
  + Most Popular
  + Highly Rated
  + Hacker News Formula
  + Reddit Formula
  + Steam Rating Formula

+ 추천 시스템에서 딥러닝의 한계
  + 추천 시스템은 Deep Learning으로 얻을 수 있는 성능의 향상폭이 CV나 NLP에 비해 크지 않다.
  + 많은 유저들의 트래픽이 몰리는 곳에서 서비스 하는 경우가 많기 떄문에 모델의 Inference Time 즉 Latency가 중요함.

+ 연관 규칙 분석(Association Rule Analysis)
  + 상품의 구매, 조회 등 하나의 연속된 거래들 사이의 규칙을 발견하기 위해 적용함
  + 흔히 장바구니 분석 혹은 서열 분석이라고도 불림
    + ex) 맥주와 기저귀를 같이 구매하는 빈도가 얼마나 되는가?
  + itemset
  + frequent itemset
  + support count
  + support
  + confidence
  + lift

    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/195975212-7cba102c-295b-403f-9c84-60f8329754e4.png">

    <img width="70%" alt="support" src="https://user-images.githubusercontent.com/94548914/195975379-0e76e7f3-c92e-434c-9c00-171d5ecf923a.png">
    
    <img width="70%" alt="confidence" src="https://user-images.githubusercontent.com/94548914/195975380-cfbfe517-b782-4a64-9eff-6028f777b06b.png">

    <img width="70%" alt="lift" src="https://user-images.githubusercontent.com/94548914/195975381-3ad87ef9-0bd0-4ca7-bbb0-eebb287e429c.png">

  + lift 값을 내림차순 정렬하여 의미 있는 rule을 평가함.
    + 주의할 점은 이 rule 어떠한 상관관계를 의미하는 것은 아님.
  + 실제로 연관 규칙 분석을 활용할 때는 모든 연관 규칙을 구하는 Brute-force가 아닌 Apriori, DHP, FP-Growth 등의 알고리즘을 활용한다.


+ TF-IDF(Term Frequency-Inverse Document Frequency)
  + 유저가 선호하는 아이템을 기반으로 해당 아이템과 유사한 아이템을 추천
  + 컨텐츠 기반 추천
    + 장점
      1. 유저에게 추천을 할 때 다른 유저의 데이터가 필요하지 않음 
      2. 새로운 아이템 혹은 인기도가 낮은 아이템을 추천할 수 있음 
      3. 추천 아이템에 대한 설명이 가능함
    + 단점
      1. 아이템의 적합한 피쳐를 찾는 것이 어려움
      2. 한 분야/장르의 추천 결과만 계속 나올 수 있음
      3. 다른 유저의 데이터를 활용할 수 없음

    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/195975549-3d7e62df-dfd8-410d-bedf-a9f82cab3499.png">

    $$ TF-IDF\left(w ,d\right) =  TF\left(w ,d\right) \cdot IDF(w) $$


<br>
<br>


피어세션: 멘토님이 만들어 주신 질문에 각자 팀 노션에 답을 달아보는 시간을 가졌다.

## Day 24
+ Collaborative Filtering
  + Neighborhood-based CF (NBCF)
    + 구현이 간단하고 이해가 쉽다.
    + 아이템이나 유저가 계속 늘어날 경우 확장성이 떨어진다. (Scalability) 
    + 주어진 평점/선호도 데이터가 적을 경우, 성능이 저하된다. (Sparsity)
      + NBCF를 적용하려면 적어도 sparsity ratio*가 99.5%를 넘지 않는 것이 좋음
    + K-Nearest Neighbors CF (KNN CF)
      + 유저 가운데 유저 u와 가장 유사한 K명의 유저(KNN)를 이용해 평점을 예측
        + 유사하다는 것은 우리가 정의한 유사도 값이 크다는 것을 의미함
  + Similarity Function
    + Mean Squared Difference Similarity
    + Cosine Similarity
    + Pearson Similarity
    + Jaccard Similarity
  + Rating Prediction
    + Weighted Average
      + 유저 간의 유사도 값을 가중치(weight)로 사용하여 rating의 평균을 냄
    + Absolute Rating Formula vs Relative Rating Formula
      + 유저가 평점을 주는 기준이 제각기 다르다. 이 점을 보완하기 위해 편차를 활용한다.
  + Model Based CF(MBCF)
    + MBCF의 특징
      + 데이터에 숨겨진 유저-아이템 관계의 잠재적 특성/패턴을 찾음
      + 모델 학습/서빙으로 이미 학습된 모델을 통해 추천하기 때문에 서빙 속도가 빠름
      + Sparsity / Scalability 문제 개선
      + Overfitting 방지
        + NBCF는 주변 특정 데이터로 학습하지만 MBCF는 데이터 전체에 대해 학습하기 때문
      + Limited Coverage 극복
        + NBCF의 유사도 값이 정확하지 않은 경우 이웃의 효과를 보기 어려우나 MBCF는 이 한계를 극복했다.
    + Latent Factor Model(Embedding)
      + 유저와 아이템 관계를 잠재적 요인으로 표현할 수 있다고 보는 모델
      + 유저-아이템 행렬을 저차원의 행렬로 분해하는 방식으로 작동
      + 같은 벡터 공간에서 유저와 아이템 벡터가 놓일 경우 유저와 아이템의 유사한 정도를 확인할 수 있음

+ Matrix Factorization (MF)

  <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/195982877-a1fbe988-fc00-4ee3-9e93-09fcf54279e5.png">

+ Alternative Least Square (ALS)
  + 유저와 아이템 매트릭스를 번갈아가면서 업데이트 두 매트릭스 중 하나를 상수로 놓고 나머지 매트릭스를 업데이트 p<sub>u</sub>, q<sub>i</sub>가운데 하나를 고정하고 다른 하나로 least-square 문제를 푸는 방법
  + Sparse한 데이터에 대해 SGD 보다 더 Robust 하며 대용량 데이터를 병렬 처리하여 빠른 학습 가능

    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/195983026-b6cf5222-e2ec-4960-bf5c-145989c57149.png">


+ Bayesian Personalized Ranking
+ Word2Vec
+ Item2Vec
  

<br>
<br>

의문점: ANN 에서 priority queue를 어떻게 사용하는가?

  <img width="50%" src="https://user-images.githubusercontent.com/94548914/195974515-48267eec-1c13-412c-af62-e3a9ba176df3.png">
  <img width="50%" src="https://user-images.githubusercontent.com/94548914/195974513-56653663-7d54-42d9-9cf4-082a7963a8d8.png">


답: Section을 나눌 때 그림처럼 인접한 부분은 비슷한 Section으로(비슷한 색으로) 나누고 우선순위 정하며 Tree을 구성한뒤 가정 인접한 곳이 해당 노드에 없을때 queue 규칙에 맞게 트리를 타고 올라가 다음 탐색을 시행한다.

<br>
<br>

의문점: SGNS에서 임베딩 2개를 다음 layer로 사용한다는 부분이 이해가 잘 안됐다.   

<br>

피어세션:Cross Entrophy를 줄이는 방향으로 학습한다고 한다. Cross Entrophy 부분이 기억이 잘안나서 복습해야겠다.

## Day 25
+ Recommender System with Deep Learning
+ AutoREC
+ CDAE
+ Matrix Factorization 모델 직접 구현(과제)

<br>
<br>

의문점: CDAE에서 개별 유저에 대해서 파라미터 V<sub>u</sub> 를 학습하고 이 때문에 Collaborative한 모델이라고한다. Collaborative의 정의에 대해서 다시 살펴봐야겠다. 강의 중 이렇게 말하셨다.

<blockquote>
"각각의 유저별로 특징이 다르고 그 특징이 각각의 파라미터로 학습되며 이 모델이 Collaborative가 되었다."
</blockquote>

<br>

피어세션: NCF 모델 구현 과제중 MF와 MLP의 feaure가 다른 부분에 대해 같이 고민했다.

## Day 26

+ Alternative Least Squares 직접 구현(과제)
+ Neural Collaborative Filtering 모델 직접 구현해보기
+ AutoRec 모델 직접 구현해보기
+ Item2Vec 모델 직접 구현해보기

<br>
<br>
<br>

### 회고
이번 주는 본격적으로 RecSys 도메인에 대해 학습할 수 있었다. 특히 Youtube 관련 모델은 유튜브 알고리즘에 신기하고 성능에 놀랐던 적이 많아서 재밌고 흥미로웠다. 그러나 그 만큼 학습량이 많고 어려웠다. RecSys에 밀려 Data visualization 부분을 제대로 학습하지 못한 거 같다. 주말간 보충해야겠다.
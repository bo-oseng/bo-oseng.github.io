---
layout: single

title: 부스트캠프 AI 12주차(Day-86) Movie Recommendation - (1) SOTA of RecSys

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, SOTA, WRMF, BPR]

toc: true
---
## Day 75

# SOTA of RecSys 살펴보기

State-of-the-art (SOTA) RecSys 연구의 근간이 되는 중요한 논문들에 대해 살펴보자.

## With an instance Reweighting Matrix Factorization

### Matrix Factorization

사용자와 아이템의 저차원 표현 (low-dimensional representations)을 학습하는 방법이다.  

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207336918-bd6d74cc-6d28-4ffd-88eb-3c95f658f925.png">

명시적인 feature 를 사용하지 않고도 잠재 표현을 학습하기 때문에 Latent Factor Model이라고도 한다.


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207338016-c2ee4a1d-ef13-44c3-9c6f-b3a716d127a1.png">

 MF는 Explicit feedback 으로 이루어진 rating을 예측하는 데에도 쓰이지만, Implicit feedback으로 이루어진 데이터로부터 ranking을 수행하는 데에도 효과적으로 쓰인다.

 + Rating Prediction with Explict feeback
   + Regression Optimize error metrics ( ex] RMSE )
 + Top-K Raknking with Implicit feedback
   + Classfication approaches

### Instance re-weighting scheme

MF로 Implicit feedback을 다룰 때에는 행렬의 각 원소에 서로 다른 confidence 값을 부여하는 Instance re-weighting scheme 이 활용됨

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207340631-40dccadc-1e40-4f2c-ba97-3ec6c7dace4d.png">

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207340770-6486c224-1116-4dc5-a00f-9047f8a165d9.png">


## Bayesian Personalized Ranking

사용자의 선호도를 두 아이템 간의 Pairwise-ranking 문제로 접근한다. 각 사용자 u의 Personalized Ranking Function (i ><sub>u</sub> j)을 추정한다.


+ 기존의 방법들은 missing value를 0 으로 처리했고 그결과 선호하지 않는 아이템과, 사용자가 경험하지 않았지만 선호할지 모르는 아이템이 섞이게 되었다.
  + BPR을 통해 이를 보완했다.
+ Positive Instance (+)에는 높은 score를 주고 non-positive instance (?) 에는 낮은 score를 주어 ranking하는 형태로 변경했다.


### BPR-OPT

Maximum a Posteriori, Maximum Likelihood Estimate를 통해 OPT 유도

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207344061-712147c4-10ff-4189-8d7a-2dcbf4ed03ab.png">

MF는 pointwise (one item)의 optimzation을 BPR은 pairwise( two items)의 비교로 optimzation 갖는다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207344971-dca791e7-550e-4876-b587-0318b60536e6.png">



### AUC

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207346001-ac5d938c-b249-4ff9-a45d-4bfa3165a163.png">

BPR-OPT를 최적하 하는것이 AUC를 최적화 하는것과 유사함을 알 수 있다.

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/207346423-8e430b0e-436a-4e82-8340-4dc3e7df742d.png">
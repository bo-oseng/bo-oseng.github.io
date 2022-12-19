---
layout: single

title: 부스트캠프 AI 13주차(Day-89) Movie Recommendation - (2) SOTA of RecSys - 2

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, SOTA, FPMC, PRME]

toc: true
---

## Day 89

# SOTA of RecSys 살펴보기

State-of-the-art (SOTA) RecSys 연구의 근간이 되는 중요한 논문들에 대해 살펴보자.

## Factorizing Personalized Markov Chains

### Matrix Factorizing

사용자와 아이템의 저차원 표현 (low-dimensional representations)을 학습하는 방법이다.  

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207336918-bd6d74cc-6d28-4ffd-88eb-3c95f658f925.png">

명시적인 feature 를 사용하지 않고도 잠재 표현을 학습하기 때문에 Latent Factor Model이라고도 한다.


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/207338016-c2ee4a1d-ef13-44c3-9c6f-b3a716d127a1.png">

 MF는 Explicit feedback 으로 이루어진 rating을 예측하는 데에도 쓰이지만, Implicit feedback으로 이루어진 데이터로부터 ranking을 수행하는 데에도 효과적으로 쓰인다.

### Markov Chain

다음 단계의 상태는 이전의 상태에만 영향을 받는다는 가정을 계산하는 모델이다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208058197-6e2ce206-77af-4e11-b6d8-01fc01a09d7b.png">

### FPMC

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208058923-5cd74158-62d3-4686-95fb-cd94443ccc69.png">

MF와 Markov Chain을 결합한 모델로 사용자와 아이템 간의 관계 및 아이템과 바로 이전 아이템간의 관계를 함께 모델링한다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208058499-325004af-fd44-4202-b072-7dea4f2f3103.png">

FPMC는 사용자 별로 개인화 된 형태의 User-Specific한 Markov Chain을 가정한다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208058525-b9761a8d-22fa-4ff3-b3df-1cce6d96c04b.png">

<br>


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208059211-074b4c5a-9b75-4550-8db5-8ee868b03ce5.png">


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208059370-3a24a2c9-d9c4-4d06-9018-bae606977522.png">




## Personalized Ranking Metric Embedding

### PRME

Compatibility function으로 inner prodcut 대신 distance metric을 (ex. Euclidean distance) 사용한다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208059774-ef70775c-5ff4-44eb-8743-c2b328f73a1a.png">

+ Inner Procut
  + 가장 유사한 아이템 집단 중 각도 상으로는 가까우나 극단적으로 거리가 먼 아이템을 추천할 수 도 있음
+ Euclidean distance
  + 가장 유사한 아이템 집단을 거리를 기준으로 추천 하므로 적당한 추천이 가능함.(locality를 고려할 수 있음)
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208059950-75225d04-f314-4230-9b08-a0d0bc8ac4a4.png">


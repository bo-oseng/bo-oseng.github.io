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
+ 추천 시스템 Basic


<br>
<br>
의문점: ANN 에서 priority queue를 어떻게 사용하는가?

<br>

피어세션: 멘토님이 만들어 주신 질문에 각자 팀 노션에 답을 달아보는 시간을 가졌다.

## Day 24
+ Collaborative Filtering
+ Bayesian Personalized Ranking
+ Word2Vec
+ Item2Vec
  

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

의문점: CDAE에서 개별 유저에 대해서 파라미터 V<sub>u</sub> 를 학습하고 이 때문에 Collaborative한 모델이라고한다. Collaborative의 정의에 대해서 다시 살펴봐야겠다.

<br>
    

    "각각의 유저별로 특징이 다르고 그 특징이 각각의 파라미터로 학습되며 이 모델이 Collaborative가 되었다."

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
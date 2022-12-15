---
layout: single

title: 부스트캠프 AI 13주차(Day-88) GES-LMSTattn

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, DKT, GCN, GES, GES-LMSTattn]

toc: true
---

## Day 88

# GES-LMSTattn

DKT 대회 동안 구현했던 모델을 팀원들에게 설명하는 세미나를 가졌다.

## 구현 배경

 EDA를 통해 본 대회의 데이터는 시계열적 데이터임을 확인했다. 또한 일반적으로 시계열 데이터 학습에 뛰어난 성능을 보인다고 알려진 Recurrent 계열 모델 중 LSTM, LSTMattn, Transformer 등이 대회의 Baseline으로 주어졌다.

 하지만 위 3개 모델은 공통적으로 시계열 데이터 만을 활용한다. 때문에 MF나 GCN 에 비해 User-Item의 관계나 Item-Item의 관계에 대한 정보를 잘 활용하지 못할 것이라고 판단했다.

 Knowlege Tracing의 특성상 Item-Item의 연관 관계는 User의 학습 수준을 파악하는데 중요한 지표가 될 것으로 기대된다.때문에 GCN을 통해 Item-Item의 관계 Graph를 Embedding으로 나타내고 해당 Embedding을 Transformer의 Item Embedding 대신 사용해 Item-Item의 관계와 시계열 데이터 모두 학습이 가능한 모델을 기대하고 설계했다.



## PPT

<iframe src="https://docs.google.com/presentation/d/1MR-oMVX5Fmn1cusj16H3YNzIs9wAFSiu/edit?usp=sharing&ouid=112152832839407050731&rtpof=true&sd=true" width="800" height="600"></iframe>
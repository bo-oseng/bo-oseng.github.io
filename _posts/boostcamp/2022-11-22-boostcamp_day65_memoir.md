---
layout: single

title: 부스트캠프 AI 10주차(Day-65) 회고록, DKT 대회 - (6) Temporal Graph

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, Temporal Graph]

toc: true
---

## Day 65

# Day 65 DKT 대회 진행상황

## PyTorch Geometric Temporal 라이브러리
[KATRec 논문](https://arxiv.org/abs/2012.03323)을 이어서 연구중에 참고할만한 코드가 있나 검색을 하다보니 PyTorch Geometric Temporal 라이브러리와 GC-LSTM을 발견했다.

### GC-LSTM
[GC-LSTM](https://pytorch-geometric-temporal.readthedocs.io/en/latest/_modules/torch_geometric_temporal/signal/static_graph_temporal_signal.html) 처음엔 KATRec과 비슷한 구조로 이루어진 모델을 기대하고 모델 구조 파악을 위해 첨부된 [GC-LSTM 논문](https://arxiv.org/abs/1812.04206)을 읽어봤다.

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/203333674-468bda54-70c6-4abc-b6c5-151b7eeda42d.png">

기대한것과는 달랐지만 현재 진행중인 DKT에는 KATRec보다 Temporal graph를 활용하는 GC-LSTM이 더 잘 어울린다는 생각이 들었다.

### Temporal graph
KATRec은 5회 인용이었는데 [T-GCN](https://ieeexplore.ieee.org/abstract/document/8809901) 무려 873회 인용된 논문이 있었다. 이 논문의 아이디어로 변형시킨게 GC-LSTM인듯 하다.

Temporal graph 기본적인 아이디어는 그래프가 시간에따라 변화하는 경우 시간마다의 그래프의 Adjacent Matrix 하나하나를 입력으로 하여 Sequntial한 데이터를 만들고 이를 바탕으로 학습하는 것이다.

+ [참고자료](https://towardsdatascience.com/temporal-graph-networks-ab8f327f2efe)

### PyTorch Geometric Temporal의 Signal

이런 특징적인 grpah들과 모델들의 라이브러리가 PyTorch Geometric Temporal이고 이 라이브러리를 제대로 쓰기위해서 PyTorch Geometric의 Data 그리고 PyTorch Geometric Temporal의 Signal을 살펴봐야한다.



## Appendix

### 피어섹션 & 의문점
새로 발견한 GC-LSTM 모델을 소개했고, 강의에서 제시해줬던 KATRec에 비해 어떤 부분에서 더 좋은지에 대한 나의 직관을 소개했다.
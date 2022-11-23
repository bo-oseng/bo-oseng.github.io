---
layout: single

title: 부스트캠프 AI 10주차(Day-66) 회고록, DKT 대회 - (7) PyTorch Geometric Temporal - 2

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, PyTorch Geometric Temporal, PyG, Temporal Graph]

toc: true
---

## Day 66

# Pytorch Geometric & Pytorch Geometric Temporal

[Pytorch Geomertic Temporal](https://pytorch-geometric-temporal.readthedocs.io/en/latest/index.html) 라이브러리르 제대로 이해하고 쓰기 위해서는 일단 [Pytorch Geometric](https://pytorch-geometric.readthedocs.io/en/latest/notes/introduction.html)의 기초 지식이 선행 되어야한다.

## Data Handling of Graphs

``` torch_geometric.data.Data ```

데이터를 그래프의 형태로 나타내기 위한 방법으로 다음과 같은 속성을 가진다.

+ data.x
+ data.edge_index
+ data.edge_attr
+ data.y
+ data.pos

```python

import torch
from torch_geometric.data import Data

edge_index = torch.tensor([[0, 1, 1, 2],
                           [1, 0, 2, 1]], dtype=torch.long)
x = torch.tensor([[-1], [0], [1]], dtype=torch.float)

data = Data(x=x, edge_index=edge_index)
>>> Data(edge_index=[2, 4], x=[3, 1])

```

<img width="80%" src="https://pytorch-geometric.readthedocs.io/en/latest/_images/graph.svg" />

```python

import torch
from torch_geometric.data import Data

edge_index = torch.tensor([[0, 1],
                           [1, 0],
                           [1, 2],
                           [2, 1]], dtype=torch.long)
x = torch.tensor([[-1], [0], [1]], dtype=torch.float)

data = Data(x=x, edge_index=edge_index.t().contiguous())
>>> Data(edge_index=[2, 4], x=[3, 1])

```

## Temporal Signal Iterators

Graph가 시간에 따라 변화하는 모습의 인접행렬로 나타내기 위한 클래스다. 이 부분이 이해하기가 힘든거 같다.
아직 학습 중이라 부정확한 정보가 다수 포함되어있다.

+ StaticGraphTemporalSignal
+ DynamicGraphTemporalSignal
+ DynamicGraphStaticSignal


```python

dataset = StaticGraphTemporalSignal(
    self._edges, self._edge_weights, self.features, self.targets
)

```

dataset을 선언할 때 ```_edges```, ```_edge_weights```, ```features```, ```targets```이 필요하다.

+ _edges
+ _edge_weight
+ features
+ targets

DKT 대회 데이터셋으로 StaticGraphTemporalSignal를 만들면 바로 모델에 적용해 볼 수 있을 듯하다.

하나 걱정되는것은 DKT 대회의 데이터가 유저 - 아이템 간의 그래프로 이루어질 것이다. 그럼 PyG에서 정의한 Heterogeneous Graph에 해당하는 것 같다. Heterogeneous Graph에 대해서도 PyG Temporal에 있는 모델이 잘 학습될지를 잘 모르겠다.



## Appendix

### 피어섹션 & 의문점
Heterogeneous Graph에 대해 팀원들과 의견을 나눴고, 결론은 일단은 끝까지 해보자 였다. 대신 다른 인원은 만약을 대비해 KATRec을 구현 해보기로 했다.

 또한 KATRec의 전체적인 구조에 대한 나의 의견을 팀원들과 공유했다. 데이터를 GCN으로 학습 하면 Prediction 단계 이전에 User Embedding과 Item Embedding을 구하는데, 여기서 구한 Item Embedding을 Transforemr의 입력으로 word2vec으로 임베딩 대신 GCN을 활용하는 느낌인듯 하다.
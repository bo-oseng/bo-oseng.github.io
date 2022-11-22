---
layout: single

title: 부스트캠프 AI 10주차(Day-64) 회고록, DKT 대회 - (5) LightGCN + LSTM 구상

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, KATRec]

toc: true
---

## Day 64

# Day 64 DKT 대회 진행상황

## LightGCN + LSTM 구상

LightGCN 으로 임베딩한 정보를 Positional 정보와 함께 Sequntial하게 만든뒤 LSTM에 넣어 값을 예측하는 모델에 대해 고민해봤다.

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/203077356-07e0169c-188a-4429-a78d-d60ad9dbfe06.png">

아무래도 Positional Embedding 부분 개념이 약간 부족해서 이해가 잘 안되는거 같다. 
Transformer 부터 차근차근 다시 살펴봐야겠다.

### 참고자료
+ [KATRec 원작자 깃허브](https://github.com/DanialTaheri/KATRec)
+ [KATRec 논문](https://arxiv.org/abs/2012.03323)

## Github Issue 해결

<img width="80%" alt="스크린샷 2022-11-20 오후 5 26 50" src="https://user-images.githubusercontent.com/94548914/203078633-16bdc517-dfbc-41e7-aa69-94d8dfbf2112.png">


사진과 같이 중간에 팀원의 실수로 orgin/main의 위치가 변경되며 내 브랜치가 히스토리를 공유하는 부분이 없어지게 되었다.
이를 해결하기위해 --allow histories를 쓸까 고민했으나 결국엔 깔끔하게 새로운 브랜치를 만들었다.

## Supervised와 Unspervised 그리고 semi-supervised

[Semi-supervised learning 소개](https://blog.est.ai/2020/11/ssl/)
[inductive learning vs. transductive learning](https://komputervision.wordpress.com/2020/03/24/)

두 글을 읽고 이해하는데 굉장히 도움이 많이됐다 나중에 스스로 다시 한번 정리해봐야겠다.



## Appendix

### 피어섹션 & 의문점
DKT 대회에서 train과 test 데이터가 주어졌지만 만약 처음 등장하는 유저가 있다면 어떻게 처리 되는가?
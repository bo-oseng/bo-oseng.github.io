---
layout: single

title: 부스트캠프 AI 13주차(Day-89) Movie Recommendation - (3) 베이스라인 분석

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, SASRec]

toc: true
---

# Baseline 분석

### 배경

저는 이번 baseline이 단계마다 데이터가 어떻게 변해가지는 데이터의 흐름을 쫓아가는 과정이 무척 버거웠습니다. 디버깅으로 사이즈를 찍고 전체적인 구조를 종이에 직접 그려가며 이해했는데 확실히 시각화를 하는 게 이해하는 데 도움이 많이 된 거 같습니다.  


이 내용들을 팀원들에게 더 잘 공유하고 싶어서 draw.io를 통해 깔끔하게 도식화해봤습니다. 완성하고 나니 모두에게 공유하는 게 좋겠다는 생각이 들었습니다. 오류가 있으면 편하게 피드백 주시면 감사할 거 같습니다!  


<blockquote>
참고로 UML이라는 게 있습니다. UML에 따라 도형 모양까지 용도에 맞춰 도식화하는 게 좋은 방법이다 정도만 알고 있습니다. (저는 그냥 예뻐 보이는 도형 골라서 했습니다 ㅋㅋㅋ)
</blockquote>


## SASRec dataset

<img width=100% src='https://user-images.githubusercontent.com/94548914/208921384-00b2c5b7-80cd-4145-b410-188fad0ff550.jpg'>

+ dataset은 data type 키워드에 따라 조금씩 달라집니다. 그림에 나타낸 부분은 해당 type에서 주요하게 쓰일 값들입니다.

+ Train을 위주로 도식화했습니다.

## SASRec Model

<img width=100% src='https://user-images.githubusercontent.com/94548914/208921405-6b441228-3ca8-43ca-8fc0-4eb4e653ce14.jpg'>

+ subsequnet mask는 시퀀스의 미래 데이터를 보지 않고 출력값을 예측하기 위한 처리라고 생각하시면 됩니다. max_len이 50일 때는 한 유저당 50개의 시퀀스가 들어옵니다. 50개의 시퀀스를 하나씩 늘려가며 정보를 오픈한다고 생각하면 총 50개의 마스크가 필요합니다. 그리므로 subsequnet mask 크기는 50 x 50이 됩니다.
  
  + ex) [1, 2, 3, 4]의 시퀀스에 대해 [ [1, 0, 0, 0], [1, 2, 0, 0], [1, 2, 3, 0], [1, 2, 3, 4] ] 총 4개의 마스크가 필요합니다.

+ extended attentions mask는 각각의 유저에게 마스크를 할당하는 과정이라고 생각하시면 됩니다. user의 수 = batch size)

+ sequnec emb는 파라미터를 주어지는 hidden 사이즈만큼 Item Embedding을 만들어주고, 시퀀스의 위치 성분을 위해 Postional Embedding을 더해준 값입니다.

+ item encoded layers가 실제로 모델의 연산이 진행되는 부분입니다. extended attentions mask와 sequnec emb 입력으로 받습니다.

+ 모델의 최종적인 output인 sequence output의 사이즈는 (256, 50, 64)입니다. 각각 (batch, maxlen, hiddensize) 을 의미하고 풀어쓰자면 각 유저의(batch) 시퀀스마다(maxlen) Item Embedding(hidden size) 값을 의미합니다.
  
  + sequence output[0]는 0번째 유저의 50개의 각 시퀀스에 대한 Item embedding 모두를 의미합니다.(50개의 임베딩)
  
  + sequence output[0][-1]은 0번째 유저의 마지막 시퀀스에 대한 Item embedding을 의미합니다.(1개의 임베딩)
  
  + sequence output[0][-1][0]은 0번째 유저의 마지막 시퀀스에 대한 Item Embedding의 0번째 인덱스를 룩업한 값을 의미합니다. (hidden size 개수 만큼의 리스트)




## Finetune trainer

<img width=100% src='https://user-images.githubusercontent.com/94548914/208921420-96404ee1-bd77-4e31-90f8-7dfd5be1598d.jpg'>

+ train과 eval 모두 finetune trainer를 통해 진행됩니다.

+ Loss 함수는 SASRecModel에 input_ids를 입력했을 때 출력값 sequence out, SASRecDataset의 target pos, target neg를 입력받아 Cross Entropy를 계산하고 모델은 해당 Loss를 backward로 학습합니다.

+ eval은 SASRecModel에 input_ids를 입력했을 때 출력값 recommned_output, SASRecDataset의 answer의 조합으로 predictlist를 만듭니다.
  
  + eval의 자세한 내용은 김재인 조교님이 준비해 주신 오피스아워 자료와 강의가 이미 너무 훌륭한거 같아서 생략하겠습니다.


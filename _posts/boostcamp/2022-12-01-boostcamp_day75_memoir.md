---
layout: single

title: 부스트캠프 AI 10주차(Day-75) 회고록, DKT 대회 - (11) Feature Engineering

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회, Feature Engineering]

toc: true
---

## Day 75

# DKT 대회 Feature Engineering

## Feature Engineering

### LightGBM 대조군

Default Featue  

+ KnowledgeTag: 원본 데이터의 KnowledgeTag
+ user_correct_answer: 유저의 현시점 누적 정답수
+ user_total_answer: 유저의 현시점 누적 문항수
+ user_acc: 유저의 현시점 정답률

+ test_mean: 테스트별 평균 정답률
+ test_sum: 테스트별 정답자 총합
+ test_std: 테스트별 표준편차

+ tag_mean: 태그별 평균 정답률
+ tag_sum: 태그별 정답자 총합
+ tag_std: 태그별 표준편차



<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250069-c823c0a1-ffdb-4b16-a9d3-36e70faffa87.png">  

<br>

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250140-f0d96a09-e679-4308-8237-d4b03f141223.png">

<br>


<img width="100%%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250281-6c5f2c73-c019-4233-8542-66bfb0d1065b.png">

<br>



```python
Training until validation scores don't improve for 500 rounds
[100]	training's binary_logloss: 0.559427	valid_1's binary_logloss: 0.681014
[200]	training's binary_logloss: 0.556528	valid_1's binary_logloss: 0.680677
[300]	training's binary_logloss: 0.554283	valid_1's binary_logloss: 0.681167
[400]	training's binary_logloss: 0.552284	valid_1's binary_logloss: 0.681011
[500]	training's binary_logloss: 0.550545	valid_1's binary_logloss: 0.681547
[600]	training's binary_logloss: 0.548883	valid_1's binary_logloss: 0.681466
Early stopping, best iteration is:
[123]	training's binary_logloss: 0.558699	valid_1's binary_logloss: 0.680181
VALID AUC : 0.6904581594116477 ACC : 0.6098654708520179
```


### assessmentItemID 별 평균, 분산 , 총합 (assess_mean, assess_std, assess_sum)

+ assess_mean: 문항별 평균 정답률
+ assess_std: 문항별 정답자 총합
+ assess_sum: 문항별 표준표차
  
###  assessment ID 처음 세자리 카테고리화 후 평균, 분산 ,총합 (prefix, prefix_mean, prefix_std, prefix_sum)

+ prefix: 문항 대분류 A'XXX'0000000
+ prefix_mean: 대분류 별 정답률
+ prefix_std: 대분류 별 표준편차
+ prefix_sum: 대분류 별 정답자 총합
  
### weekday, month, day, hour 별  mean, std 추가

+ weekday: 문제를 푼 요일
+ weekday_mean: 요일별 평균 정답률
+ weekday_std: 요일별 표준편차

+ month: 문제를 푼 월
+ month_mean: 월별 평균 정답률
+ month_std: 월별 표준편차

+ day: 문제를 푼 일
+ day_mean: 일별 평균 정답률
+ day_std: 일별 표준편차

+ hour: 문제를 푼 시간
+ hour_mean: 시간별 평균 정답률
+ hour_std: 시간별 표준편차


### 문제별 풀이에 걸린시간 (이상치는 정상치들의 전체 평균 풀이시간으로 대체)

+ elapsedTime: 문항별 풀이에 걸린 시간



<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250756-315120a1-664a-42ac-af88-08bdf5e9bb55.png">  

<br>


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250817-c81017fd-3881-4e8d-bfe6-152e2b74882d.png">  

<br>


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/205250931-985e5d7d-a52d-4e16-bf28-fa7d1c7c6972.png">

<br>


```python

[LightGBM] [Info] Start training from score 0.642855
Training until validation scores don't improve for 100 rounds
[100]	training's binary_logloss: 0.477544	valid_1's binary_logloss: 0.593096
[200]	training's binary_logloss: 0.474668	valid_1's binary_logloss: 0.593779
Early stopping, best iteration is:
[114]	training's binary_logloss: 0.477068	valid_1's binary_logloss: 0.592838
VALID AUC : 0.7473235937189425 ACC : 0.680119581464873

```
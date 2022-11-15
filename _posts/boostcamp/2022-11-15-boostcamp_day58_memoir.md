---
layout: single

title: 부스트캠프 AI 9주차(Day-57) 회고록, DKT 대회 - (2) DKT EDA

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회]

toc: true
---

## Day 58

# DKT Exploratory Data Analysis

입력 데이터와 해결 해야할 문제와의 직관이나 연관성을 얻기 위해 데이터 분석을 진행한다. EDA를 통해 문제 정의를 더 구체화 하거나 실마리를 얻을 수 있다.

## i-Scream 데이터 분석

### 입력 데이터의 형태

<center>
    <img width="100%" alt="image" src="https://user-images.githubusercontent.com/94548914/201923851-c7723d46-6be4-410f-957c-7d30296761e3.png">
</center>

+ userID
  + 사용자 별 고유번호
  + 총 7,442명의 고유한사용자 존재
+ assessmentItemID
  + 사용자가 푼 문항의 일련 번호
  + 총 9,454개의 고유한 문항이 존재
  + 총 10자리로 구성 일련 번호의 규칙은 아래와 같음.
    + 첫 자리는 항상 알파벳 A
    + 그 다음 6자리는 시험지 번호
    + 마지막 3자리는 시험지 내 문항의 번호
    + ex) A030071005
+ testID
  + 사용자가 푼 문항이 포함된 시험지의 일련 번호
  + 총 1,537개의 고유한 시험지가 존재
  + 10자리로 구성 일련 번호의 규칙은 아래와 같음.
    + 첫 자리는 항상 알파벳 A
    + 그 다음 9자리 중 앞의 3자리와 끝의 3자리가 시험지 번호
      + 앞의 3자리 중 가운데 자리만 1~9 값을 가지며 나머지는 모두 0 이다, 이를 대분류라는 Feature로 활용할 수 있다.
    + 가운데 3자리는 모두 000
+ answerCode
  + 사용자가 문항을 맞았는지 여부를 담은 이진 데이터
  + 전체 Interaction에 대해 65.45%가 정답을 맞춤
  + 0은 해당 문항을 틀린 것, 1은 해당 문항을 맞은 것
+ Timestamp
  + 사용자가 Interaction을 시작한 시간 정보
+ KnowledgeTag
  + 문항 당 하나씩 배정되는 태그
  + 일종의 중분류 역할
  + 총 912개의 고유 태그가 존재


### 기술 통계량 분석

보통 데이터 자체의 정보를 수치로 요약, 단순화하는 것을 목적으로 하며 우리가 잘 알고 있는 평균, 중앙값, 최대/최소와 같은 값들을 뽑아내고, EDA 과정에서는 이들을 유의미하게 시각화하는 작업을 거칩니다.

#### 사용자 분석

+ 한 사용자가 몇 개의 문항을 풀었는지 (평균 339 문항, 최소 9문항, 최대 1,860문항)
  
    <img width="406" alt="image" src="https://user-images.githubusercontent.com/94548914/201927270-d42802e0-aeb7-416d-83fe-21e657442fe4.png">

+ 학생 별로 정답률이 어떻게 되는지 (평균 62.8%, 최소 0.0%, 최대 100.0%, 중앙값 65.1%)
  
    <img width="406" alt="image" src="https://user-images.githubusercontent.com/94548914/201927495-300935ad-8cbb-4ebf-9956-296f1bb5a384.png">


#### 문항 별 / 시험지 별 정답률 분석

+ 문항들의 정답률 추이가 어떻게 되는지 (평균 65.4%, 최소 4%, 최대 99.67%)

    <img width="406" alt="image" src="https://user-images.githubusercontent.com/94548914/201927601-39e52919-bc6c-46b4-875d-e0f624e43fca.png">

+ 시험지 별로 정답률이 어떻게 되는지 (평균 62.8%, 최소 0.0%, 최대 100.0%, 중앙값 65.1%)

    <img width="406" alt="image" src="https://user-images.githubusercontent.com/94548914/201927658-9567cd57-f7a6-4753-8da5-d6c802f63a1a.png">


### EDA
+ 문항을 더 많이 푼 학생이 문제를 더 잘 맞추는가?

    <img width="382" alt="image" src="https://user-images.githubusercontent.com/94548914/201927826-456058cb-afda-4412-91e8-eab0a5dbba7e.png">

+ 더 많이 노출된 태그가 정답률이 더 높은가?

    <img width="382" alt="image" src="https://user-images.githubusercontent.com/94548914/201927909-0b3c54d7-b37b-4074-bfe9-99d4ee621c29.png">

+ 문항을 풀수록 실력이 늘어나는가?

    <img width="402" alt="image" src="https://user-images.githubusercontent.com/94548914/201928486-9a465efe-d7e4-4654-839d-ddc9ee4b0815.png">

+ 문항을 푸는 데 걸린 시간과 정답률 사이의 관계는?

    <img width="419" alt="image" src="https://user-images.githubusercontent.com/94548914/201927996-3ff8403b-89e4-4fb6-936e-4c588da5d4c9.png">


---
## Appendix

### 의문점
왜 git fetch로 .gitignore는 생성이 안되는지 이상하다.

### 피어섹션
- 다같이 aistages 서버 git 설정을 살펴봤다.
- 우팀소를 작성했다.

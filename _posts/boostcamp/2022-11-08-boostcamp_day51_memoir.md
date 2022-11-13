---
layout: single

  

title: 부스트캠프 AI 8주차(Day-51) 회고록, Product Serving 1

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, MLOps, ucbrise, mlflow, feast]


toc: true
---

## Day 51

## MLOps 개론

### 모델 개발 프로세스 - Research

Research는 보통 자신의 컴퓨터나 서버 인스터스등에서 실행한다. 또한 고정된 데이터를 사용해 학습 한다.  

+ 문제 정의
+ EDA
+ Feature Engineering
+ Train
+ Predict


<br>

### 모델 개발 프로세스 - Production

학습된(Research) 모델을 앱, 웹 서비스에서 사용할 수 있도록 만드는 과정이 필요하다. 이런 경우 Real world 또는 Production 환경에 모델을 배포한다고 표현한다.  

웹, 앱 서비스에서 활용할 수 있게 만드는 과정을 Production 이라한다. "모델에게 Input을 제공하면서, Output을 예측해주세요" 라고 요청하는 일련의 과정을 일컫는다.

<br>

Productuon시 자주 발생 될 이슈에 대해 생각 해보자.
+ 모델의 결과값이 이상한 경우가 존재할 때?
  + 원인 파악 필요
  + Input 데이터가 이상한 경우가 존재(x값이 0~100 이내여야 하는데 Input으로 200이 들어오는 경우)
  + Research 할 땐 Outlier로 제외할 수 있지만, 실제 서비스에선 제외가 힘든 상황(별도 처리 필요)
+ 모델의 성능이 계속 변경될 때?
  + 모델의 성능은 어떻게 확인할 수 있을까?
  + 예측 값과 실제 레이블을 알아야 함
  + 정형(Tabular) 데이터에서는 정확히 알 수 있지만, 비정형 데이터(이미지 등)는 잘 모를 수 있음
+ 새로 적용한 모델이 더 안 좋다면?
  + 그 모델을 다시 사용해야 함
  + Research 환경에선 성능이 더 좋았던 모델이 Production 환경에선 더 좋지 않을 수 있음
  + 이전 모델을 다시 사용하기 위한 작업이 필요
  
<br>

다양한 이슈들을 잘 다룰수 있게 도와주는 다양한 툴과 라이브러리들이 존재한다.

<br>

### MLOps란?
+ MLOps = ML (Machine Learning) + Ops (Operations)
+ 머신러닝 모델을 운영하면서 반복적으로 필요한 업무를 자동화시키는 과정을 말한다.
+ 머신러닝 모델 개발(ML Dev)과 머신러닝 모델 운영(Ops)에서 사용되는 문제, 반복을 최소화하고 비즈니스 가치를 창출하는 것이 목표이다.
+ 모델링에 집중할 수 있도록 관련된 인프라를 만들고, 자동으로 운영되도록 만드는 일을 말한다.


<img width="80%" alt="스크린샷 2022-11-13 오후 2 30 33" src="https://user-images.githubusercontent.com/94548914/201507430-e6e588e5-2ba0-4d39-b28b-bd1d8055d203.png">

<br>

<strong>MLOps의 가장 큰 목표는 빠른 시간 내에 가장 적은 위험을 부담하며 아이디어 단계부터 Production 단계까지 ML 프로젝트를 진행할 수 있도록 기술적 마찰을 줄이는 것</strong>

<br>

## MLOps Component
    각 Component에서 해결하고 싶은 문제는 무엇인지 파악하며 학습하자.

집에서 만들던 음식을 레스토랑으로 열어 대규모로 제공한다고 비유해 봅시다.

[ 집 = Research, 레스토랑 = Production ]

[ 음식 = 모델 ]

[ 식재료, 소스, 밀가루 빵 = Data / Feature ]

[ 요리하는 행위 = 모델 Train ]

<img width="80%" alt="스크린샷 2022-11-13 오후 2 56 51" src="https://user-images.githubusercontent.com/94548914/201508077-5b020202-84df-4e20-b995-bd09a9adaef7.png">


<br>

### Infra(Server, GPU)

[ 집에선 그냥 요리하면 되지만, 레스토랑을 위해선 “장소”가 필요합니다 ]

장소 선정에는 고려할 부분이 많습니다.(Server)
- 유동 인구가 얼마나 있는가(예상되는 트래픽이 얼마나 되는가?)
- 가게의 평수(서버의 CPU, Memory 성능은 어느정도 할 것인가?)
- 점포 확장이 가능한가(스케일 업, 스케일 아웃이 가능한가)
- 직접 장소를 구입할지? 월세로 사용할지?(자체 서버 구축, 클라우드)

요리도구 식기 등(GPU)
- 레스토랑에서 도구와 식기를 매번 사는것 보다 임대하는 방법도 고려 할 수 있습니다.(클라우드 GPU)


### Serving

[ 정기 배송처럼 매일 20시마다 서빙받길 원하는 경우엔 Batch Serving을 하게 됨 ]

+ Batch Serving은 많은 양(=많은 데이터)을 일정 주기(1일, 1주, 1달 등)로 한꺼번에 음식 서빙(=예측)


[ 주문하자마자 만들어서 실시간으로 전달하는 경우엔 Online Serving이라 함 ]

+ Online Serving은 한번에 하나씩(=실시간으로) 포장해서 배송(=예측) 동시에 여러 주문이 들어올 경우 병목이 없어야 하고, 확장 가능하도록 준비해야 함

<img width="80%" alt="스크린샷 2022-11-13 오후 2 59 14" src="https://user-images.githubusercontent.com/94548914/201508129-9f8c8a32-dfb7-4706-9fac-0dc10486671b.png">

<span colr="grey">출처: https://cloud.google.com/resources/mlops-whitepaper</span>

+ ucbrise/cliper

<br>

### Experiment, Model Management 
[ 요리 할 때 시도 했던 여러가지 레시피 중 가장 맛있었던 조합을 기억한다. ]
[ 요리 만드는 과정에서 생기는 부산물을 저장한다.(=모델Artifact,이미지등) ]

+ 또 다양한 요리 중 언제 만든 요리인지(모델 생성일), 얼마나 맛있었는지(모델 성능), 유통기한(모델 메타 정보) 등을 기록해 둘 수 있음.

<img width="80%" alt="스크린샷 2022-11-13 오후 5 32 57" src="https://user-images.githubusercontent.com/94548914/201513010-67e5a69d-4ea9-4adb-bada-d3b4364fc603.png">

<span colr="grey">출처: https://cloud.google.com/resources/mlops-whitepaper</span>


<br>

+ mlflow

### Feature Store

[ 요리별로 사용되는 재료들이 중복됨. 반죽이나 간 등을 미리 만들면 편함 ]
[ 이런 재료를 가공해서 냉장고에 저장(=머신러닝 Feature를 집계한 Feature Store) ]
[ 집과 레스토랑에서 같은 재료를 사용하도록 냉장고 구축 ]

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/201513116-eb9fba44-6a6a-47cd-8b03-9e975fc101c8.png">

<span colr="grey">출처: https://cloud.google.com/resources/mlops-whitepaper</span>

<br>

+ feast

### Data Validation

[ 재료들이 예전에 요리할 때 사용한 재료와 비슷한지 확인할 필요가 있습니다(=Feature의 분포 확인) ]

만약 재료(Data)가 변하면 음식(Output)이 변질 되므로 재료를 잘 관리해야 한다.

+ TFDV
+ AWS Deequ

### Continuous Training

[ 만들었던 요리를 언제부터 고객분들이 좋아하지 않습니다. 신선한 재료로 다시 만듭시다(Retrain) ]

다시 요리하는 경우
1. 신선한 재료가 도착한 경우(=새로운 데이터) 2) 일정 기간(매일, 매시간)
2. 갑자기 매출이 줄어들 경우(Metric 기반)
3. 요청시

### Monitoring

[ 레스토랑의 매출, 손님 수, 대기열 등(=모델의 지표, 인프라의 성능 지표 등)을 잘 기록해야 함. 집에서 만든 요리법이 레스토랑에서 얼마나 많이 팔리는지 확인해서 추가로 아이디어 얻을 수 있음 ]

+ 얼마나 판매되었는지?
+ 동시에 주문이 많이 몰리는 시기가 있었는지?

### AutoML

[ 시대가 좋아져서 자동으로 음식을 만들 수도 있음 => 에어프라이어(AutoML)에 넣기만 하면! (집 / 레스토랑 모두 활용 가능) ]

+ NNI
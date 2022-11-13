---
layout: single

title: 부스트캠프 AI 8주차(Day-52) 회고록, Product Serving 2

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, online vs batch]

toc: true
---

## Day 52

## Product Serving 개론

### Model Serving
### Serving Basic

[ 요리를 만들면 이제 손님에게 서빙해야죠! ]

[ 정기 배송처럼 매일 20시마다 받길 원하는 경우엔 Batch Serving을 사용 ]

[ 주문하자마자 만들어서 전달하는 경우엔 Online Serving을 사용 ]

Serving
- Production(Real World) 환경에 모델을 사용할 수 있도록 배포
- 머신러닝 모델을 개발하고, 현실 세계(앱, 웹)에서 사용할 수 있게 만드는 행위 - 서비스화라고 표현할 수도 있음
- 머신러닝 모델을 회사 서비스의 기능 중 하나로 활용
- Input이 제공되면 모델이 예측 값(Output)을 반환


Serving : 모델을 웹/앱 서비스에 배포하는 과정, 모델을 활용하는 방식, 모델을 서비스화하는 관점

Inference : 모델에 데이터가 제공되어 예측하는 경우, 사용하는 관점

Serving - Inference 용어가 혼재되어 사용되는 경우도 존재

<br>

#### Online Serving Basic

#### Web Server Basic

Web Server(Wikipedia)
- HTTP를 통해 웹 브라우저에서 요청하는 HTML 문서나 오브젝트를 전송해주는 서비스 프로그램 - 요청(Request)을 받으면 요청한 내용을 보내주는(Response) 프로그램

머신러닝 모델 서버
- 어떤 데이터(Input)를 제공하며 예측해달라고 요청(Request)하면, 모델을 사용해 예측 값을 반환(Response)하는 서버

#### API

API(Application Programming Interface) 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

### Online Serving

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/201519645-69b30ad1-5df0-4d90-b3ee-8d66fa2e5074.png">


Online Serving을 구현하는 방식
- 직접 API 웹 서버 개발 : Flask, FastAPI 등을 사용해 서버 구축
- 클라우드 서비스 활용 : AWS의 SageMaker, GCP의 Vertex AI 등
- Serving 라이브러리 활용 : Tensorflow Serving, Torch Serve, MLFlow, BentoML 등


### Batch Serving

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/201519668-a5cf9d44-8180-486a-a536-b97e85bde786.png">

    Workflow Scheduler: 위 작업을 특정 기간 단위(하루, 1시간) 등으로 실행 10시에 python main.py, 11시에 python main.py

Batch Serving은 주기적으로 학습을 하거나 예측을 하는 경우에 사용한다.
- 30분에 1번씩 최근 데이터를 가지고 예측
- Batch 묶음(30분의 데이터)를 한번에 예측
- 모델의 활용 방식에 따라 30분일 수도 있고, 1주일, 하루 단위일 수 있음 - 한번에 많은 예측을 실행
- 특정 시간에 반복해서 실행
- Airflow, Cron Job 등으로 스케쥴링 작업(Workflow Scheduler)

#### Online Serving vs Batch Serving

##### Intput 관점
- Input 관점 데이터 하나씩 요청하는 경우 : Online
- 여러가지 데이터가 한꺼번에 처리되는 경우 : Batch



##### Output 관점
인퍼런스 Output을 어떻게 활용하는지에 따라 다름
- API 형태로 바로 결과를 반환해야 하는 경우 : Online 
- 서버와 통신이 필요한 경우 : Online
- 1시간에 1번씩 예측해도 괜찮은 경우 : Batch
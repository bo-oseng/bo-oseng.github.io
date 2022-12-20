---
layout: single

title: 부스트캠프 AI 13주차(Day-92) Movie Recommendation - (3) SOTA of RecSys - 3

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, SOTA, Auto Encoder, VAE]

toc: true
---

## Day 89

# Variational AutoEncoder

+ 참고자료
  + [오토인코더의 모든것 - 이활석님](https://www.youtube.com/watch?v=o_peo6U7IRM&ab_channel=naverd2)

## 배경지식

### Maximum Likelihood Estimation

<img width="80%" src="https://user-images.githubusercontent.com/94548914/208682967-3a04f946-7e50-4fd6-b6bf-a656e93b0578.png">

+ **모델 출력값이 Given** 될 때 **우리가 원하는 정답**(Ground Truth)이 나올 확률이 높길 바란다.  

+ 모델 출력값이 Given 될 때 원하는 정답이 나올 확률 즉 조건부 확률이다.

  → 조건부 확률의 분포 자체를 가정한다. 
  
  → ex) 조건부 확률이 Gaussian 분포를 따를 것이다 가정.

+ **가정한 확률분포**에서 출력Y가 나올 가능도
  
  → Likelihood  

  → 모델의 모수(모평균, 모분산)에 따라 그 값이 변함.

+ Maximum Likelihood Estimation   
  
  → Likelihood를 최대로 하는 모수를 찾자.  

  → Likelihood의 미분 값이 0이 되는 지점이 최대값을 가질 것이다.

  → log Likelihood의 미분 값이 0이 되는 지점을 찾아도 같은 지점일 것이다.

  → 추정한 모수가 Y의 평균, 분산과 같을때 최대일 것이다.

  <img width="80%" alt="스크린샷 2022-12-20 오후 11 08 45" src="https://user-images.githubusercontent.com/94548914/208686655-b44f991e-aca1-46e0-8763-772d596f7359.png">

  <img width="80%" alt="스크린샷 2022-12-20 오후 11 11 14" src="https://user-images.githubusercontent.com/94548914/208686650-fd4651d6-0540-408c-8483-6cc16de16075.png">

  → 확률 분포간의 거리를 나타내는 KL발산이 가장 낮을때 Likelihood가 최대일 것이다.

  → MLE로 찾은 모수에서 **모델 출력값이 Given 될 때 원하는 정답이 나올 확률**이 최대가 될 것이다.

#### MLE의 결론
+ 입력 X가 주어졌을때 모델$f_{\theta }\left( \cdot \right)$을 거쳐 $\omega$를 출력할 때, $\omega$는 MLE에 의해 가정한 Y의 분포를 가장 잘 나타내는 분포를 따르는 모델의 출력값이다.
+ **확률 분포$f_{\theta }\left( \cdot \right)$를 모델링 했으므로 모델을 바탕으로 데이터 샘플링이 가능하다.**
  + 가장 큰 장점
  + VAE에서 MLE를 활용하는 핵심 이유

<br>
<br>

### MLE와 MSE, CE의 관계

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/208394106-ed471798-dc00-4030-ba5e-6b49733b7447.png">

<span style="font-size:70%; color:grey;">(출처: http://videolectures.net/kdd2014-bengio_deep_learing/)</span>

+ 모델이 **Gaussian 분포**를 따른다고 가정하고 MLE을 한다는건 그 식을 정리하면 **MSE**를 구한다는 것과 의미가 같다.  
  
  → Backpropagtion의 Loss로서 MSE를 사용하는 것과 큰 틀에서 의미가 같다.

+ 모델이 **Bernoulli 분포**를 따른다고 가정하고 MLE을 한다는건 그 식을 정리하면 **Cross Entropy**를 구한다는 것과 의미가 같다.  
  
  →  Backpropagtion의 Loss로서 Cross Entropy 사용하는 것과 큰 틀에서 의미가 같다.

### AutoEncoder

Keyword
+ Unsupervised learning
+ ML density estimation
+ Generative model learning
+ **Manifold learning**



## Variational AutoEncoder

Keyword
+ **Generative Model Learning**

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208398166-9f833e5c-1ac2-4cba-9f60-3ca84190e6f2.png">

<br>

Z는 Latent Variable로서 리모콘의 버튼과 같은 역할을 해줄거다. (ex] 1번 버튼을 누르면 남성의 이미지가 2번의 버튼을 누르면 여성의 이미지가 나온다.)

<br>

Z를 X로 Genrate 하는 함수중 가장 X를 잘 설명하게끔 해주는 함수 G를 추정해보자.

→ Marginal 적분을 통해 p(x)가 최대가 되는 G를 구해보자.

$$\int p\left(  x| g_{\theta}\left( z\right) \right) p\left( z\right) \cdot dz = p(x) $$ 
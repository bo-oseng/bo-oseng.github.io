---
layout: single

title: Variational AutoEncoder 정리

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, MLE, Autoencoder, VAE]

toc: true
---

## Day 92

# Variational AutoEncoder

+ 참고자료
  + [오토인코더의 모든것 - 이활석님](https://www.youtube.com/watch?v=o_peo6U7IRM&ab_channel=naverd2)

## 필요 배경지식

### Maximum Likelihood Estimation

<img width="80%" src="https://user-images.githubusercontent.com/94548914/208682967-3a04f946-7e50-4fd6-b6bf-a656e93b0578.png">

+ **모델 출력값이 Given** 될 때 **우리가 원하는 정답**(Ground Truth)이 나올 확률이 높길 바란다.  

+ 모델 출력값이 Given 될 때 원하는 정답이 나올 확률 즉 조건부 확률이다.

  → 조건부 확률의 분포 자체를 가정한다. 
  
  → ex) 조건부 확률이 Gaussian 분포를 따를 것이다 가정.

+ **가정한 확률분포**에서 출력Y가 나올 가능도
  
  → Likelihood  

  → 추정할 모델의 모수(모평균, 모분산)에 따라 그 값이 변함.

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
+ 입력 X가 주어졌을때 모델$f_{\theta }\left( \cdot \right)$을 거쳐 $\omega$를 출력한다. 이때 $\omega$는 MLE에 의해 가정한 Y의 분포를 가장 잘 나타내는 분포를 따르는 모델$f_{\theta }\left( \cdot \right)$의 출력값이다.
+ **$f_{\theta }\left( \cdot \right)$를 확률 분포로 모델링 했으므로 해당 확률 분포를 바탕으로 데이터 샘플링이 가능하다.**
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


### Manifold learning
   
Manifold(고차원 데이터를 공간에 흩뿌렸을 때 모든 데이터를 에러 없이 아우르는 곡면)을 나타내는 함수가 있을 것이다 해당 함수를 잘 찾아서 Projection을 시키면 데이터 압축이 가능할 것이다.

<img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/208699316-1c116d51-d872-482e-935e-ec243185e605.png">

<span style="font-size:70%; color:grey;">(출처: https://www.semanticscholar.org/paper/Algorithms-for-manifold-learning-Cayton/100dcf6aa83ac559c83518c8a41676b1a3a55fc0)</span>
   
+ What is useful for?

  1. Data compression
  2. Data visualization
  3. Curse of dimensionality
  4. Discovering most important features

<br>
<br>

+ Reasnable distance metric

  <img width="90%" alt="스크린샷 2022-12-21 오전 12 00 12" src="https://user-images.githubusercontent.com/94548914/208697609-f1aabe87-dc02-4c50-8ce9-9454cf33a927.png">

  <img width="90%" alt="스크린샷 2022-12-21 오전 12 00 24" src="https://user-images.githubusercontent.com/94548914/208697616-7de7f661-13ed-4dcd-a310-3dd21c57ec4f.png">

  <img width="90%" alt="스크린샷 2022-12-21 오전 12 00 32" src="https://user-images.githubusercontent.com/94548914/208697619-53da5390-e2f0-4789-963d-cffea816a05f.png">


  <span style="font-size:70%; color:grey;">(출처: https://www.slideshare.net/NaverEngineering/ss-96581209 이활석님 강의자료)</span>

  단순 Euclidean distance보다 Manifold 곡면을 따라 거리를 측정하는게 더 Reasonable하다.

### AutoEncoder

AutoEncoder의 가장 중요한 기능 중 하나는 Manifold learning이다.


<img width="80%" src="https://user-images.githubusercontent.com/94548914/208701322-10fdcd40-1a6d-4ac7-a94c-e4709d6fab39.png">

Keyword
+ Unsupervised learning
+ ML density estimation
+ Generative model learning
+ **Manifold learning**



## Variational AutoEncoder

Variational AutoEncoder은 Generative가 모델의 목적이다. 목적부터 AutoEncoder와는 조금 다르다. Generative를 더 잘 하려고 모델을 설계하다 보니 모델의 모양이 AutoEncoder와 비슷해졌다.

Keyword
+ **Generative Model Learning**

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208398166-9f833e5c-1ac2-4cba-9f60-3ca84190e6f2.png">

<br>

+ Z는 Latent Variable로서 리모콘의 버튼과 같은 역할을 해줄것이다. 

  → ex] 1번 버튼을 누르면 남성의 이미지가 2번의 버튼을 누르면 여성의 이미지가 나온다.

+ Z를 X로 Genrate 하는 함수중 가장 X를 잘 설명하게끔 해주는 함수 G를 추정해보자.

  → Marginal 적분을 통해 p(x)가 최대가 되는 G를 구해보자.

$$\int p\left(  x| g_{\theta}\left( z\right) \right) p\left( z\right) \cdot dz = p(x) $$ 
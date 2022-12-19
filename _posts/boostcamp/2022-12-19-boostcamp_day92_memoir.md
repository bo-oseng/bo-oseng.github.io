---
layout: single

title: 부스트캠프 AI 13주차(Day-92) Movie Recommendation - (3) SOTA of RecSys - 3

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Movie Recommenddation, SOTA, Auto Encoder, VAE]

toc: true

published: False
---

## Day 89

# SOTA of RecSys 살펴보기

State-of-the-art (SOTA) RecSys 연구의 근간이 되는 중요한 논문들에 대해 살펴보자.

+ 참고자료
  + [오토인코더의 모든것 - 이활석님](https://www.youtube.com/watch?v=o_peo6U7IRM&ab_channel=naverd2)

## Variational Auto-Encoder

### Maximum Likelihood Estimaation


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208394106-ed471798-dc00-4030-ba5e-6b49733b7447.png">

<span colr="grey">출처: http://videolectures.net/kdd2014-bengio_deep_learing/</span>

MLE란 주어진 데이터를 제일 잘 설명하는 모델의 확률분포를 가정하고 Likelihood가 최대가 되는 분포를 추정하는 방법이다.

Gaussian Distribution 분포를 따른다고 가정하고 MLE을 한다는건 MSE를 구한다는 것과 의미가 같고, Bernoulli distribution 분포를 따른다고 가정하고 MLE룰 구한다는 건 Cross Entropy를 구한다는 것과 의미가 같다.

### Manifold Learning

### Variational AutoEncoder

+ Generative Model Learning

<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/208398166-9f833e5c-1ac2-4cba-9f60-3ca84190e6f2.png">

<br>

Z는 Latent Variable로서 리모콘의 버튼과 같은 역할을 해줄거다. (ex] 1번 버튼을 누르면 남성의 데이터가 2번의 버튼을 누르면 여성의 이미지가 나온다.)

<br>

Z를 X로 Genrate 하는 함수중 가장 X를 잘 설명하게끔 해주는 함수 G를 추정해보자.

→ Marginal 적분을 통해 p(x)가 최대가 되는 G를 구해보자.

$$\int p\left(  x| g_{\theta}\left( z\right) \right) p\left( z\right) \cdot dz = p(x) $$ 
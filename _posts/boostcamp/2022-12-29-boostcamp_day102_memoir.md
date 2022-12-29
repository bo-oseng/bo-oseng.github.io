---
layout: single

title: Variational AutoEncoder 정리 - (2) Variational AutoEncoder

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, Autoencoder, VAE]

toc: true
---

## Day 92

# Variational AutoEncoder

+ 참고자료
  + [오토인코더의 모든것 - 이활석님](https://www.youtube.com/watch?v=o_peo6U7IRM&ab_channel=naverd2)

[Variational AutoEncoder 정리 - (1) Intro](https://bo-oseng.github.io/boostcamp/boostcamp_day92_memoir/)

## Variational AutoEncoder

Variational AutoEncoder은 Generative가 모델의 목적이다. 

목적부터 AutoEncoder와는 조금 다르다.

Generative를 더 잘 하려고 모델을 설계하다 보니 공교롭게도 모델의 모양이 AutoEncoder와 비슷해졌다.

Keyword
+ **Generative Model Learning**


<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/209950644-cc8d17e6-d887-41fa-9d52-c7032765e8b0.png">

<span style="font-size:70%; color:grey;">(출처: https://www.slideshare.net/NaverEngineering/ss-96581209 이활석님 강의자료)</span>


+ Z는 manifold learnig을 통해 추출된 Latent Variable 이다.
+ Z는 Latent Variable로서 리모콘의 버튼과 같은 역할을 해줄것이다. 

→ ex) 1번 버튼을 조작하면 남성의 이미지가 2번의 버튼을 조작하면 여성의 이미지가 나온다. 

+ Training set에 있는 각각의 X가 Generative 과정을 거친뒤 나올 확률을 최대화 하는걸 목표로 한다.

→ Marginal 적분을 통해 p(x)가 최대가 되는 G를 구해보자.

→ 샘플링 된 값을 쉽게 컨트롤하기 위해 Z를 다루기 쉬운 확률분포로 접근하자.(가정)

$$ \int p\left(  x| g_{\theta}\left( z\right) \right) p\left( z\right) \cdot dz = p(x) $$

### Why dont't we use maximum likelihood estimation directly?

$$ P\left( x\right) \approx \sum _{i}P\left(  x| g_{\theta }\left( z_{i}\right) \right)p\left( z_{i}\right) $$

생성기에 대한 확률 모델 $P\left(  x| g_{\theta }\left( z_{i}\right) \right)$ 
을 가우시안으로 할 경우, MSE 관점에서 가까운 것이 더 p(x)에 기여하는 바가 크다.

<img width='90%' src='https://user-images.githubusercontent.com/94548914/209943672-f0a6ad99-d16f-471e-bedc-4d1c44f0288e.png'>

MSE가 더 작은 이미지가 의미적으로 더 가까운 경우가 아닌 이미지들이 많기 때문에 현실적으로 올바른 확률값을 구하기가 어렵다.

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/209954687-9fdb29e5-f393-46c2-bb70-eaa6b13cd896.png">

예를들어 위 사진을 살펴보자. (a)는 원본, (b)는 픽셀의 일부를 자른경우, (c)는 오른쪽으로 1픽셀씩 Shift한 사진이다.  

가우시안(MSE) 관점에서 이를 관찰하게 되면 (a)는 (c)보다 (b)와 더 가깝다. 하지만 의미의 관점에서는 (a)와 (c)가 더 가까워야 한다.  

이런 문제를 해결하기 위해 가우시안 분포를 가정한 MLE 방법이 아닌 x와 유의미하게 유사한 샘플이 나올 수 있는 이상적인 확률 분포 $P(z\mid x)$로 부터 샘플링을 한다.  

그러나 $P(z\mid x)$가 무엇인지 알지 못하므로, 우리가 알고 있는 확률분포 중 하나를 택한다. $q_{\phi }\left(z\mid x\right)$   

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/209956035-0f3b2ff2-f723-462d-a23d-7ec71385ea40.png">

파라미터값을 조정하여 $P(z\mid x)$와 유사하게 만들어본다.(Variational Inference)  

1. X를 보여줄 테니 적어도 X는 잘 Generate 되게하는 Z를 만들어보자.
2. 그 Z를 만드는 이상적인 샘플링 함수 $P(z\mid x)$는 뭘까?
3. $P(z\mid x)$는 Variational Inference으로 찾아보자.
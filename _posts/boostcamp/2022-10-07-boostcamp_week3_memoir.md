---

layout: single

  

title: 부스트캠프 AI 3주차(Day-15 ~ Day-20) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록]

  

toc: true

  

---
## Day 15
+ Dive Into Deep Learning 17장을 팀원들과 리뷰했다.
  + 17.1 Overview of Recommender Systems
  + 17.2 The MovieLens Dataset
  + 17.3 Matrix Factorization
  + 17.4 AutoRec: Rating Prediction with Autoencoders
  + 17.5 Personalized Ranking for Recommender Systems
  + 17.6 Neural Collaborative Filtering for Personalized Ranking
  
피어세션: 17.5, 17.6 부분이 너무 어려워서 나중에 다시 살펴보기로 했다.
## Day 16
+ 딥러닝이 등작한 역사와 배경에 대해 간단히 살펴보았다.
  + Alexnet
  + DQN
  + Encode/Decoder, Adam
  + GAN, ResNet
  + Transformer
  + Bert
  + GPT-X
  + Self-Supervised Learning
+ 인공지능이란: 사람의 지능을 모방하는것.
+ Machine Learning과 Deep Learning의 차이가 뭘까?
  + Machine Learning: 무언가 학습하고자 할 때 학습을 데이터를 통해 알고리즘을 만드는 방법.
    + Deep Learning: Machine Learning의 세부 분야 중 하나로 뉴럴네트워크를 활용하는 분야.
+ Deep Learning의 Key Components 4가지
  + Data
  + Model
  + Loss
  + Algorithm

<br>
<br>

+ Neural Networks 에 대해 학습했다.
  + 비행기는 처음 새나 박쥐를 모방하며 시작해 지금의 동물과는 동떨어진 형태가 되었다.
  + 이와 비슷하게 Neural Networks도 인간의 뇌를 모방하며 시작했지만 지금은 살짝 동떨어진 형태가 되지 않았나 싶다.
  + Function approximators의 일종이다.
+ Multi-Layer Perceptron 을 공부했다.
  + Linear Neural Network

    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194794486-8ebf2c07-e752-493f-9ea1-ca3edb6d36ed.png">

  + Mult Layer Perceptron
  
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194794581-ab453716-ad93-4c5e-bb42-f88d924a5b60.png">

+ Logit 함수가 무엇인지 알아봤다.

<br>
<br>

+ Optimization
  + Generalization
    + 모델이 학습된 데이터 이외의 일반적인 unseendata에 대해 얼마나 잘 동작하는지를 말한다.
  + Under-fitting vs. over-fitting
    + Under-fitting 되면 성능이 떨어진다.
    + Over-fitting 되면 Generalization이 떨어진다.
  + Cross validation
    + K-fold validation 이라고도 한다.
    + Train data를 k개로 나누어 k개의 데이터 끼리 교차로 Vaildation data로 활용한다.
    + 하이퍼 파라미터의 최적을 Cross validation으로 찾고, 하이퍼 파라미터를 가지고 모든 train data를 학습시킨다.
  + Bias-variance tradeoff
    + Bias는 표적으로 부터 전체적으로 얼마나 떨어져 있는지 거리를 말한다.
    + Variance는 데이터들 끼리 얼마나 뭉쳐 있는지 여부를 말한다.
    + Bias와 Variance 간에는 Tradeoff가 존재한다.

      <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194795614-baff2f17-0619-47e0-86d1-1d469ca16264.png">

  + Bootstrapping
    + 학습 데이터 중 Subsmapling으로 여러개의 셋을 만들어 학습하겠다.
  + Bagging and boosting
    + Bootstrapping aggregating
      + 여러개의 모델을 Bootstrapping으로 학습하고 평균을 내겠다.
      + Parallel
    + Bagging
      + Sequential

      <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194796556-929c7c47-2bf1-4f43-a204-6b77bf4c0639.png">

+ Gradient Descent Methods
  + Stochastic gradient descent
  + Mini-batch gradient descent
  + Batch gradient descent
    + Lager batch -> Sharp Minimizers
    + Small batch -> Flat Minimizers

      <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194796761-4ece8f35-5f71-4dd3-b19b-dc64f44ffa03.png">

+ Momentum
  $$ \begin{aligned}a_{t+1}\leftarrow \beta a_{t}+g_{t}\\
  W_{t+1}\leftarrow W_{t}-\eta a_{t+1}\end{aligned} $$
+ Nesterov Accelerated Gradient
  + Lookahead Gradinet 추가.
  $$ \begin{aligned}a_{t+1}\leftarrow \beta a_{t}+\nabla L\left( W_{t}-\eta \beta _{at}\right) \\
  W_{t+1}\leftarrow W_{t}-\eta a_{t+1}\end{aligned} $$
+ Adagrad
  + 많이 변해온건 조금만, 조금만 변해온건 많이 변화시킨다.
  + Gt가 너무 커지면 0에 가까워지고 학습이 잘 안된다.

  <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194797450-bbdf7e57-8ac2-48ee-81ac-23673dd42a88.png">

+ Adadelta
  + EMA: Exponetial Moving Average
  + Learning rate이 없어 조절하기가 힘들다.
  + 바꿀수 있는 부분이 많이 없음.

  <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194797495-91ae73b4-b34f-4d67-a159-3a1f560db425.png">

+ RMSprop
  + Stepsize가 추가됨.

  <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194797541-63dfba88-75c3-43c2-9dd2-4b91bb0868da.png">

+ Adam
  + 최근 가장 많이 쓰이는 Optimizer

  <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194797594-0d6ab8ad-4ad4-460a-93b7-24830199143c.png">

<br>

+ Regularization
  + Over fitting을 막기위해 학습을 방해, 규제한다.
  + Early stopping 
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798076-85988f12-dc6e-4424-9929-d11845ec969e.png">
  + Parameter norm penalty 
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798113-65dfbda8-47d6-4842-9987-2518b8180577.png">
  + Data augmentation 
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798147-54d3b75b-307a-486c-adcb-69d258a42562.png">
  + Noise robustness
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798186-dbded22c-46c7-442b-a012-9a92b18a7ea2.png">

  + Label smoothing
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798239-572e6bcc-fd05-41d6-a73c-6f8626e47dc5.png">
  + Dropout
  
    <img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/194798277-45262cb3-546c-4f3f-adf1-e0f56fb63ae7.png">

  + Batch normalization
          

피어세션: 강의에서 "여기는 logit이니까 activation 없이 나오죠"라고 언급됐었다. 의문이 생겨 Logit 함수에 대해 조사했고 팀원들과 공유했다.
[Logit](https://velog.io/@guide333/logit-%ED%99%95%EB%A5%A0-sigmoid-softmax)

<br>
<br>
<br>

## Day 17
+ CNN에 대해 학습했다.
+ Modern CNN에 대해 살펴봤다.
+ CNN이 CV분야에서 어떻게 활용하는지 공부했다.
+ CNN에서 1by1 커널과 3by3 커널의 의미를 학습했다.
+ 왜 2by2 또는 짝수 커널을 쓰지 않는지 찾아봤다.
+ RNN에 대해 학습했다.
+ LSTM의 탄생배경과 구조에 대해 학습했다.
## Day 18
+ Transformer에 대해서 학습했다.(All you need attetion)
+ Multi Head Attention에 대해 학습했다.
+ Generative Models에 대해 학습했다.
## Day 19
+ VIT에 대해 학습했다.
+ Matplotlib에 대해 살펴봤다.
---
layout: single

title: 부스트캠프에 AI 1주차(Day-1 ~ Day-5) 회고록  
categories:
  - Boostcamp

tag: [Boostcamp, 부스트캠프, 경사하강법, SGD, RNN, BPTT, MLE, CNN]

toc: true

---

## Day 1

+ 첫날인 만큼 타운홀 미팅(오리엔테이션)이 진행됐다.

+ 타운홀 미팅이 무슨 뜻일까 궁금해서 찾아봤는데 정치인이 지역구 주민들과 만나 의견을 경청하는 미팅이라고 한다.

+ 모더레이터, 피어 세션, 데일리 스크럼 등등 낯선 용어가 많았다. 운영진분들도 네이밍 하기 힘들거 같다.

+ 오리엔테이션인 만큼 부스트캠프의 전체적인 흐름에 대해 설명해주셨다.

+ level 1 8주 동안 함께할 동료들을 만났다. 든든하다.

+ 유사 역행렬인 무어 펜로즈 행렬에 관해서 학습했다. 선형회귀는 유사 역행렬을 푸는 것만으로도 풀이가 가능하다. 단 비선형 함수는 불가능하다.

+ 선형 함수를 위해서는 경사하강법 즉 Gradient Descent가 쓰인다.

+ 심화 과제에서 Gradient Descent를 학습하면서 sympy로 다항식을 세우고 미분하는 법에 대해 배웠다.

+ Gradient Descent를 직접 알고리즘으로 구현하면서 더 깊게 이해하게 되었다.   
<br>
<br>
<br>
<br>



## Day 2

+ 경사하강법에 매운맛을 수강했다. 

     $$\\  \left\|y - X \beta\right\|$$
+ 경사하강법에 매운맛을 수강했다. 

  + 위와 같은 손실 함수의 L2_norm은 제곱 합의 평균 Mean Square Error로 접근하는 게 미분할 때 더 쉽게 계산할 수 있다.(제곱근 안의 수가 0에 가까워지는 방향과 MSE 자체가 0에 가까워지는 방향은 같다.)

    $$\\  \nabla_\beta\left\|y - X \beta\right\| = \frac{\partial{MSE}}{\partial\beta} =\delta_\beta\{\frac1n \sum_{i=1}^n(y_i - \sum_{j=1}^dX_{ij}\beta_{j})^2\}$$

    $$ = -\frac{2}{n}X^{T}(y-X\beta^{(t)}) $$

  + 경사하강법은 미분가능하고 conves(볼록)한 함수에 대해선 적절한 학습률과 학습 횟수를 선택했을 때 이론적으로 항상 수렴이 보장되어 있다.
<br>
<br>
<br>
<br>

+ Stochastic Gradient Descent(확률적 경사 하강법)에 대해 학습했다. 데이터 중 일부만을 활용해 업데이트를 하는 방법이다. 
    + SGD는 데이터의 일부로 파라미터를 업데이트하므로 연산 자원을 좀 더 효율적으로 활용할 수 있다.
    + 데이터의 일부를 미니배치라 하는데 미니배치는 에폭마다 미니배치를 확률적으로 선택해 모델을 학습하므로 목적식 모양이 바뀌게 된다.
    + SGD는 볼록이 아닌 목적식에서도 사용이 가능하다.
    + 정확한 gradient를 구해서 움직이는 방법이 아니라 극솟값으로의 이동만이 이루어 지지 않고 왔다갔다 하는 경향이 있다.
    + SGD가 만능은 아니지만 실증적으로 경사하강법보다 더 낫다고 검증 되었다.

  + 하드웨어 입장에서도 SGD는 모든 데이터를 한 번에 메모리에 업로드하지 않고 데이터의 일부를 미니배치로 쪼개 메모리에 올려 계산하므로 원본 데이터가 크더라도 좀 더 out-of-memory로 부터 자유롭다.

<br>

+ 유용한 사이트를 발견했다. [데이터 사이언스 스쿨](https://datascienceschool.net/intro.html)

<br>
<br>
<br>
<br>

## Day 3

+ softmax 함수에 대해 학습했다.
+ 추론을 할 때는 one-hot 벡터를 분류를 학습할 때는 softmax를 사용하자.
<br>
<br>
<br>
<br>
+ 딥러닝 학습방법 이해하기를 수강했다. 
  + 비선형 모델인 neural network에 대해 학습했다. 


  $$\\  \vec{o}_{({n}\times{p})} ={\vec{x}_{({n}\times{d})}}\times{ \mathbb{W}_{({d}\times{p})}} + \vec{b}_{({n}\times{p})} $$
  + 위 식은 지금까지 다뤄왔던 선형모델이고 그려보면 다음과 같다.


  <img src="https://user-images.githubusercontent.com/94548914/191547199-77cd864b-4c9c-4374-a62b-c5f6d1d7af77.png">

    + d 개의 변수로 p 개의 선형모델을 만들어서 p개의 잠재변수를 설명하는 모델이라고 할 수 있다.
    + 화살표들의 개수가 총 (d x p)개 이고 이는 가중치 행렬의 성분수와 같다.
    + 기존까지 다뤄왔던 선형모델의 각각에 비선형 함수를 합성한다. 
  $$\\  \mathbb{H} = (\sigma(z_1), \sigma(z_1), ..., \sigma(z_n)) $$
  $$ \sigma(\vec{z}) = \sigma(\mathbb{W}\vec{x} + \vec{b}) $$
    + 위 식을 모델로 그려보면 다음과 같다.
<img src="https://user-images.githubusercontent.com/94548914/191549251-1f83886a-c1d0-4f34-b803-69e78c35d3e8.png">
    + 각각의 z<sub>1</sub>, z<sub>2</sub>, ..., z<sub>n</sub>에 각각 비선형 함수를 합성해야한다.
    + z 벡터에 비선형함수를 합성해 새로만든 잠재벡터를 Hidden Vector라고도 하고 H 벡터라한다.
    + 비선형 모델을 합성함으로써 선형모델만 다룰 수 있던 한계를 극복할 수 있다.
    + 네모 상자 안에 있는 벡터들을 퍼셉트론 혹은 뉴런이라 하고 뉴런으로 이루어진 모델을 neural network라 한다.
  <br>
  <br>
  <br>
  <br>
    + 히든 벡터 층이 2개 있으면 Two Layer Perceptron 여러 개 있으면 Multi Layer Percetropn이라 한다.
    + 이론적으로 2층 신경망으로도 임의의 연속함수를 근사할 수 있다.
    + 그러나 층이 깊을수록 뉴런의 숫자가 더 빨리 줄어들어 좀 더 효율적이다.
    + 층이 깊다고 해서 최적화가 쉬운 것은 아니다. 깊어질수록 최적화하기는 더 어려워진다.
    + 대표적인 활성함수는 Sigmoid, tanh, ReLu 등이 있다.
    + 딥러닝에선 ReLu 함수를 많이 쓴다.
<br>
<br>
<br>
<br>
+ RNN 강의를 먼저 들었다.
  + 시퀀스 데이터의 특징은 이벤트의 발생 순서가 아주 중요한 요소라는 점이다. 
    + ex) 개가 사람이 물었다. 사람이 개를 물었다.
  + 시퀀스 데이터는 조건부확률의 곱셈 형태로 나타내어 다룰 수 있다.
  $$ \\ \prod ^{t}_{5=1}P\left(  X_{s}| X_{s-1},..., X_{1}\right) $$
  + 시퀀스 데이터를 다루기 위해서 과거의 정보가 저장되며 출력에 전달되는 형태의 모델이 필요하다.
  <center>
    <br/>
    <img src = "https://wikidocs.net/images/page/22886/rnn_image2_ver3.PNG"/>
    <div style="color:gray"> 출처: https://wikidocs.net/book/2155</div>
  </center>
  
   +  RNN 모델의 역전파 즉 BPTT를 구하기 위해 수식을 전개해 봤다.(나중에 다시 한번 해봐야겠다.)

$$\\ \frac{\partial \xi}{\partial W_x} = (\frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_n} \frac{\partial S_n}{\partial W_x} + \frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_n} \frac{\partial S_n}{\partial S_{n-1}}\frac{\partial S_{n-1}}{\partial W_x} \cdots) = 
\sum\limits_{k=0}^n \frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_k} \frac{\partial S_k}{\partial W_x} = \frac{1}{n}\sum\limits_{k=0}^n \frac{\partial \xi}{\partial S_k} X_k $$ 

$$\frac{\partial \xi}{\partial W_{rec}} =(\frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_n} \frac{\partial S_n}{\partial W_{rec}} + \frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_n} \frac{\partial S_n}{\partial S_{n-1}}\frac{\partial S_{n-1}}{\partial W_{rec}} \cdots) = 
\sum\limits_{k=0}^n \frac{\partial \xi}{\partial y} \frac{\partial y}{\partial S_k} \frac{\partial S_k}{\partial W_{rec}} = \frac{1}{n}\sum\limits_{k=1}^n \frac{\partial \xi}{\partial S_k} S_{k-1} $$ 

<br>
<br>
<br>
<br>

## Day 4
+ 확률론
  + 딥러닝은 데이터 공간을 통계적으로 해석해 예측이 틀릴 Risk를 최소화하도록 데이터를 학습시킨다.(Loss Function의 작동원리) 그러므로 반드시 확률론이 필요하다.
    + 회귀분석은 L2_norm의 예측 오차의 분산을 가장 최소화하는 방향으로 학습한다.
    + 분류 문제는 모델 예측의 불확실성(cross-entropy)을 최소화하는 방향으로 학습한다.
    
  + 이산확률변수와 연속확률변수는 그 값을 다룰 때 급수와 적분이라는 차이가 존재한다.
  + 모델의 확률분포는 학습 사전에 알 수 없다. 그렇기에 주어진 데이터에서 결합 분포를 활용해 실증적으로 모델의 분포를 추정해 나간다. 이 과정 떄문에 원래의 확률분포에 관계없이 결합분포는 이산확률분포나 연속확률분포로 만들 수 있다.
  + 주어진 데이터에서 실증적으로 추정한 분포는 원래의 분포와 다를 수 있다. 하지만 원래의 분포에 근사할 수 있는 방법들이 존재한다.
  + 분류 문제는 데이터 x로부터 추출된 패턴𝜙(x)와 가중치 행렬 W을 통해 조건부 확률을 계산한다.(softmax(W𝜙(x) + b))
  + 회귀문제의 경우 밀도함수로 추정을 해야 하므로 조건부 기댓값을 추정한다.
$$ \\ \mathbb{E}\left[  y| x\right] = \int _{y}yP\left(  y| x\right) \cdot dy \left( 1\right) $$
  + 조건부 기댓값은 아래식을 최소화하는 함수와 일치하는 게 수학적으로 증명되었다. (나중에 더 찾아봐야겠다.)  
$$ \\ \mathbb{E}\left\| y-f\left( x\right) \right\| _{2} $$    
  + 통계적 모형에서 목적에 따라 추정량이 달라질 수 있다.
  + 평균, 분산, 첨도, 공분산에 대해 배웠다.
  + 몬테 카를로 샘플링에 알아봤다.

<br>
<br>
<br>
<br>

+ 통계학
  + 모수의 정의와 모수를 추정하는 법을 배웠다..
    + 모수는 모집단의 특성을 나타내는 값을 말한다.
    + 유한한 데이터로 모집단을 정확히 아는 것은 불가능하므로 근사적으로 확룰분포를 추정한다.
    + 통계적 모델링을 통해 적절한 가정위에서 확률분포를 추정한다.
  + 모수적 방법론
    + 모수를 가정하고 그 분포를 결정하는 방법이다.
    + N - 1로 나누는 이유는 unbiased(불편) 추정량을 구하기 위해서이다.
$$ \\ \overline{X}=\dfrac{1}{N}\sum ^{N}_{i=1}x_{i} $$
$$S^{2}=\dfrac{1}{N-1}\sum ^{N}_{i=1}\left( x_{i}-\overline{x}\right) ^{2}$$

  + 비 모수적 방법론
    + 데이터에 따라 모델의 구조 및 모수의 개수를 수정하는 방법이다.
    + 기계적으로 확률분포를 가정하지 않고 데이터를 생성하는 원리를 먼저 고려해야 한다.
    + 모수를 추정한 후엔 반드시 검정을 해야 한다.
    + Maximum Likelihood Estimations (MLE) 최대 가능도 추정법이 있다.
$$ \\ \widehat{\theta }_{MLE}=argmax L\left( \theta ;x\right) =argmaxP\left( \left| x\right| \theta \right)  $$

+ Maximum Likelihood Estimations (MLE)
  + 이론적으로 가장 가능성이 높은 모수를 추정하는 방법 중 하나이다.
  + 가능도 함수는 θ를 따르는 분포가 X를 관찰할 가능성을 뜻하지만 확률로 해석하면 안 된다.
  + 데이터 집합 X가 독립적으로 추출되었을 경우  로그 가능도를 최적화하는 게 유리히다.
    + 곱을 덧셈으로 바꿀 수 있어 연산이 간단해진
    + 미분연산의 시간 복잡도가 O(n^2)에서 O(n)으로 줄어든다.
$$ \\ L\left( \theta ;\mathbb{X}\right) =\prod ^{n}_{i=1}P\left(  x_{i}| \theta \right)  $$
$$  ⇒ \log L\left( \theta ;\mathbb{X}\right) =\log \prod ^{n}_{i=1}P\left(  x_{i}| \theta \right)  $$

<br>

+ 마스터 클래스를 진행했다. 현업에 계신 교수님의 대부분이 궁금해할 질문에 대해 답변해 주셨다.


<br>
<br>
<br>
<br>

## Day 5
+ 베이즈 통계학
  + 데이터가 새롭게 추가될 때 정보를 어떻게 업데이트할 것인가?
  + 베이즈 정리를 활용해 가능하다.
$$\\ P\left(  B| A\right) =\dfrac{P\left( B\cap A\right) }{P\left( A\right) } = P\left( B\right) \cdot \dfrac{P\left(  A| B\right) }{P\left( A\right) } $$
  + A라는 새로운 정보가 주어졌을 때 P(B)로부터 P(B\|A)를 계산할 수 있다.
$$\\ P\left(  B| A\right)_{사후확률} = P\left( B\right)_{사전확률}  \cdot \dfrac{P\left(  A| B\right)_{가능도}  }{P\left( A\right)_{Evidence}  } $$
  + 베이즈 정리를 통해 계산한 사후 확률을 다시 사전 확률로 사용하여 갱신된 사후 확률을 계산할 수 있다.
  + 조건부 확률로 인과관계를 함부로 추론해서는 안 되고, 인과관계를 알아내기 위해서는 중첩 요인(Confounding factor)의 효과를 제거해야 한다.
<br>
<br>
<br>
<br>

+ Convolution Neural Network (CNN)
  + Convolution 연산은 커널을 입력 벡터 상에서 움직여가면서 선형모델과 합성되는 구조이다.
    $$ \\ \left[ f\ast g\right]\left( x\right) =\int _{\mathbb{R} ^{d}}f\left( z\right) g\left( x-z\right) \cdot dz = \int _{\mathbb{R} ^{d}}g\left( x-z\right) f\left( z\right) \cdot dz = \left[ g\ast f\right]\left( x\right)$$

    $$ \left[ f\ast g\right]\left( x\right) =\sum _{a\in \mathbb{Z} ^{d}}f\left( z\right) g\left( x-z\right) \cdot dz = \sum _{a\in \mathbb{Z} ^{d}}g\left( x-z\right) f\left( z\right) \cdot dz = \left[ g\ast f\right]\left( x\right)$$
    + 커널은 정의역 내에서 움직여도 변하지 않는다. (Translation invariant)
    + 커널은 주어진 신호에 국소적으로 적용된다.
    + [커널 실습 해보기](https://setosa.io/ev/image-kernels/)
  + 입력 크기를 (H, W), 커널 크기를 (K<sub>H</sub>, K<sub>W</sub>), 출력 크기를 (O<sub>H</sub>, O<sub>W</sub>)라 하면 출력 크기는 다음과 같이 계산합니다.

      $$ \\ O_{H}=H-K_{H}+1 \\ $$
      $$ O_{W}=W-K_{W}+1 $$

  + 채널이 여러 개면 convolutuion을 채널 개수 만큼 적용한다.
    + 출력이 한 개면 커널의 채널수와 입력의 채널수가 같아야 한다.
    + 출력을 여러 개 받고 싶다면 채널의 수를 늘리면 된다.
  + CONV 연산의 역전파
    + CONV는 역전파를 계산할 때도 CONV 연산이 나오게 된다.
  $$ \\ \dfrac{\partial }{\partial x}\left[ f\ast g\right]\left( x\right) =\dfrac{\partial }{\partial x}\int _{\mathbb{R} ^{d}}f\left( z\right) \dfrac{\partial g}{\partial x}\left( x-z\right) \cdot dz = \int _{\mathbb{R} ^{d}}g\left( x-z\right) f\left( z\right) \cdot dz = \left[ f\ast g'\right]\left( x\right)$$
    + 그림을 통해 자세히 살펴보자.

      <center>
        <img src="https://user-images.githubusercontent.com/94548914/191957662-b99446b1-6865-4da8-ac4a-a5aef85f70a5.png" >

      CNN 모델에서 역전파를 고려해보자.
      <center>
        <img src="https://user-images.githubusercontent.com/94548914/191958225-de314746-cdd0-4936-a081-3235f9b010f3.png">
      </center>


      O<sub>1</sub>은 x<sub>3</sub>에  W<sub>3</sub>를 통해 Grad를 전달한다.   

      O<sub>2</sub>은 x<sub>3</sub>에  W<sub>2</sub>를 통해 Grad를 전달한다.   

      O<sub>3</sub>은 x<sub>3</sub>에  W<sub>1</sub>를 통해 Grad를 전달한다.


      <center>
        <img src="https://user-images.githubusercontent.com/94548914/191958469-5601a9cb-061f-4fbd-8331-1e0d3fa854fa.png">
      </center>

      O<sub>3</sub> 가 x<sub>3</sub>에 대해서 w<sub>1</sub>을 통해 Grad을 전달 했기 떄문에 w<sub>1</sub>을 통해서 전달 되었던 Grad였던 δ<sub>3</sub>가  w<sub>1</sub>에 배정된다.<br> 
      O<sub>2</sub> 가 x<sub>3</sub>에 대해서 w<sub>1</sub>을 통해 Grad을 전달 했기 떄문에 w<sub>1</sub>을 통해서 전달 되었던 Grad였던 δ<sub>2</sub>가  w<sub>2</sub>에 배정된다.<br> 
      O<sub>1</sub> 가 x<sub>3</sub>에 대해서 w<sub>1</sub>을 통해 Grad을 전달 했기 떄문에 w<sub>1</sub>을 통해서 전달 되었던 Grad였던 δ<sub>1</sub>가  w<sub>3</sub>에 배정된다.<br> 

      <center>
        <img src="https://user-images.githubusercontent.com/94548914/191958639-70bd6d78-5c2b-4846-aa4c-9ab7e7fd2742.png">
      </center>

      이 과정을 모든 입력 x에 대해 반복하면 결국 convolution 연산의 꼴이 된다.
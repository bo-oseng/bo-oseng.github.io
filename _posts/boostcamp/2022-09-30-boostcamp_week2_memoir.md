---

layout: single

  

title: 부스트캠프에 AI 2주차(Day-7 ~ Day-12) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록]

  

toc: true

  

---

  

## Day 7

+ Pytorch Intro 강의를 수강했다. 많은 딥러닝 라이브러리가 있지만 대표적으로 PyTorch와 TensorFlow가 있다.
	+ Pytorch의 방식은 즉시 확인이 가능하고 사용하기 편하다는게 큰 장점이다.
	+ TensorFlow의 장점은 Production과 Scalability에 장점이 있다.
	+ PyTorch는 Numpy, Auto Grad, Function 세가지의 조합이 특징이다.
	+ Numpy 구조를 가지는 Tensor 객체로 array를 표현한다.
	+ Auto grad(자동미분)을 지원하다.
	+ 다양한 형태의 함수와 모델을 지원한다.
	+  Pytorch와 TensorFlow의 큰차이는 Pytorch는 Define by Run의 방식이고 TensorFlow는 Define and Run의 방식이다.
		+ 이 개념을 이해하기 위해서는 먼저 [Computational Graph](https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/)에 대해 알아야한다.
		+ 쉽게 말하자면 함수하나 하나를 Node 처럼 취급하는 Graph이다. 
		+ 각 Node에는 알맞은 함수와 역전파시 해당 노드 입장에서 Input Grad와 Outpu Grad를 체인룰을 이용해 함수형태로 저장하고 있다. 
		+ 이런 형태로 모델이 이루어져 있기에 Autograd가 가능하다.
		+ Pytorch는 에서는 Runtime에 Computational Graph가 Define되며 출력이 가능하지만 TensorFlow는 Graph를 Define하고 Runtime에 데이터를 feed시켜준다.


			 <img src="https://scontent-ssn1-1.xx.fbcdn.net/v/t1.6435-9/39012951_549971678790670_7494296685422575616_n.png?_nc_cat=109&ccb=1-7&_nc_sid=8024bb&_nc_ohc=0LSxRo2Nw8kAX8-E6g_&_nc_ht=scontent-ssn1-1.xx&oh=00_AT_A-GeKoYbp9K0QawwLgbGyJUaA0Wg-LBAOwRy_NB1kSA&oe=635D2333" width="50%">

<br>

<br>

<br>

+ Pytorch basic 강의를 수강했다. tensor의 선언과 사칙연산 등을 배웠다
	+ tensor의 선언과 기본적인 연산은 numpy와 거의 비슷하다.
	```python
	import numpy as np
	import torch
	n_array = np.arange(10).reshape(2,5)
	t_array = torch.FloatTensor(n_array)
	print("ndim :", n_array.ndim, "shape :", n_array.shape)
	print("ndim :", t_array.ndim, "shape :", t_array.shape)
	```
	```bash
	ndim : 2 shape : (2, 5)
	ndim : 2 shape : torch.Size([2, 5])
	```
	+ tensor는 GPU에 올려 연산이 가능하다.
		+ ```t_array.data.to('cuda')```
	
	+ View와 Reshpae
		+ Contiguity 보장에 차이가있다. Contiguity란 메모리의 연속성과 데이터의 순서가 일치하는지의 여부이다.
		+ 예시를 살펴 보자
  
		```pyton
		a = torch.arange(6).view(3, 2)
		b = a.view(6)
		print(f'a: {a}')
		print(f'b: {b}')
		```

		```bash
		-> a: tensor([[0, 1], [2, 3], [4, 5]]) 
		-> b: tensor([0, 1, 2, 3, 4, 5])
		```

		```python
		a.fill_(1)
		print(f'a: {a}')
		print(f'b: {b}')
		```

		```bash
		-> a: tensor([[1, 1], [1, 1], [1, 1]])
		-> b: tensor([1, 1, 1, 1, 1, 1])
		```
		```pyton
		a = torch.arange(6).view(3, 2)
		b = a.t().reshape(6)
		print(f'a: {a}')
		print(f'b: {b}')
		```
		```bash
		-> a: tensor([[0, 1], [2, 3], [4, 5]])
		->  b: tensor([0, 2, 4, 1, 3, 5])
		```
	
		```python
		a.fill_(1)
		print(f'a: {a}')
		print(f'b: {b}')
		```

		```bash
		-> a: tensor([[1, 1], [1, 1], [1, 1]])
		# 연속성이 보장되지 않자 referernce를 끊고 copy형태로 저장된다.
		->  b: tensor([0, 2, 4, 1, 3, 5])
		```

	
	+ dim별 squeeze와 unsquzee를 학습했다.
		<img src="https://user-images.githubusercontent.com/94548914/193257456-ee454200-f900-48b1-9908-75ea72612a4f.png" width="80%">   

		<a href = https://bit.ly/3CgkVWK style='color: gray'>출처: https://bit.ly/3CgkVWK</a>
	
	+ mm과 matmal의 차이를 학습했다. (matmal은 brodcasting을 지원하고, mm은 지원하지 않는다.)
	
	+ 자동 미분의 핵심인 backward 배웠다.(requires_grad 키워드를 학습했다.)
		+ 자동 미분은 1일차에 학습했던  [Computational Graph](https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/)를 통해 이루어지고 해당 Parameter의 미분값을 저장할지 말지 정하는 옵션이 바로 requires_grad 키워드 이다.

<br>

<br>

<br>

+ 과제로 주어진 custom model제작 문서를 살펴봤다.
	+ PyTorch의 [Documentation](https://pytorch.org/docs/stable/index.html)을 활용하는 법에 대해 배웠다.
	+ torch와 tensor에 있는 기본적인 메소드들과 객체들을 살펴봤다.
		+ torch.view
		+ torch.gather
		+ torch.zeros
		+ torch.ones_like
		+ torch.arage
		+ torch.cat
		+ torch.chunk


<br>

<br>

  

## Day 8

+ custom model제작을 이어서 풀이했다.
	+ torch.nn
		+ nn.Module
		+ nn.Sequential
		+ nn.ModuleList
			+ module 클래스는 하위 포함된 모든 submodules들을 module단위로 출력해준다.
			+ 모듈리스트가 아닌 리스트로 만들어진 python_list는 리스트 내부가 모듈임을 인지하지 못한다.
			+ 모델 구조의 명확성을 위해 ModuleList를 써야한다.
		+ nn.ModuleDict
		+ nn.Parameter
			+ 모델 학습에 주로 사용되는 역전파 방법을 활용하기 위해서는 단계별로tensor의 gradient를 저장하고 있어야 한다.
			+ 이런 관점에서 따로 변수에 저장하기보다 Parameter 클래스로 생성한 인스턴스 안에 제네레이션(함수) 형태로 이를 저장하고 있는것이 매우 효율적인 방법이다.
		+ nn.Buffer
		+ named_children
		+ named_modules
		+ named_parameters
		+ named_buffers
		+ get_submodule
		+ get_parameter
		+ get_buffer
	+ hook
		+ 패키지화된 코드에서 다른 프로그래머가 custom 코드를 중간에 실행시킬 수 있도록 만들어놓은 인터페이스이다.
		+ forward hook은 module에만 적용 가능하다.
		+ backward hook은 tensor와 module 모두 가능하다.
		+ register_forward_hook( hook(module, input, output) -> None or modified output )
			+ return ouptput으로 output만 수정가능하다.
		+ register_forward_pre_hook( hook(module, input) -> None or modified input )
			+ return inpur으로 input만 수정가능하다.
		+ register_full_backward_hook( hook(module, grad_input, grad_output) -> tuple(Tensor) or None )
			+ input, output을 수정하면 에러가 일어난다.
		+ register_backward_hook(hook_fn)
			+ tenosr만 가능한 hook
	+ apply
		+ 모듈의 Computational Graph의 루트부터 리프까지 모든 함수에 일일히 적용하는 함수.
		+ model.apply( func )   
<br>

		[hook 참고자료](https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/)

<br>

<br>

<br>

  

## Day 9

+ Auto grad

+ Optimizer

+ Datasets

+ DataLoader

+ 기본과제 2

<br>

<br>

<br>

  

## Day 10

+ Model 불러오기, pretrainde

+ Monitoring tools

+ 오피스아워

+ 기본과제2

<br>

<br>

<br>

  

## Day 11

+ Pytorch template 이해하기

+ multi GPU

+ Hyperparmeter Tuning

+ Pytorch troubleshooting

+ 마스터클래스
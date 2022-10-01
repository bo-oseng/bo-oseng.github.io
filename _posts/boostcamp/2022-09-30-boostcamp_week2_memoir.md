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
	<br>

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

	<br>
	
	+ dim별 squeeze와 unsquzee를 학습했다.
		<img src="https://user-images.githubusercontent.com/94548914/193257456-ee454200-f900-48b1-9908-75ea72612a4f.png" width="80%"/>   

		<a href = 'https://bit.ly/3CgkVWK' style='color: gray'>
		  출처: https://bit.ly/3CgkVWK 
		</a>
	
	+ mm과 matmal의 차이를 학습했다. (matmal은 brodcasting을 지원하고, mm은 지원하지 않는다.)
	
	+ 자동 미분의 핵심인 backward 배웠다.(requires_grad 키워드를 학습했다.)
		+ 자동 미분은 1일차에 학습했던  [Computational Graph](https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/)를 통해 이루어지고 해당 Parameter의 미분값을 저장할지 말지 정하는 옵션이 바로 requires_grad 키워드 이다.

<br>

<br>

<br>

+ 과제로 주어진 Documentation 활용과 nn.Module에 대해 살펴봤다.
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
+ 모델의 미분을 담당하는 Backwad 함수는 Auto grad가 아닌 사용자가 직접 미분 수식을 오버라이딩이 가능하다.
  
+ 모델을 훈련시키는 Optimize 또한 오버라이딩 가능하다.
<br>
<br>
+ 전체적인 Trainning의 흐름
<img alt="모델 훈련 흐름" src="https://user-images.githubusercontent.com/94548914/193284626-a8213eda-9890-4daa-bc96-8b2b9a4f4c50.png">

  
+ Datasets
  + 데이터를 시작할 때 어떻게 불러 온 것인지(\_\_init\_\_())
  + 데이터의 총 길이가 얼마나 되는지(\_\_len\_\_())
  + 하나의 데이터를 불러올 때 어떤식으로 매핑 할 것인지(\_\_getitem\_\_())
  + Image, Text, Audio 등 데이터 입력 형태에 따라 각 함수를 다르게 정의한다.
  + 데이터 셋에 대한 표준화된 처리 방법 제공이 필요하다.
  + 모든 것을 데이터 생성 시점에 처리할 필요는 없음.

+ Transforms
  + 데이터를 다루기 쉽게 Preprocessor(전처리)
    + ToTensor(), CenterCrop() 등등

+ DataLoader
  + Data들을 묶어서 Sampling과 Shuffle을 하거나 Batch를 생성해 주는 클래스.
  + Tensor로 변환 + Batch 처리가 메인.
  + 학습 직전(GPU feed)전 데이터 변환을 책임짐.
  + 병렬적인 데이터 전처리 코드의 고민이 필요함.
  + [DataLoader Parameters](https://subinium.github.io/pytorch-dataloader/)
    + sampler
    + batch_sampler
    + num_workers
    + collate_fn

+ Custom Dataser과 Custom DataLoader 생성
  + MyMNISTDataset을 만들어보자.
    + 숫자 이미지 데이터를 받아 0 ~ 9까지의 클래스로 분류하는 모델
	```python
	# MyMNISTDataset 생성
	class MyMNISTDataset(Dataset):
		_repr_indent = 4

		def __init__(self, path, transform, train=True):
			self.path = path
			self.transform = transform
			self.images = read_MNIST_images(self.path['image'])
			self.lables = read_MNIST_labels(self.path['label'])
			self.classes = [
				"0 - zero",
				"1 - one",
				"2 - two",
				"3 - three",
				"4 - four",
				"5 - five",
				"6 - six",
				"7 - seven",
				"8 - eight",
				"9 - nine",
				]

		def __len__(self):
			len_dataset = None
			len_dataset = len(self.lables)
			return len_dataset

		def __getitem__(self, idx):
			X,y = None, None
			# transform 적용
			X = self.transform(self.images[idx])
			
			# path를 '/'로 쪼갠뒤, 4번째 인덱스의 문자열의 5글자가 train이 아닌 경우는 학습데이터가 아니다.
			if 'train' != self.path['image'].split('/')[4][:5]:
				return torch.tensor(X, dtype=torch.double)
			y = self.lables[idx]
			return torch.tensor(X, dtype=torch.double), torch.tensor(y, dtype=torch.long)

		def __repr__(self):
			'''
			https://github.com/pytorch/vision/blob/master/torchvision/datasets/vision.py
			'''
			head = "(PyTorch HomeWork) My Custom Dataset : MNIST"
			data_path = self._repr_indent*" " + "Data path: {}".format(self.path['image'])
			label_path = self._repr_indent*" " + "Label path: {}".format(self.path['label'])
			num_data = self._repr_indent*" " + "Number of datapoints: {}".format(self.__len__())
			num_classes = self._repr_indent*" " + "Number of classes: {}".format(len(self.classes))

			return '\n'.join([head,
							data_path, label_path, 
							num_data, num_classes])
	```
	```python
	# dataloader_train_MNIST 생성
	dataloader_train_MNIST = DataLoader(dataset=dataset_train_MyMNIST,
										batch_size=16,
										shuffle=True,
										num_workers=4,
										)
	```

<br>

<br>

<br>

  

## Day 10

+ Model 저장과 불러오기
  + 학습 결과를 공유하고 싶다.
  + model.save()
    + 학습의 결과를 저장하기 위한 함수
    + 모델 학습 중간 과정의 저장을 통해 최선의 결과 모댈울 선택
    + 만들어진 모델을 외부 연구자와 공유하여 학습 재연성을 향상 시킴
  
	```python
	# 파라미터만 저장 및 로드
	torch.save(model.state_dict(), 'model.pt')

	new_model = TheModelClass()
	new_model.load_state_dict(torch.load('model.pt'))
	```
	```python
	# 모델의 전체 architecture와 함께 저장 및 로드
	torch.save(model, 'model.pt')

	new_model = TheModelClass()
	model = torch.load('model.pt')
	```
+ Checkpoints
  + 학습의 중간 결과를 저장하여 최선의 결과를 선택하는데 활용
  + earlystopping 기법 사용시 이전 학습의 결과물을 저장
  + loss와 metric 값을 지속적으로 확인 저장
  + 일반적으로 epoch, loss, metric 을 함께 저장하여 확인
  + Dict 형태로 저장 및 로드를 함.
  + 특정한 조건때에만 저장되게 설정하는게 좋음.
	```python
			
		torch.save({
			'epoch': e,
			'model_state_dict': model.state_dict(),
			'optimizer_state_dict': optimizer.state_dict(),
			'loss': epoch_loss,
			}, "checkpoint.pt")
			
	```

+ Transfer Learning, Pretrained Model
  + 다른 데이터셋으로 만든 모델을 현재 데이터에 적용
  + Data Centric AI에 유리하다.
  + 일반적으로 대용량 데이터셋으로 만들어진 모델의 성능이 뛰어나다.
  + 현재의 Dl에서 가장 일반적이 학습 기법
  + Backbone Architecture가 잘 학습된 모델에서 일부분만 변경하여 학습을 수행함.
  + BERT, ImageNet, GPT 등등
  + Pretrained model을 활용시 모델의 일부분을 frozen 시킴
    + 내 모델과 합칠때 적절하게 특정 위치까지만 학습시키고 원래 모델의 Parameter를 frozen시킴
    + Stepping frozen 기법.
    + 입력과 출력을 내 데이터에 맞게 조절.
    + Parameter Frozen 반드시 확인.
	```python
	# pretrained=True로 참고할 모델 vgg16을 불러옴
	my_model = models.vgg16(pretrained=True).to(device)
	```
	```python
	# 원래 모델의 parameter 들을 equires_grad = False 시킴
	for param in my_model.parameters():
		param.requires_grad = False
	# 내가 필요한 부분의 parameter 들을 requires_grad = True 시킴
	for param in my_model.linear_layers.parameters():
		param.requires_grad = True
	```

+ model.eval()
  + 모듈을 평가 모드로 바꾼다.
  + Dropout이나 BatchNorm 같이 Traim time에서만 사용하는 동작들을 멈추게 해준다.
  + evaluation이 종료되면 model.train()으로 모드를 변경해 주어야 한다.

+ binary_acc 함수
	```python
	# y_pred_tag과 y_pred_tag가 일치하는  갯수를 세고
	# 데이터의 크기로 나눈값의 백분율을 구한다.
	def binary_acc(y_pred, y_test):
		y_pred_tag = torch.round(torch.sigmoid(y_pred))
		correct_results_sum = (y_pred_tag == y_pred_tag).sum().float()
		acc = correct_results_sum/y_test.shape[0]
		acc = torch.round(acc * 100)
		
		return acc
	```
	
+ nn.BCEWithLogitsLoss
  + Binary Cross Entropy With Logits Loss 
  + BCE Loss에 Sigmoid Layer를 추가한다.
  + ℓ(x,y)=L={l 
1
​
 ,…,l 
N
​
 } 
⊤
 ,l 
n
​
 =−w 
n
​
 [y 
n
​
 ⋅logx 
n
​
 +(1−y 
n
​
 )⋅log(1−x 
n
​
 )],
+ Monitoring tools
  + 분석을 도와주는 유용한 도구들이 있다.
  + TensorBoard
  + WadnB
  
	<img width="80%" alt="image" src="https://user-images.githubusercontent.com/94548914/193386432-bfc0a005-140a-48df-965d-c8754283be1f.png">


<br>

<br>

<br>

  

## Day 11


+ multi GPU

+ Hyperparmeter Tuning

+ Pytorch troubleshooting

+ Pytorch template 이해하기

+ 마스터클래스
---
layout: single

  

title: 부스트캠프 AI 5주차(Day-33) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, MAB, Multi-Armed Bandit, Epsilon-Greedy Algorithm, Greedy Algorithm,UCB, Upper Confidence Bound, Thompson Sampling, LinUCB]

  

toc: true
---

## Day 33

### Multi-Armed Bandit (MAB)
+ 배경
  + One-Armed Bandit (Slot Machine이라고 생각하자)

    + 카지노에 있는 슬롯머신은 한 번에 단 한 개의 arm을 당길 수 있음
    
  + Multi-Armed Bandit
    
    + 카지노에 있는 K개의 슬롯머신을 N번 플레이 할 수 있다면?
    
    + 단 K개의 슬롯머신에서 얻을 수 있는 reward의 확률이 모두 다르다고 가정
    
    + K개의 슬롯머신을 N번 플레이 할 수 있을 때 어떻게하면 가장 많은 reward를 얻을 수 있을까? 어떤 슬롯머신이 가장 많은 reward를 가질지 추정 해 나가는 문제
    
    + 수익을 최대화하기 위해서는 arm을 어떤 순서대로 혹은 어떤 Policy(정책, 방법)에 의해 당겨야 하는가?
    
    + Challenge in MAB
    
      + 사전에 슬롯머신의 reward 확률을 정확히 알 수 없다.
    
    + Exploration & Exploitation Trade-off
    
      + Exploration(탐색): 더 많은 정보를 얻기 위하여 새로운 arm을 선택하는 것
    
      + Exploitation(활용): 기존의 경험 혹은 관측 값을 토대로 가장 좋은 arm을 선택하는 것
    
      + 모든 슬롯머신을 동일한 횟수로 당긴다면 높은 reward를 기대할 수 없다. 그러므로 Exploration의 성질이 필요하다.
    
      + 일정 횟수만큼 슬롯머신을 당겨보고, 남은 횟수는 그 시간 동안 제일 높은 확률을 가진 슬롯머신만 당기게 된다. Exploitation의 성질로 가능하다.
    
      + 둘 사이에는 Trade-off가 항상 존재한다
        + 탐색에 비용을 지나치게 낭비하여 높은 reward를 얻을 수 없음
        + 잘못된 슬롯머신을 활용하게 되면 높은 reward를 보장할 수 없음
  
### MAB Formula
  
$$ q_{*}(a) = E[R_{t}|A_{t}=a] $$

  + t: time step or play number
  
    + 첫번쨰 두번째 세번째 레버를 당기는 시간
  
  + A<sub>t</sub>: 시간 +에 play한 action
  
    + t 스텝에 내가 선택한 슬롯머신
  
  + R<sub>t</sub>: 시간 +에 받은 reward
  
    + 액션이 a일 때 받는 reward
  
  + q<sub>*</sub>(a):액션#에따른reward의실제기대값

  + 모든 action에 대한 실제 q<sub>*</sub>(a) 즉 reward의 true distribution을 모르기 때문에 추정을 한다.

  + q<sub>*</sub>(a)에 대한 시간 t에서의추정치 Q<sub>t</sub>(a)를 최대한 정밀하게 구하는 것이 목표!
  

### Simple Average Method (Greedy Algorithm)

+ 추정 가치가 최대인 action을 선택하는 것을 greedy action이라고 함
+ 매번 time step마다  추정치를 계산하고 추정가치가 최대인 action을 선택하는 방법
+ greedy action 선택 → exploitation
+ 다른 action 선택 → exploration
+ 실제 기대값 q<sub>*</sub>(a) 의 가장 간단한 추정 방식으로 표본평균을 사용한다.
+ 가장 간단한 policy로서문제는 policy가 처음에 선택되는 action과 reward에 크게 영향을 받음. exploration이 부족한 문제가 있다.

<center>
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/197189736-6101ae30-bc88-4441-955b-19b5fcdaa6b0.png">
</center>

$$A_{t}=argmax_{a} Q_{t}\left( a\right) $$

### Epsilon-Greedy Algorithm
+ 배경
  + Exploration이 부족한 greedy algorithm의 policy를 수정한 전략
+ 일정한 확률에 의해 랜덤으로 슬롯머신을 선택하도록 함.(ex) epsilon = 0.5)
+ 장점
  + epsilon-greedy는 심플하면서도 강력한 알고리즘.
  + 가장 간단하면서도 안정적인 성능을 보여 성능 비교를 위한 Baseline으로 많이쓰임.
+ 단점
  +  다만 시간이 많이 지나 충분히 데이터가 쌓여서 각각의 액션의 true distributuon을 충분히 추정했음에도 항상 Epsilon의 확률로 랜덤액 액션을 선택(Exploration & Exploitation Trade-offf)이 있으므로 후반으로 가면서 손해를 보는구조임

  <img width="60%" alt="image" src="https://user-images.githubusercontent.com/94548914/197329973-80723399-aad1-439c-8866-a9a170cba01c.png">

###  Upper Confidence Bound (UCB)

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197330261-3159a667-4e10-43b9-983c-488fe8d4ebb3.png">

  + t: time step or play number
  + Q<sub>t</sub>(a): 시간 / 에서 action a에 대한 reward 추정치 (simple average)
  + N<sub>t</sub>(a): action $를 선택했던 횟수
  + c: exploration을 조정하는 하이퍼파라미터
    + 새로 추가된 term이 해당 action이 최적의 action이 될 수도 있는 가능성 (불확실성)

### MAB 알고리즘의 파라미터 튜닝
  + 최적의 policy를 위해서 적절한 하이퍼 파리미터를 찾아서 trade off가 optimal한 부분을 찾아야함

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197330558-26e47bbd-9ced-4902-ab55-45814db4f3b8.png">

### MAB를 이용한 추천 시스템

+ 베경
  + 기존 추천 시스템과 어떤 차이가 있는가?
    + 실제 서비스의 지표인 클릭/구매를 모델의 reward로 가정
      + (기존의 추천시스템들과는 조금 다른 접근)
  + 해당 reward를 최대화하는 방향으로 모델이 학습되고 추천을 수행함
  + 무거운 추천 모델을 사용하지 않고 간단한 Bandit 기법을 적용하여도 온라인 지표가 좋아짐 (CTR, CVR)
  + 추천하는 개별 아이템은 개별 action에 해당함
  + 유저에게 아이템을 추천하는 방식이 MAB 알고리즘의 policy
  + 이템을 추천했을 때 사용자의 클릭 여부를 reward로 측정
  + 구현이 간단하고 이해가 쉬움 → 실제 비즈니스 어플리케이션에 매우 유용함
    + exploration ... 지속적으로 변화하는 유저의 취향 탐색 및 추천 아이템 확장
    + exploitation ... 유저의 취향에 맞는 아이템 추천
  
+ 유저 추천 (유저에게 아이템을 추천)
  + 개별 유저에 대해서 모든 아이템의 Bandit을 구하는 것은 불가능하다.
    + 개인별로 구축하기에는 데이터가 부족하여 Bandit이 수렴하지 않는다.
  + 클러스터링을 통해 비슷한 유저끼리 그룹화한 뒤에 해당 그룹 내에서 Bandit을 구축해 활용한다.
    + CF, 인기도 기반등의 다양한 방법으로 클러스터 별 아이템 후보 리스트를 생성한다.
    + 필요한 Bandit의 개수 = 유저 클러스터 개수 x 후보 아이템 개수

  <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197331349-ce780a6b-1a7e-4b5c-af9e-a6307684c798.png">


1. 다양한 아이템들을 각각의 유저 클러스터에 할당한 
2. 어떤 특정 유저가 들어왔을 때 해당 유저의 클러스터가 무엇인지 찾고 
3. 그 클러스터에 있는 후보 아이템들에 대해 MAB 알고리즘 진행

+ 유사 아이템 추천
  + 주어진 아이템과 유사한 후보 아이템 리스트를 생성하고 그 안에서 Bandit을 적용함
  + MF, Item2Vec 기반의 유저-아이템 상관관계나, Content-Based 기반의 유사도를 기반으로 한 유사도를 할용해 아이템 리스트를 생성한다.
  + 필요한 Bandit의 개수 = 아이템 개수 x 후보 아이템 개수

  <img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197331480-fb7cb7b0-5d4e-4906-8403-b9fde1c545f0.png">
  
  1. 입력으로 주어진 아이템과 비슷한 아이템 후보들을 MF나 CB로 생성함
  2. 후보들 중 MAB를 통해 클릭이 가장 많을것 같은 아이템을 추천.
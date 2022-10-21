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
  
$q_{*}(a) = E[R_{t}|A_{t}=a]$

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

$$A_{t}=\argmax _{a}Q_{t}\left( a\right) $$

### Epsilon-Greedy Algorithm
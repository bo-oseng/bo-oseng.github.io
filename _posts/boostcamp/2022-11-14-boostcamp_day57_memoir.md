---
layout: single

title: 부스트캠프 AI 9주차(Day-57) 회고록, DKT 대회 - (1) DKT 이해 및 DKT Trend 소개

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, DKT, Deep Knowledge Tracing, DKT 대회]

toc: true
---

## Day 57

# DKT 이해 및 DKT Trend 소개

## DKT Task

### Knowledge Tracing
+ 학습자의 실력을 바탕으로 학습자의 전체 지식 수준을 추적 및 평가하는 Task를 말한다.
+ 지식 상태는 계속 변화한다. 그리므로 변화하는 지식상태를 지속적으로 추적해야 한다.

<center>
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/201623367-f81cb953-2f35-4159-8826-840cc5c5b5f4.png">
</center>
<br>
  
### DKT
+ Deep Learning + Knowledge Tracing
+ 딥러닝을 이용하는 지식 상태 추적을 의미한다.
+ DKT를 통해 학생이 얼마나 아는지를 측정할수있다.
+ " 곱하기가 약하구나, 우리 그 부분을 더 공부해보자", "빼기를 더 풀 필요가 없겠다, 곱하기를 더 공부해보자", "이 순서대로 문제를 풀면 너에게 가장 효율적일거야"
  
<center>
    <img width="70%" style="margin-bottom: 5px;" alt="image" src="https://user-images.githubusercontent.com/94548914/201623630-694fdd7c-3d8e-4462-a9fe-0159582e311f.png">
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/201622923-8cc970ce-fb47-4769-bd44-179a9a76009c.png">
</center>

### DKT 대회
+ 본 대회에서는 지식상태보다는 주어진 문제를 맞췄는지 틀렸는지에 집중한다.
+ TEST SET은 맨 마지막 문제의 정답 여부가 주어지지 않으며 이를 예측해야 한다.


## Metric

<center>
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/201625335-211d8030-af86-4b5f-9b03-77027a82356a.png">
</center>

### ACC(Accuracy)

+ ACC = (TP + TN) / TOTAL

### AUC(Area Under the ROC Curve)

<center>
    <img width="70%" style="margin-bottom: 5px;" alt="image" src="https://user-images.githubusercontent.com/94548914/201627617-39f39ca9-cfff-4a47-8323-f2c07b3d6e1d.png">
    <img width="70%" style="margin-bottom: 5px;" alt="image" src="https://user-images.githubusercontent.com/94548914/201627617-39f39ca9-cfff-4a47-8323-f2c07b3d6e1d.png">
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/94548914/201628315-bdc80c5d-4974-4528-a131-ef2466b8fdef.png">
</center>

+ ROC( Receiver Operating Characteristic Curve ): x축이 FPR, y축이 TPR를 나타내는 커브.
  + FPR: False Positive Rate, FP / ( TN + FP )
  + TPR: True Positive Rate, TP / ( FN + TP )
+ 0과 1의 분포의 차이를 보는 Metric이다.
+ AUC는 척도불변이다. 절대값이 아니라 예측이 얼만 잘 평가되는지를 측정한다.
  + 잘 보정된 확률 값이 직접적으로 필요한 경우에는 적절치 않다.
+ AUC는 분류 임계값(Threshold) 불변이다. 어떤 분류 임계값이 선택되었는지와 상관없이 모델의 예측 품질을 측정한다.
  + FN, FP, TN, TP 중 특정 값을 우선시 하려는 목적을 가졌을 때 적절치 않다.

## DKT History 및 Trend

## Sequence Data

1. RNN: 장문장에서 거리가 먼 단어와의 관계 정보가 소멸된다는 단점이 있다.
2. LSTM: RNN의 장기기억 문제를 개선했다. 문장을 입력받아, 문장을 생서하는 모델을 연구해보자.
3. SEQ2SEQ: 문장이 길어졌을 때 긴 문장을 하나의 Context Vector로 표현하는데 한계가 있었다.
4. Attention: RNN 방식의 학습속도 한계를 개선했다.
5. Transformer: 단어간의 관계를 끊고 Position Embedding 활용해 병렬적으로 연산을 한다.


## Appendix

### 의문점

### 피어섹션
level 2 팀원들을 처음 만나는 시간을 가졌다. 수요일에 있을 우팀소를 준비했고 그라운드룰 정했다.
---
layout: single

title: 비트마스크와 전가산기 with python

categories:
  - Algorithm

tag: [알고리즘, 비트마스크, BitMask, python, 전가산기, Fulladder]

toc: true

---
# 가산기란

>배경    
>[비트마스크](https://bo-oseng.github.io/algorithm/bitmask/)에 대해서는 이전에 다루어 보았다. 이전 글에서는 비트마스크를 활용해서 대소문자 변환을 해봤었다. 이번 글에서는 하드웨어에서 다루는 전가산기에 대해 알아보고, 이를 python에서 구현 해보자.
   
가산기란 논리 게이트(AND, OR, NOT, XOR 등)들의 조합으로 이루어진 연산 장치이다. 가산기는 입력으로 주어진 신호를 더해 더한 값을 출력하는 덧셈연산을 수행한다.  
   

|논리게이트 | 기호|
|:-----:|:-:|
|OR|![OR](https://upload.wikimedia.org/wikipedia/commons/b/b9/OR-gate-US.png)|
|AND|![AND](https://upload.wikimedia.org/wikipedia/commons/0/03/AND-gate-US.png)|
|XOR|![XOR](https://upload.wikimedia.org/wikipedia/commons/6/61/XOR-gate-US.png)|



### 반가산기
<center>
  <img src="https://upload.wikimedia.org/wikipedia/commons/1/14/Half-adder.svg" width="50%">
</center>
가산기 중 반가산기는 한자리 비트 두개 A, B를 입력받아 더하고 더한값을 Carry와 Sum으로 출력하는 연산 장치이다. 이때 두 비트를 더해 자리 올림이 발생한다면 Carry의 값은 1이되고, 발생하지 않는다면 0이된다.

### 전가산기

<center>
  <img src="https://w.namu.la/s/23746a0db87056a06f685a676186bca7a8a2c99dc982639a6bd10e00a3c6c25853d5bfa2b7fdbf21d3286c1848920d3a3529c820ac9b3eb9095628557f26f7f7dab38902242eb7b3059be18b456c8e5b8e2b63817413f569d6f88a0f1028a611dd8ac7babdfaff471a4018c3a4f46537" >
</center>   
<center>
  <p>출처: 파이썬 알고리즘 인터뷰</p>
</center>    
<br>   

전가산기는 이전 연산에서 발생한 Carry를 Carry In 그리고 A, B를 입력 받아 세 입력의 합을 더해 더한 결과를 Carry Out과 Sum으로 출력하는 연산장치이다. 그리고 이때 발생한 Carry Out은 더 높은 자리수의 Carry In으로 입력되는 식으로 구성되어있다. 컴퓨터의 덧셈은 이런식으로 전가산기를 활용해 덧셈 연산을 진행한다.

### 비트마스크와의 관련
파이썬에서 음수를 다루는 방법 때문에 비트 마스크가 필요하다. 
컴퓨터에서 덧셈과 뺄셈 연산을 할때에는 특성상 2의보수를 활용하는 방법이 가장 적절하다. 
하지만 파이썬에서 음수는 부호필드와 값 필드를 따로 가지고 저장하고 있다가, 
연산시에 내부적으로 2의 보수로 변환해 연산을 진행하는 식으로 이루어 져있다.
+ 예시 1
    ```python
    >> bin(-10) // 0x 1111 1111 1110 0110(기대한 2의 보수)
    >> '-0b1010' // (-1) * 0x 0000 0000 0000 1010 // 기대한 2의보수 값이 아닌 파이썬이 내부적으로 처리한뒤의 값이 나온다
    ```   
+ 예시 2
    ```python
    >> 5 & -4 // 0x 0000 0000 0000 0101 & 0x 1111 1111 1111 1100
    >> 4 // 0x 0000 0000 0000 0100 -> 연산의 결과는 정상적으로 나오는것을 알 수 있다.
    ```
+ 비트마스크 활용 예시
    ```python
    MASK = 0xFFFFFFFF
    x = bin(-10) // '-0b1010'
    y = bin(-10) & MASK // '0b11111100' -> 비트마스크를 통해 2의 보수로 명시적으로 나타낼수 있음
    ```
    ```bash
    x: -0b1010
    y: 0b11111100
    ```
내부적으로 사람이 이해하기 복잡한 2의보수를 대신 처리해주는 파이썬이 대부분의 경우에 편하지만 비트단위로 접근하고자 할때는 오히려 방해가 되는 부분이 있다.
이때 명시적으로 2의보수처럼 다루기 위해서는 비트마스크가 필요하다.   


### 전 가산기 파이썬으로 전체 구현

```python
def getSum(a: int, b: int) -> int:

    MASK = 0xFFFFFFFF
    INT_MAX = 0x7FFFFFFF
    
    a_bin = bin(a & MASK)[2:].zfill(32)
    b_bin = bin(b & MASK)[2:].zfill(32)

    result = []
    Sum = 0
    Carry = 0

    for i in range(32):
        A = int(a_bin[31 - i], 2)
        B = int(b_bin[31 - i], 2)

        Q1 = A & B
        Q2 = A ^ B
        Q3 = Q2 & Carry
        Sum = Q2 ^ Carry
        Carry = Q1 | Q3

        result.append(str(Sum))

    if Carry == 1:
        result.append('1')

    result = int(('').join(result[::-1]), 2) & MASK

    if result > INT_MAX:
        result = ~(result ^ MASK)

    return result

print(getSum(-3, -5))
```
```python
>> -8
```
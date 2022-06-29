---
layout: single

title: 병합정렬 직접 구현해보기 with python
categories:
  - Algorithm

tag: [알고리즘, 병합정렬, Merge Sort, python]

toc: true

---

# 병합정렬이란   

+ 병합 정렬은 원소가 한 개가 될 때까지 계속해서 반으로 나누다가 다시 합쳐나가며 정렬을 하는 분할 정복 방식의 정렬이다.
+ 재귀적인 구조로 구현할 수 있고, 폰 노이만이 설계한 정렬이다.   
  
<center>
  <img src="https://w.namu.la/s/1850a8d6f7f4bbc1343082a38bd4a3fb8170dfbbc1af00ae1923cfb86b5412314824821d60b902216b3eff6830527194d0fae3222ae4843b68665848713b2ce9a71a879b1c61f804682cc2780963be0a80d5244b22e744aec22ac32b6339a0c119b59aff689bc0dfea10e6e08d65bcbe">
</center>
<br>   


## 병합정렬 코드   

코드는 원소가 1개가 될때까지 재귀적으로 _merge_sort를 호출하다가 원소가 1개가 되면 return이 되며 같은 depth에 있던 배열끼리 크기를 비교하며 병합해나가 최종적으로 전체 배열을 정렬 하는 식으로 구성된다, 

```python
def merge_sort(a) -> None:

    def _merge_sort(a, left: int, right: int) -> None:

        #  left >= right가 되면 원소가 배열의 1개이다.
        if left < right:
            center = (left + right) // 2

             # 원소가 1개가 될때까지 재귀적으로 호출해준다.
            _merge_sort(a, left, center)
            _merge_sort(a, center + 1, right)

             # 배열의 인덱스를 다루기 위한 변수를 4개 선언한다.
            j = p = 0
            i = k = left


    ''' 
    임시 배열 buff에 a 원소를 center번째 까지 복사하고
    i는 a의 나머지에 접근하기 위해 (center 이후부터)
    p는 buff의 크기를 알기 위해사용된다.
    '''
            while i <= center:
                buff[p] = a[i]
                p += 1
                i += 1

        
    '''
    i가 a의 끝인 right에 도달하거나, 
    j가 buff크기(j == p)가 되기 전까지 
    크기를 비교하며 a[0]번째 부터 채워 나간다 (병합)
    '''
            while i <= right and j < p:
                if buff[j] <= a[i]:
                    a[k] = buff[j]
                    j += 1
                else:
                    a[k] = a[i]
                    i += 1
                k += 1


    '''
    전 단계의 반복문이 끝나고 j < p를 만족한다면 
    buff에 남은 배열들 모두가 a의 k번째까지의 원소들보다 모두 크다는 뜻이다.
    buff는 이미 재귀적 호출에서 정렬이 완료된 원소들이므로
    그대로 a에 복사해주면 된다.
    '''
            while j < p:
                a[k] = buff[j]
                k += 1
                j += 1

    n = len(a)
    buff = [None] * n
    _merge_sort(a, 0, n -1)
     # 메모리를 할당을 해제 해준다.
    del buff
    return a


print(merge_sort([3,4,1,8,9,33,21]))
```   
```python
>> [1, 3, 4, 8, 9, 21, 33]
```   



### 병합정렬 특징
+ 병합정렬은 상한, 하한, 평균 모두 nlog(n)의 시간복잡도를 기대할 수 있는 알고리즘이다.
+ 같은 수끼리는 순서가 바뀌지 않는 안정 정렬이다.
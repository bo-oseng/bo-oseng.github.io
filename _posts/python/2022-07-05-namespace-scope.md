---
layout: single

title: Python의 Namespace와 Scope

categories:
  - Python

tag: [scope, namespace, global, nonlocal]

toc: true

---
# Python의 namespace와 scope
> 배경
> 
> 파이썬으로 코딩을 하다보면, 함수 내부에서 외부 변수를 다루기 위해 global이나 nonlocal 키워드를 쓰는 경우가 간혹 있다.
> 쓸때마다 정확한 개념이 헷갈려서 확실하게 정리하려고 한다.

## Namespace란
### global의 namespace
namespace는 개체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 개체만을 가리키게 된다. 
```python
a = 'save_at_globlas_namespace'
print(globals())
```
```
{'__name__': '__main__', '__doc__': None, '__package__': None,
 '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x104524340>, '__spec__': None, '__annotations__': {}, 
 '__builtins__': <module 'builtins' (built-in)>, '__file__': '/Users/gimboseong/Desktop/data-structure/tmp.py', 
 '__cached__': None, 
 'a': 'save_at_globlas_namespace'}
```
파이썬에서 들여쓰기를 한번도 하지 않은 공간은 global에 해당하고, global에 변수를 선언하면 key, value 형태로 연결되어 저장되게 된다. 이때 global의 namespcae를 출력하는 함수 globals()의 결과를 살펴보면 a와 그 값이 저장된걸 볼 수 있다.   
   
### 외부함수와 내부함수의 namespace
다음은 내부함수와 외부함수의 namespace 예시를 살펴보자.
   
```python
a = 'save_at_globlas_namespace'

def outer():
    a = 100
    b = 'save_at_outer_locals_namespace'
    print('outer namespace: ',locals())
    print()

    def inner():
      print('inner namespace: ', locals())
      print()
    inner()

print('globals namespace: ', globals())
print()
outer()

```
```
globals namespace:  
{'__name__': '__main__', '__doc__': None, '__package__': None, 
'__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x10293c340>, '__spec__': None, '__annotations__': {}, 
'__builtins__': <module 'builtins' (built-in)>, 
'__file__': '/Users/gimboseong/Desktop/data-structure/tmp.py', 
'__cached__': None, 'a': 'save_at_globlas_namespace', 
'outer': <function outer at 0x1029e4430>}

outer namespace:  
{'a': 100, 'b': 'save_at_outer_locals_namespace'}

inner namespace:  
{}

```
위 와 같이 블록마다 각각의 namespace를 가지는걸 볼 수 있다. 다시말해 내부함수를 포함해 각각의 함수들과 전역 블록은 각각의 namespace를 가지고 있다.   
```python
x = 5
def outer():
    y = 3

print(x + y)
```
```bash
NameError: name 'y' is not defined
```
   
이렇게 서로 다른 namesapce때문에 생기는 문제가있다. 사실 문제라기보다는 더 코드를 좋게 짜기위한 일종의 패턴이다. 다른 영역에서 선언된 변수를 개발자의 의도와 다르게 변형된다면 찾기 힘든 버그가 생길 수 있기 때문이다. 그리고 이를 위해 변수를 접근하는데있어 중요한개념인 scope가 있다.


## Scope
Scope란 변수의 유효범위를 의미하며 참조 대상을 찾아내기 위한 규칙이다. 다음을 살펴보자.
```python
x = 5

def outer():
    y = 7
    print(x + 5)

    def inner():
      print(x + y)

    inner()
outer()

```
```bash
10
12
```

inner함수에서  x, y 모두 선언되지 않았고, outer함수에서 y가 선언되지 않았음에도 정상적으로 다른 namespace의 변수에 접근할 수 있다.    
   
상위 스코프에서의 하위 스코프로는 변형과 참조 모두 불가능하지만 하위 스코프에서 상위 스코프에 있는 변수의 변형은 불가능하나 참조는 가능하다.   

파이썬에서 스코프를 탐색하는 우선순위는 다음과 같다.
1. local namespace의 공간에 선언된 변수(내부함수)
2. local namespace의 공간에 선언된 변수(외부함수)
3. global namespace의 공간에 선언된 변수
4. built-in namespace의 공간에 있는 변수


## global, nonlocal 키워드
### globla, nonlocal 키워드란
scope와 namespace를 통해 얻을 수 있는 장점도 있지만, 간혹 하위 스코프에서 상위 스코프의 변수를 수정해야 할때가 있다.
```python
x = 10


def outer():
    x += 20
    print(x)


    def inner():
        x + 30
        print(x)

    inner()
    print(x)

print(x)
outer()
```
```
UnboundLocalError: local variable 'x' referenced before assignment
```

이때 필요한게 global 키워드 이다. global 키워드를 통해 변수를 선언하면 해당 변수는 이 스코프의 namespace에 등록하지 않게하고, 그러므로 변수를 찾기위해 상위스코프를 타고 올라가 전역변수 globals에 namespace에서 발견한 x를 참조하겠다는 뜻이된다.   

```python
x = 10


def outer():
  global x
  x += 20
  print(x)


  def inner():
    global x
    x += 30
    print(x)

  inner()
  print(x)

print(x)
outer()


```
```
10
30
60
60

```
위와 같이 global x 를 선언함으로써 전역변수 x를 outer함수와 inner함수에서 변형시킬 수 있게 된다.   
   
nonlocal은 global과 비슷하나, 중첩함수에서 내부함수에서 외부함수의 변수를 다룰때 쓰인다.
```python
def outer():
  x = 30
  print(x)


  def inner():
    nonlocal x
    x += 30
    print(x)

  inner()

outer()

```
```
30
60
```
global과 비슷하게 x를 local namespace에 등록하지 않고 상위 스코프를 타고 올라가 outer함수의 namespace에 있는 x를 참조하게 된다.
   

### 주의점
앞서 말했듯 namespcae와 scope는 다른 영역에서 선언된 변수를 영역이 다른 스코프에서 변형해 찾기 힘든 버그가 생기는걸 방지하기 위해 고안된 패턴이다. 그러므로 globals와 nonlocal응 남용하게 되면 이러한 버그가 생길 수 있다. 그러므로  return을 통해 값을주고 받는 방식을 권장하고 꼭 필요할 경우에만 사용해야한다.
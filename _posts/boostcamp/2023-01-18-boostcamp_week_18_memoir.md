---
layout: single

title: numpy 자료형과 Pydantic에 의해 발생했던 버그

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, pandas, dtype, fastapi, pydantic, numpy.int64, int, item()]

toc: true
---

### 배경

부스트캠프 파이널 프로젝트의 추천 서비스의 프로타입을 만들던 중 찾기 어려운 버그가 발생했었다.   


해당 버그 해결법과 그 과정에서 추가적으로 학습한 것들을 정리하고자 한다.   
 
    AI모델: pandas==1.5.2, numpy==1.24.1, torch==1.13.1, scipy==1.10.0을 활용한 EASE 모델
    백엔드: fastapi==0.89.1, 
    프론트: react==18.2.0

# 발생했던 버그 "Exception in ASGI application"

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/213140615-b9d1243a-7023-426f-ad83-34e4e757716e.png">

<div> Exception in ASGI application </div> 
<div style='color: gray;'>ASGI는 fastapi가 따르고 있는 서버의 인터페이스 정책이다. </div>  
<div style='color: gray; font-size: 10px;'>(ASGI is a spiritual successor to WSGI, intended to provide a standard interface between async-capable Python web servers, frameworks, and applications.)</div>

<br>

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/213141307-c9ce01cd-f711-4a9e-a45a-602ffe4efde7.png">

<div style='color: gray;'>문제가 발생했던 fastapi 코드, inferecne_result들이 아이템의 id를 담은 List[int]의 아웃풋을 기대했다.</div>  

<br>

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/213141919-6415c376-4f03-4a4e-a66c-33fec1d9581c.png">


<div style='color: gray;'>문제가 발생했던 model의 get_model_rec 코드, result는 아이템의 id를 담은 List[int]의 아웃풋을 기대했다.</div>   

<br>


## 버그 원인분석

<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/213141307-c9ce01cd-f711-4a9e-a45a-602ffe4efde7.png">

모델에서 **pandas로 csv를 읽어올 때** int는 파이썬의 기본형 int가 아닌 **numpy.int64**로 설정된다.  

그렇다면 numpy.int64은 왜 문제가 됐을까?  

원인은 fastapi의 pydantic 에 있다.

pydantic은 vaildation을 쉽고 빠르게 검증하기 위한 라이브러리로 pydantic을 통해 fastapi는 복잡하고 긴 코드의 vaildation을 쉽게 컨트롤할 수 있다.  

버그가 발생한 코드의 make_inference_track의 return type은 fastapi의 swagger를 통해 살펴보면 다음과 같다.  

<img width="1430" alt="image" src="https://user-images.githubusercontent.com/94548914/213181813-ae877228-cfe7-4656-922e-975e1e1371eb.png">

swagger의 docs에 따르면 return으로 {} 형태의 json이 와야한다.  

return {'inference_result': inference_result}의 json으로 변환하는 과정에서 numpy.int64를 만나 오류가 발생한것으로 보인다.  

## 버그 해결법

### 해결법 - (1)

문제가 되는 numpy.int64를 int로 바꿔주는 방법이다.  

여기서 주의할 점은 ndarray의 변수에 int() 메소드를 씌워도 여전히 numpy.int64가 된다는 것이다.  

이 부분에서 조금 당황하고 헤멨었다. 처음 오류를 보자마자 시도했던 방법이 get_model_rec 내부에서 id2item 값들에 int() 메소드를 사용하는 방법이었으나 해결이 되지 않았었다.

이는 numpy.int64 변수의 클래스를 스코프를 따라 타고 올라가다보면 파이썬의 기본 자료형인 int가 아닌 numpy.int64를 먼저 찾게되기 때문에 발생했던 오류였다.

이를 해결하기 위해선 다음과 같은 item() 메소드를 쓰면 된다.

```python
# At EASAE_model.py
def get_model_rec(model, input_ids, top_k) -> EASE:
    ...
    ...
    result = [id2item[i].item() for i in result[0]] # numpy.int64 -> int

    return result
```

numpy.int64 -> int 로 의도한대로 자료형이 변환된다.

### 해결법 - (2)
함수의 return type을 response_model 파라미터를 통해 json이아닌 List로 정해주면 된다.

response_model를 통해 json의 schema가 아닌 custom type을 사용자가 직접 지정할 수 있다.

단, 이 경우 response_model 파라미터로 설정한 값의 타입과 같은 return을 반환하지 않는다면 오류가 생긴다.

```python
# At fastapi_backend.py

@app.post("/recplaylist", description="추천을 요청합니다.", response_model=List[int])
async def make_inference_track(request: Request):
    ...
    ...
    
    return inference_result

```

## Appendix

### read_csv 함수의 dtype 설정이 권장되는 이유

사실 이 이슈를 다루면서 pandas가 read_csv을 통해 파일을 읽어올 때 파이썬의 기본 자료형이 아닌 numpy의 자료형으로 읽어온다는 사실을 처음 알았다.  

생각해보면 효율적인 메모리 사용을 위해서 불가결한 부분인데 당연하게 쓰다보니 조금 간과했던 부분이다.  

그렇다면 csv가 저장될 시점의 자료형을 같이 저장했다가 read_csv 실행시 자료형을 같이 불러오는건가 라는 의문이 생겨 서칭을 했다.  

서칭결과 read_csv을 통해 csv를 읽어올 때 pandas가 컬럼의 타입을 동적으로 추론하고 이 과정에 많은 메모리가 소모된다고 한다.  

그래서 read_csv 함수에 dtype 파라미터를 통해 type을 지정해주면 메모리가 최적화되고 속도가 매우 향상되므로 dtype 설정이 권장된다.
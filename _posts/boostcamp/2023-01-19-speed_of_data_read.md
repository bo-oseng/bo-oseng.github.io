---
layout: single

title: numpy 자료형과 Pydantic에 의해 발생했던 버그

categories:

- Boostcamp

tag: [Boostcamp, 부스트캠프, 회고록, pandas, dtype, fastapi, pydantic, numpy.int64, int, item()]

toc: true
---

### 배경

프로젝트 진행 중 실시간 추천 모델의 Inference를 최적화 하던 중 데이터를 불러올 떄 aws의 RDS의 속도와 로컬에서 읽는 속도의 차이가 얼마나 날지 궁금했고 실험을 진행했다.

해당 실험의 결과와 나의 생각을 정리했다.

# 데이터 읽기 속도 비교

## 70만개의 데이터를 읽어올 때 각 방법별로 걸린 시간을 측정했다.

### 실험 환경

+ CPU: Intel(R) Xeon(R) Gold 5120 CPU @ 2.20GHz
+ GPU: Tesla V100


```python
# read_json 13.5s
song_meta = pd.read_json('/opt/ml/final/data/train/song_meta.json')
```


```python
# read_csv 2.8s

read_csv = pd.read_csv("/opt/ml/final/data/train/song_meta_original.csv", sep=";")
```


```python
dtypes = read_csv.dtypes.to_dict()
```


```python
# read_csv_with_dtypes 2.7s

read_with_dtypes = pd.read_csv("/opt/ml/final/data/train/song_meta_original.csv", sep=";", dtype=dtypes)
```


```python
# read_csv_with pyarrow 0.9s

read_with_dtypes_and_pyarrow = pd.read_csv("/opt/ml/final/data/train/song_meta_original.csv", sep=";", dtype=read_csv.dtypes.to_dict(), engine="pyarrow")
```


```python
# pymysql rds로 읽어오기 11.7s
cursor.execute("SELECT * FROM song_meta_original")
res = cursor.fetchall()
```

### 결론

RDS와 물리적인 하드에서 읽는 방식의 속도차이가 생각보다 많이났다.  

실시간으로 서빙을 제공하는 입장에서 크리티컬 할 정도의 차이라 유동성이 없고 고정적인 데이터는 최대한 모델이 실행될 서버에 데이터를 같이 저장하는게 최선이라고 생각된다.

특히 engine="pyarrow" 방식으로 불러오는 속도는 0.9s인 반면 pymysql을 거쳐 불러오는 속도는 11.7s로 거의 11배 이상 차이가 난다.




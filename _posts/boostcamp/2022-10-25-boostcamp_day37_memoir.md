---
layout: single

  

title: 부스트캠프 AI 5주차(Day-37) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, 추천 시스템 정의, 부캠대회, Book rating Predition]


toc: true
---

## Day 36

### Unsupervised Learning Mode

+ Latent Factor Model
  + 유저와 아이템 사이 다양한 정보를 담은 데이터를 축약된 벡터 공간에 표현할 수 있다는 아이디어
  + 축약된 벡터가 가까운 유저 혹은 아이템은 유사하다고 볼 수 있음
  + 저차원으로 매핑 했을 때 시각적으로 유저와 아이템의 유사성을 시각적으로 판단할 수 있으나, 각 잠재변수의 의믜를 정확히 알수 없음 
  + Latent Factor 행렬들을 만들어 이들의 행렬곱으로 User-Item 행렬에서 비어있는 값을 유추한다.

+ Singular Value Decomposition 
  + SVD 이용하여 행렬을 분해하고 차원 축소한다.
$$R=U\Sigma V^{T} (Full SVD)$$
$$R\approx \widehat{U}^{r}\sum _{k}\widehat{V}^{T} (Trucated SVD)$$
+ Matrix Factorization 
+ Alternative Least Square
+ Hybrid approach

### Supervised Learning Model

+ Naive Bayes
+ GBDT(Gradient Boosting Decision Trees)
+ Deep Learning

### 부캠 Book Rating Prediction 대회
- user
  - 나라에 따른 언어
  - 지역에 따른 유행
  - 연령대 <<
- book
  - 카테고리/장르 살펴보기 → 나중에 문제되면 수정.
  - 같은 publisher에서 책을 구매한적 있던 사람
  - summary word2vec 유사도
  - 책 제목 길이랑 레이팅 선호도의 관계를 한번보자 (개인적으로 짧은 제목이 더 강렬했던  듯)
  - isbn 찾아보기
  - 각 책의 평균 rating
- train_rating
  - 평점의 분포 살펴보기
  - 연령대에 따른 장를 선호도
  - 시간이 지날수록 평점이 높아짐 → 재평가
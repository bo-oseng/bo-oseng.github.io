---
layout: single

  

title: 부스트캠프 AI 8주차(Day-50) Book Rating Prediction 대회

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, 부캠대회, 프로젝트. Ai stages]


toc: true
---

# 랩업리포트

**RecSys 14조**

# Part 1. 프로젝트 Wrap Up

## 1-1. 프로젝트 개요

### 1-1-1. 프로젝트 목표

  대한출판문화협회에 따르면 2021년 대한민국 신간 발행 책은 64,657권이다. 책을 읽기 위해 소요되는 시간과 책을 구매하는 비용 측면에서 구매할 책을 선택하는 것은 중요한 문제이다. 하지만 소비자는 제목, 저자, 표지, 카테고리 등 그 책의 정보와 타 구매자들의 리뷰와 평점만으로 책을 선택해야한다.

 본 대회는 소비자들의 책 구매 결정에 도움을 주기 위해 소비자가 책에 대해 내릴 평점을 예측하는 것을 목표로 한다.

### 1-1-2. 프로젝트 구조

```bash
|📦 code
|    |📂 data
|        |📜 images
|        |📜 train_ratings.csv
|        |📜 test_ratings.csv
|        |📜 books.csv
|        |📜 sample_submission.csv
|        |📜 users.csv
|    |📂 models
|        ****|**📜** FFM.pt
|    |📂 src
|        |📂 data
|            |📜 __init__.py
|            |📜 dl_data.py
|            |📜 context_data.py
|            |📜 image_data.py
|            |📜 text_data.py
|        |📂 ensembles
|            |📜 ensembles.py
|        |📂 models
|            |📜 _models.py
|            |📜 dl_models.py
|            |📜 context_models.py
|            |📜 image_models.py
|            |📜 text_models.py
|        |📜 __init__.py
|        |📜 utils.py
|    |📂 submit
|    |📜 main.py
|    |📜 ensemble.py
```

### 1-1-3. 데이터셋 구조

 주어진 데이터는 `.csv` 형식의 파일로, 사용자(user)과 책(item)의 정보 그리고 사용자(user)가 책(item)에 매긴 평점 데이터이다.

![https://s3-us-west-2.amazonaws.com/aistages-prod-server-public/app/Users/00000004/files/e216d971-e310-45ce-b1d1-707c255b666f..png](https://s3-us-west-2.amazonaws.com/aistages-prod-server-public/app/Users/00000004/files/e216d971-e310-45ce-b1d1-707c255b666f..png)

- `users.csv` : 68,092명의 사용자(user)에 대한 정보를 담고 있는 메타데이터이다.
- `books.csv` : 149,570개의 책(item)에 대한 정보를 담고 있는 메타데이터이다.
- `train_ratings.csv` : 59,803명의 사용자(user)가 129,777개의 책(item)에 대해 남긴 306,795건의 평점(rating) 데이터이다.

### 1-1-4. 출력 데이터

 최종 결과물은 주어진 책에 대해 사용자가 매길 것이라고 예상하는 평점을 채워 넣은 `.csv` 형태의 파일로, 아래 `sample_submission.csv` 파일과 같이 출력해야 한다.

| user_id | isbn | rating |
| --- | --- | --- |
| 11676 | 0002005018 | 7.4152054786 |
| 116866 | 0002005018 | 8.3852987289 |
| 152827 | 0060973129 | 8.0158720016 |

### 1-1-5. 협업 도구

**Slack**

![Slack1](https://user-images.githubusercontent.com/94548914/200845593-93c447d4-801f-4fe3-9d68-8dfaf39243ca.png)

![Slack2](https://user-images.githubusercontent.com/94548914/200845596-d726a33a-b3ac-41fb-a371-8310a3aeca68.png)

파일 공유, 논의해야할 사항을 Slack을 이용해 공유하였다.

---

**Github**

![Git hub](https://user-images.githubusercontent.com/94548914/200845590-710951b6-ad66-4be5-87fa-69276398e5be.png)

새로운 실험과 버전관리를 위해 Git과 Github를 활용했다.

---

**Vscode Live Share**

![VS live server](https://user-images.githubusercontent.com/94548914/200845598-2e01158a-4c91-4b37-8cbc-d3b40f4bc300.png)

함께 코드를 공유해야할 상황에 vscode의 Live Share을 이용해 동시 작업을 하였다. 이를 통해 빠르고 효율적으로 오류 수정과 공유를 할 수 있었다.

---

**Notion**

![Notion](https://user-images.githubusercontent.com/94548914/200845592-4b017311-0ca9-4d4d-9090-e0d3a881cb05.png)

실험결과 Parameter와 RMSE를 쉽고 명확하게 공유하기 위해 노션을 활용했다.      

## 1-2. 프로젝트 팀 구성 및 역할

 대회의 처음부터 끝까지 모든 역할을 직접 경험 해보고 싶다는 의견이 많았다. 때문에 본 대회에서는 역할을 나누지 않고 `EDA` → `Feature Engineering` → `Model` → `Hyper Parameter Tunning` 의 과정을 다 같이 시행했다.

- 류지수(팀장) : FM, FFM 모델 연구 및 실험, EDA(결측치 보완), 하이퍼파라미터 튜닝 자동화
- 김다은(팀원) : NCF 모델 연구 및 실험, FFM 모델에 k-fold 적용.
- 김동영(팀원) : deepconn 모델 고도화. deepconn+FFM 모델 개발. 하이퍼파라미터 튜닝 자동화.
- 김보성(팀원) : Hybird Model(NCF + FFM) 실험, Custom Ensemble(Warm + Cold) 실험, lightFM 라이브러리 모델 연구 및 실험.
- 홍재형(팀원) : CNN_FM 연구 및 실험, EDA(결측치 보완)

## 1-3. 프로젝트 수행 절차 및 방법

![마일스톤](https://user-images.githubusercontent.com/94548914/200845576-fc51939b-3d87-4b42-b88c-4a0c919d95cc.png)

1. 학습 데이터 EDA, summary와 title 전처리
2. 팀원들이 baseline에서 각각 1개의 모델을 맡아 데이터의 흐름과 연산 과정을 점검
3. cold start 문제 해결 방법을 찾아보고, cold start에 강한 모델들을 tuning
4. feature 결측치 채우기
5. k-fold와 앙상블을 통해 성능 높이기

## 1-4. 프로젝트 수행 결과

**FFM에 K-Fold validation을 적용한 모델을 선택하였다.**

### 1-4-1. 데이터 분석 및 전처리

**User**

user_id

- user_id는 68,092개 모두 중복이 없다.

locaton

- location은 city, state, country 순으로 이루어진 문자열 데이터이며 결측치는 `n/a` 문자열로 채워져있어 전처리가 필요하다.
- location 정보를 city, state, country로 각각 분류하였다.
- city, state, country 각각에 존재하는 `n/a` 값은 문자열 ‘na’로 채웠다.

age

- age는 약 40%가 결측치가 존재했다.
- age의 결측치는 전체 user의 평균으로 채웠다.

**Book**

isbn

- 149,570개 모두 중복이 없다.

book_title

- 14,134개의 같은 이름의 책이 관측되었다.

book_author

- `P.J. O'Rourke`와 `P. J. O'Rourke`와 같이 동일 저자이나 공백과 대소문자로 인해 다른 저자로 분류되었다.
- book_author을 모두 소문자로 바꾼 뒤 공백을 제거하여 같은 저자인데 대소문자와 공백으로 인해 다른 저자로 분류되지 않도록 하였다.

year of publication

- 가장 낮은 데이터는 1376이다. 또한 1960년 이후의 출판된 책의 분포는 다음과 같다.
    
    ![연도별 출판 분포](https://user-images.githubusercontent.com/94548914/200845580-a3a65461-b710-4052-aae4-81444d950703.png)
    

publisher

- publisher 데이터 중 `Penguin USA (Paper)`, `Penguin USA`와 같은 표기 방법의 차이 및 오타로 인해 같은 그룹으로 묶이지 못한 데이터가 관측되었다.
- isbn의 publisher 고유번호를 기준으로 publisher를 1507개서 534개로 더 명확하게 그룹화했다.

img_url

- isbn의 데이터가 포함되어 있음이 관측되었다.
- img_url과 isbn의 일치 여부를 관측했고 모두 일치하였다.

summary

- 약 45%의 결측치가 존재하였다.
- `&#39;`와 같은 HTML 엔티티 데이터가 관측되었다.
- 관측된 HTML 엔티티를 원래의 문자로 대체하였다.
- 결측치는 book_title로 채웠다.
- 개행을 전부 없애고 lowercase로 변경했다.

language

- 약 45%의 결측치가 존재했다.
- 95%는 `en`이다. 또한 상위 5개의 language의 합이 98% 이상이다.
    
    <img width="229" alt="언어별 분포" src="https://user-images.githubusercontent.com/94548914/200845579-1c4050c2-0b9f-4af9-a2e5-2d83e05b8654.png">

- isbn의 처음 1~5자리 숫자가 국가코드인 것을 이용해 country 항목을 생성하여 채웠다.
- 결측값은 다른 책의 같은 book_author과 매핑시켜 결측값을 채웠다.
- country에서 `English`가 89.8%를 차지하였으므로 country가 `English`인 책은 1로, 아닌 책은 0으로 채웠다.
    
    ![Country](https://user-images.githubusercontent.com/94548914/200845585-c99c4968-bb5c-49fd-8eb6-e7b5d9fd6bff.png)
    

category

- 약 46%의 결측치가 존재했다.
- `1940-1049`와 같은 이상 데이터가 관측되었다.
- `history fiction`, `science fiction`과 같이 애매한 분류가 있었고, 카테고리의 개수가 지나치게 많았다.
- 유사한 카테고리를 묶어 카테고리의 수를 줄였다.
- `fiction` 카테고리가 50% 이상을 차지하였으므로 category의 결측치를 `fiction`으로 채웠다.

img_path

- img_path 데이터의 형식에 isbn의 데이터가 포함되어 있음이 관측되었다.
- img_path와 isbn의 일치 여부를 관측했고 모두 일치하였다.

**Ratings**

- 68,092명의 User와 149,570권의 Books의 Interection 중 관측된 정보는 306,795건으로 99.9%의 Sparse Matrix의 데이터가 주어졌다.
- Ratings의 분포는 다음과 같았다.
    
    <img width="461" alt="평점 분포" src="https://user-images.githubusercontent.com/94548914/200845582-87af6ef1-64aa-4e91-882c-2cecdfbdb070.png">
    

**Model Select**

- baseline 모델 중 cold start에 강한 모델인 ffm과 deepconn 을 선정했다.

### **1-4-2. 모델 개선**

- baseline에는 없는 earlystopping 기능을 추가하고, 보다 정확한 실험을 위해 rmse 도출 코드를 수정했다.
- shell script와 tensorboard를 이용해서 최적의 hyper-parameter을 찾고 학습시켰다.
- 10-fold validation을 목표로 모델을 학습 했으나 시간이 부족하여 4-fold vaildation의 결과를 제출하였다.

### 1-4-3. 최종 결과

**Model** : `FFM`

**Hyper-Parameter** : `batch_size 256` `learning_rate 0.01` `weight_decay 1e-4` `epochs 10`

10-fold (4번 fold에서 멈춤)

- CV score (valid) : RMSE 2.1758
- LB score (Public) : RMSE 2.1414
- LB score (Private) : RMSE 2.1418
![실험결과 순위](https://user-images.githubusercontent.com/94548914/200845577-01c5dffd-e8a7-4400-99f3-4d9b135c24e6.png)

## 1-5. 자체 평가 의견

**잘했던 점**

- 팀원 전체가 모든 과정에 참여한 점
- 베이스라인 모델을 나눠서 살펴본 점
- 나온 결과를 보고 토론한 점
- 다양한 시도를 해본 점
- 계속 같이하면서 하고 있는 것을 공유한 점

---

**시도 했으나 잘 되지 않았던 것들**

- 결측치 채우기
- cold start 문제 해결
- 이미지 기반 모델
- lightFM 모델

---

**아쉬웠던 점들**

- 목표를 구체적으로 정하지 않고 파트를 나누지 않은 점
- 생각만 하고 끝까지 도전하지 않은 점
- kaggle을 찾아보면서 방향을 빨리 잡지 못한 점
- 마스터클래스에서 소개했던 내용을 하지 않은 점
- 다른 평가 지표를 써보지 않은 점
- 다른 optimizer를 써보지 않은 점

---

**프로젝트를 통해 배운 점 또는 시사점**

 본 대회의 데이터는 범주형 데이터가 많았다. 상위권 팀들은 공통적으로 범주형 데이터를 잘 처리한다고 알려진 Catboost 모델을 활용했다. 반면 우리 팀은 Catboost 모델에 대한 사전 지식이 부족했다.

 모델의 구체적인 구조에 대한 깊은 이해도 중요하지만 엔지니어로서 문제 해결을 위해 각 모델들이 어떤 문제를 해결하고자 발전했는지, 어떤 특성의 데이터를 잘 처리하는지, 어떤 한계점이 있는지 특성들에 대해 체계를 잡아야 함을 배웠다.

# Part 2. 개인 회고

### 김보성

### **학습목표를 달성하기 위해 무엇을 어떻게 했는가**

**팀의 학습 목표.**

팀의 학습 목표는 본 대회의 `EDA` → `Feature Engineering` → `Model` → `Hyper Parameter Tunning`을 모두가 직접 경험해 보는 것이었다.

**팀의 학습 목표를 위해 노력한 것.**

 `EDA`와 `Feature Engineering` 등 같이 작업할 수 있는 부분은 팀원 모두가 모여 줌으로 진행했다. 명확한 역할 분배가 없어 각 과정에서 개인적인 호기심에 의한 실험이 많았으나, 자신이 실험을 하려는 배경과 기대되는 효과를 팀원들에게 납득시키려 노력했다.

**개인 학습 목표.**

개인의 학습 목표는 철저한 실험을 통한 성능의 향상이었다.

**개인 학습 목표를 위해 노력한 것.**

 타당한 논리를 바탕으로 실험을 설계하고자 했으며 실험 중 실험군과 대조군을 분리 시키려 노력했다. 

- Hybrid Model Experiment
    
    EDA를 진행하다 `test_ratings.csv`에 Cold Start 데이터가 35% 이상 존재하는 것을 발견했다. 또한 User와 Book의 Interection 중 관측된 정보는 306,795건으로 99.9% 이상의 Sparse Matrix가 데이터로 주어졌다. 때문에 Cold Strat 유저에 대한 성능 개선을 통해 General 한 성능 향상이 기대된다고 판단했고 이를 근거로 Hybrid Experiment를 설계했다.
    
     Hybrid Experiment는 Cold Start에 취약하다고 알려진 MF를 활용한 모델인 NCF를 대조군으로 설정한 뒤 Cold start를 보완했을 때 성능 향상을 관측하고자 했다. `test_ratings.csv` 으로 user, isbn의 조합이 주어지므로 Interaction이 존재하는 조합을 warm으로 처음 등장한 조합을 cold로 관리해 warm의 경우 NCF로 처리를, cold의 경우 FFM으로 데이터를 처리하는 NCF + FFM의 Hybrid 모델을 구성했다. 
    
     하지만 성능 향상이 매우 미미했고 하이퍼 파라미터에 따라 단일 모델이 더 나은 성능 보였다.
    
- Hybrid Ensemble
    
    Hybrid Experiment은 원하는 커스텀 모델마다 모델 코드를 다시 짜야 한다는 한계가 있었다. 더 다양한 모델의 조합에서 cold와 warm을 따로 관리한 결과를 관측하고 싶었다. 
    
    이를 위해 모델의 출력 결과를 저장한 `submission.csv` 들을 활용해 warm과 cold를 관리하며 ensemble을 할 수 있는 Hybrid Ensemble를 구성했다. 
    
     이후 NCF + DeepCoNN, FFM + DeepCoNN의 Ensemble을 진행했으나 성능이 오히려 떨어지는 결과가 관측되었다.
    
- Feature Engineering
    
     앞서 전처리를 진행했으나 여전히 데이터에 상당한 양의 결측치 관측되었다. 추가적인 Feature Engineering를 통해 결측치를 더 채워 보자는 의견이 나왔다. 몇몇 Feature Engineering은 주관적인 판단이 개입되는 만큼 객관적인 수치로 이를 분석하고자 했다.
    
     Feature를 활용한 모델인 FFM을 선택하였고 원본 데이터를 대조군으로 설정했다. 이후Feature Engineering을 적용한 데이터를 실험군으로 하여 각각의 Feature 변화 전후의 성능을 분석했다.
    
    (공동 실험자: 김다은, 류지수)
    
    <img width="1098" alt="Feature Enginerring 실험" src="https://user-images.githubusercontent.com/94548914/200845589-2250be3f-6a8d-4d9f-9dc6-40741caa84b8.png">
    
- LightFM
    
     lyst라는 패션몰 회사가 Cold start 문제를 개선하기 위해 연구한 모델이 LightFM이다. 라이브러리를 제공하고 있고 본 대회의 문제와 비슷한 면이 많다고 판단되어 lightfm 라이브러리를 설치하고 살펴봤다.
    
     lightfm의 특징은 feature로 사용하고자 하는 데이터에 결측이 없어야 하며, 아이템 간 우위를 정하기 위해 연산 중간에 scroe라는 값을 조작해 사용한다. 이 score의 절대적인 양을 잘 조작해 rating으로 변환하는 실험을 진행했다.
    
    하지만 score로 나오는 값을 분포와 본 대회의 rating 분포의 양상이 너무나 상이함을 발견했고 추가적인 실험은 진행하지 않았다. 
    

### **어떤 방식으로 모델을 개선했는가**

 결론적으로 cold start에 강할 거라고 판단되는 ffm에 general 한 특성을 위해 k-fold validation을 적용했다. 또한 여러 가지 모델 간의 ensemble을 통해 편향되지 않고 여러 각도에서 바라보는 효과를 주려고 했다.

### **내가 한 행동의 결과로 어떤 지점을 달성하고, 어떠한 깨달음을 얻었는가**

 여러 가지 아이디어를 떠올렸고 다양한 실험을 했으나 성공률이 저조했다. 결국 단기간에 성과를 내야 하는 상황에서 실험에만 매달리는 것은 좋지 않은 방향이라는 것을 깨달았다.

 lightfm의 score를 rating으로 바꾸려고 시도했으나 잘되지 않았다. 이는 모델 특성에 대한 이해가 부족해서 생긴 문제이다. 모델을 응용하고자 하면 모델 특성과 구조를 제대로 파악하는 게 먼저라는 것을 깨달았다.

### **전과 비교해서, 내가 새롭게 시도한 변화는 무엇이고, 어떤 효과가 있었는가**

팀원들을 설득하기 위해서 실험의 타당성을 정의하려고 노력했다. 스스로 ‘이걸 왜 하는가?’라는 질문을 던졌고 그 효과로 논리적인 전개 과정이 탄탄해졌다.

### **마주한 한계는 무엇이며, 아쉬웠던 점은 무엇인가**

 배경지식의 한계를 느꼈다. 구체적으로는 feautre간 의 importance를 측정해 주는 도구의 존재를 몰랐고, catboost 모델이 범주형 데이터를 잘 처리한다는 사실을 몰랐다.

 또한 실험에 너무 매몰되어 전 과정을 다 경험해 보자는 목표를 잃고 Hyper Parameter Tunnuing을 거의 경험해 보지 못한 점이 아쉽다.

### **한계/교훈을 바탕으로 다음 프로젝트에서 스스로 새롭게 시도해볼 것은 무엇일까**

 AI 엔지니어로서 새로운 문제를 마주했을 때 가장 필요한 능력은 무엇일까? 본인은 첫 번째는 당면한 문제의 특성을 파악하는 능력이라 생각한다. 그리고 두 번째는 각 모델들이 어떤 문제를 해결하고자 발전했는지, 어떤 특성의 데이터를 잘 처리하는지, 어떤 한계점이 있는지 특성들에 대한 체계적인 접근이다. 다음 프로젝트에서는 위 두 가지 능력의 성장을 염두에 두고 임할 것이다. 또한 이번에 활용하지 못한 catboost, Feature Importance에 관련된 tool 그리고 optuna와 tensorboard를 이용한 Hyper Parameter Tunning에 도전해 보고 싶다.

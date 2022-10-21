---
layout: single

  

title: 부스트캠프 AI 5주차(Day-32) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록, git, Fast Forward, Pull request, Branch 전략, gitflow, githubflow, Cherry pick, Branch hell, rebase, revert]

  

toc: true
---

## Day 32

### 이고잉님의 깃허브 특강 part3
#### 복습
+ git은 같은 이름의 브랜치라도 원격 저장소와 지역 저장소를 다른 branch로 취급한다.
+ 작업할때 git pull -> git commit -> git push
  + 특별한 이유가 없다면 항상 push까지 해줘야 팀원들이 편하게 일함.
  + 숨도 쉬지 말고 push까지 하는걸 습관으로 하자.


#### Fast Forward
+ Merge할 main에서 추가적인 작업이 없었다면 branch에 있던 마지막 작업을 main 가르키기만 하면 된다. 이를 fast forward라 한다.
+ 일반적인 merge보다 속도가 빠름
  
#### Pull request
+ 책임자에게 내가 만든 브랜치를 main에 병합 해 주세요, 승인 해 주세요 라는 요청서.
+ 기능설명, 테스트방법, 스펙을 담은 문서로 설명
+ 메인에 브랜치를 병합하기전에 테스트와 코드리뷰에 쓰임
+ Reviewers
  + 리뷰를 할 사람들에게 요청
+ Assigness
  + Merge를 결정할 담당자에게 요청
+ Comments
  + 코드리뷰
+ Pull requests에서 merge충돌이 나면 어떻게 처리할까?
  + Conflict가 발생할 수 있는 상태에서 git push를 하면 PR 페이지에서도 conflict를 감지하고 알려준다.
  + 해결법은 브랜치 끼리 먼저 병합을 시도해 conflict을 handle 한 후 merge를 시킨 뒤 다시 git push를 하자.
  + 그러면 PR 페이지에서 이를 다시 검증해 conflict가 해결된다.
  + conflict가 해결된 후 PR merge를 하자.
    + PR merge는 PR 페이지에서도 가능하고, 터미널에서도 가능하다.
    + 권장되는 방법은 실험 중인 branch와 main을 충돌이 적게 자주 업데이트 시켜 놓자.
  
#### Branch 전략
+ git flow
  + 새로운 기능은 기능이 완료 될 때까지 브랜치를 새로 파서 해당 브랜치에서만 작업하자는 전략
  + master 브랜치는 항상 실행 가능한 상태를 유지하자.

+ github flow
  
<img width="70%" src="https://user-images.githubusercontent.com/94548914/196868475-4d3b5b4f-312d-40c7-ac10-e7cff2b73b38.png">

  + master
    + 제품으로 출시될 수 있는 브랜치
  + develop
    + 다음 출시 버전을 개발하는 브랜치
  + featrue braches
    + 기능을 개발하는 브랜치
  + release braches
    + 출시를 위해 필요한 작업만하고 다른 일은 안함.
    + 출시를 위한 braches에서 출시 버전외의 다른 기능을 개발하면 출시할 때 해당 버전 외의 다른 기능 부분이 작업 중이면 출시를 못하게됨.
  + hotfixes
    + 출시 버전에서 발생한 버그를 수정 하는 브랜치
  + tag
    + 커밋에 이름을 붙인다.
    + Brache와 HEAD는 매우 동적이라 정적인 reference가 필요.
    + 3대 reference중 하나

#### Cherry pick
+ 3 way merge를 활용한 획기적인 방법
  
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/94548914/197166035-21604574-f51e-4209-8a61-9d97e4ddac3d.png">

#### Branch hell → rebase로 해결하자
+ merge: 진실이지만 복잡하다.

<img width="90%" alt="merge" src="https://user-images.githubusercontent.com/94548914/197172149-e2f19de1-e4ee-4021-92e9-87a443d3e114.png">



+ rebase: 단순하지만 엄밀히 말하면 거짓말이다.
  + Cherry pick을 통해 브랜치를 flatten 시킨 후 base가 하나였던거 처럼 된다.

<img width="90%" alt="step2" src="https://user-images.githubusercontent.com/94548914/197170947-86bed9c0-25ab-4f45-a3b0-b0c83d405396.png">

<img width="90%" alt="step1" src="https://user-images.githubusercontent.com/94548914/197170957-bbf4e05d-e803-48fa-8553-7d3230d4e9a4.png">

<img width="90%" alt="step3" src="https://user-images.githubusercontent.com/94548914/197171365-f7f732f6-1008-4737-be2b-7cff22a32042.png">



#### Revert
+ 깃의 특성상 과거에 잘못된 커밋을 직접 수정할 수 없음.
  + 잘못된 부분을 수정한 후 새로 커밋을 한다는 개념으로 수정한다.
+ 이미 배포한 걸 취소
+ 3 way merge

<img width="90%" alt="revert" src="https://user-images.githubusercontent.com/94548914/197174368-806c07b5-e51e-4cc9-8f48-d6a377ed24b5.png">


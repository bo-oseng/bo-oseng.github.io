---

layout: single

  

title: 부스트캠프 AI 5주차(Day-30) 회고록

categories:

- Boostcamp

  

tag: [Boostcamp, 부스트캠프, 회고록]

  

toc: true

  

---

## Day 30

### 이고잉님의 깃허브 특강 part2

+ Tracked와 Untracked
  + git add를 한번이라도 한 파일은 tracked 상태
  + git commit -a 옵션은 tracked 상태의 파일만 커밋에 포함시킨다.

+ Add의 의미
  + 첫 번째 의미는 commit 대기 상태를 만드는 것
  + 두 번째 의미는 untracked를 tracked로 만드는 것이다
  + 세 번째는 충돌을 해결했다는 것을 git에게 알림

+ .gitignore

+ Head가 가르키는 부분이 Parent
+ Checkout은 head를 옮기고 reset은 head가 가르키는 브랜치로 옮긴다.
+ Reset은 삭제면서 복원이다.
  + Git reset –head commit id
  + If(attached) { HEAD의 Branch가 움직임} else { ==checkout}
+ Git log –oneline –all –graph
  + Git config –global alias
+ Git merge
  + Git checkout “A”
  + Git merge “B” -> merge b to a
  + Confilict
  + Master가 exp를 병합한다 -> 실험에 성공했다
  + Exp가 Master를 병합한다 -> exp에 업데이트를 했다.
  + Conflict ( git: 이건 사람 불러야겠는데요?)
    + Accept Incomming
    + Accept HEAD
    + Accept both
  + 3-way merge
    + Base를 기준으로 두고 합치려는 branch들을 비교 해가며 결정한다.
    + 이 때 결정하기 애매한 부분은 conflict가 난다.
+ ammend
+ Git remote
  + Origin/maim과 main의 위치가 다르면 버전관리
+ Git push
  + Rejected
  + Fetch
  + merge

## 의문점
+ 작업을 많이 하다 보면 commit id 중 안 쓰는 것들이 점점 쌓일 것 같은데, 이 부분들은 git이 알아서 관리하는 건가요?

## 피어섹션

- 작업을 많이 하다 보면 commit id 중 안 쓰는 것들이 점점 쌓일 것 같은데, 이 부분들은 git이 알아서 관리하는 건가요? → **[Garbage collection](https://git-scm.com/docs/git-gc)가 존재(git gc)**
- 커밋을 수정할 수 있는가?

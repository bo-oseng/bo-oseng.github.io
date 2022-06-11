---
layout: single

title: User register, VanillaJS - (1)
categories:
  - VanillaJS

tag: [javascript, root, css, html]

toc: true
---

# User register with vaild judgement

+ 간단한 유효성 검사가 포함된 유저 등록 창입니다.
+ <a href='https://codepen.io/kim7720/pen/LYQNbRd' 
   target='_blank'>Live demo</a>
   
 ## 배운점
 1. css에서도 \:root 가상선택자를 이용해서지역변수나 전역변수 개념을 활용할 수 있다는 걸 학습 했습니다.  


```css
  :root {
      --success-color:#2ecc71;
      --error-color:#e74c3c;
  }

```  
  
```css
.form-control.success input{
    border-color: var(--success-color);
}

.form-control.error input{
    border-color: var(--error-color);
}
```
부트스트랩이나 bulma 같은 유명한 프레임워크들이 어떤 방법으로 동작하는지 궁금했었는데 어렴풋이 알게 되었습니다.

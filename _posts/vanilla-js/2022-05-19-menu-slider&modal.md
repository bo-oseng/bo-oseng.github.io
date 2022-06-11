---
layout: single

title: Menu Slider & Modal, VanillaJS - (6)
categories:
  - VanillaJS

tag: [javascript, css, html]

toc: true

---
# Menu Slider & Modal

- vanila JS로 모달창과 메뉴슬라이더 만들기 예제
- <a href='https://codepen.io/kim7720/pen/jOZwgZO'> Live demo </a>
- <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Menu%20Slider%20%26%20Modal'> Source </a>

<iframe height="300" style="width: 100%;" scrolling="no" title="menu slider and modal" src="https://codepen.io/kim7720/embed/jOZwgZO?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/jOZwgZO">
  menu slider and modal</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>



## 배운점

1. 메뉴 슬라이더을 접고 펼치는 기능을 단순하게 vanila js와 css로 구현하는 법을 배웠습니다.
   - 웹에서 자주 볼 수 있는 메뉴 슬라이더를 구현할때 단순히 navigation부분을 바디에 왼쪽에 위치시킨뒤, body태그의 위치를 transform으로 오른쪽과 왼쪽으로 미는 식으로 구현하는 요령을 알게 되었습니다.

```css
body.show-nav {
  /* Width of nav */
  transform: translateX(var(--nav-bar-width));
}

nav {
  background-color: var(--primary-color);
  border-right: 2px solid rgba(200, 200, 200, 0.1);
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  width: 200px;
  height: 100vh;
  z-index: 100;
  transform: translateX(-100%);
}
```

```javascript
// Toggle nav
toggleBtn.addEventListener('click', () => {
  document.body.classList.toggle('show-nav');
});
```

2. 모달창을 구현할 때 모달의 밖 부분 클릭시 모달창이 사라지는 기능을 구현 해봤습니다.
   - 모달의 내용을 표시하는 부분을 자식태그(.modal-header, .modal-content, ), 모달의 배경을 부모 태그(.modal)를 두어 event.target과 modal이 일치할때만 종료 하게끔 구현하는 법을 학습했습니다.

```html
<div class="modal-content">
  <p>Register with us to get offers, support and more</p>
  <form class="modal-form">
    <div>
      <label for="name">Name</label>
      <input
        type="text"
        id="name"
        class="form-input"
        placeholder="Enter Name"
      />
    </div>
    <div>
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        class="form-input"
        placeholder="Enter Email"
      />
    </div>
    <div>
      <label for="password">Password</label>
      <input
        type="password"
        id="password"
        class="form-input"
        placeholder="Enter Password"
      />
    </div>
    <div>
      <label for="password2">Confirm Password</label>
      <input
        type="password"
        id="password2"
        class="form-input"
        placeholder="Confirm Password"
      />
    </div>
    <input type="submit" value="Submit" class="submit-btn" />
  </form>
</div>
```

```javascript
// Hide modal on outside click
window.addEventListener('click', (e) => {
  e.target == modal ? modal.classList.remove('show-modal') : false;
});
```

---
layout: single

title: Movie seat booking, VanillaJS - (2)
categories:
  - VanilaJS

tag: [javascript, html, css, localStorage]

toc: true

---

# Movie seat booking
+ 영화예매 예제
+ <a href='https://codepen.io/kim7720/pen/OJQNKwE'>Live demo </a>
+ <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Movie%20seat%20booking'>Source </a>

<iframe height="300" style="width: 100%;" scrolling="no" title="Movie seat Booking" src="https://codepen.io/kim7720/embed/OJQNKwE?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/OJQNKwE">
  Movie seat Booking</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>


### 배운점
1. 영화 좌석을 [6개의 row 클래스를 갖는 div]  *  [8개의 seat 클래스를 갖는 div] 이런식으로 html div태그를 배열의 형태로 이용할 수 있게끔 접근하는 방식을 배웠습니다. ( 다양한 상황에서 이런식으로 배열과 비슷하게 활용할수 있을 것 같습니다.)  
   
2. 자바스크립트에서 html 태그를 클래스로 선택 할 때 헷갈리던 부분을 확실하게 정리할 수 있었습니다.

```html
  // index.html
        <div class="row">
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
            <div class="seat"></div>
        </div>
```
```javascript
  // main.js
  // 처음 작성했던 코드 
const updateSelectCount = (() => {
    const selectedSeats = document.querySelectorAll('.row.seat.selected');

````

```javascript
  // main.js
  // 수정한 코드
const updateSelectCount = (() => {
    const selectedSeats = document.querySelectorAll('.row .seat.selected');

````
  +  처음 코드를 작성할때는 클래스 선택을 ```.row.seat.selected``` 로 작성했었는데, 이는 class 속성에 'row seat selected' 를 모두 갖는 태그를 의미하므로 위의 코드에서는 ```.row > .seat.selected'```나 ```.row .seat.selected```로 수정이 필요했다.  
  
```javascript
const seats = document.querySelectorAll('.row .seat:not(.occupied)');
```
  + not 가상클래스를 어떤식으로 활용할지 감이 안잡혔는데 적절히 활용하는 법을 배웠습니다.    
3. localstorage의 JSON.parse와 JSON.stringify를 언제 쓰는지 어렴풋이만 알고 있었는데, 이번 기회에 조금 더 명확하게 알게 되었습니다.
4. 기본 웹의 모양을 없애주는 css 요소를 새로 학습했습니다.  
```css
    -moz-appearance: none;
    -webkit-appearance: none;
    appearance: none;
}
```

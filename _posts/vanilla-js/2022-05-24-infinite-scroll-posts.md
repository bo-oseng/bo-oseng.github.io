---
layout: single

title: Infinite Scroll Posts, VanillaJS - (7)
categories:
  - VanillaJS

tag: [javascript, Query Sting, async, await, scrollEvent, destructuring]

toc: true

---
# Infinite Scroll Posts

- 무한 스크롤 예제
- <a href='https://codepen.io/kim7720/pen/vYdpYjB'>Live demo</a>
- <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Infinite%20Scroll%20Posts'>Live demo</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="Infinite Scroll Posts" src="https://codepen.io/kim7720/embed/vYdpYjB?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/vYdpYjB">
  Infinite Scroll Posts</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

### 주요 개념정리

1. Query String(Query Parameter) 정리.  
    query string은 URL의 일부로서 파라미터로 데이터를 정제하는 역할을 할 수있습니다.

   - query string없이 fetch을 요청했을때
      <center>
          <img src='https://user-images.githubusercontent.com/94548914/170534748-4188fe4b-fd75-40ea-806d-5b9a67663efe.jpg'>
      </center>
     100개의 객체를 모두 불러오는걸 알 수 있습니다.
   - query string으로 limt를 5개로 제한 했을 때
      <center>
          <img src='https://user-images.githubusercontent.com/94548914/170535055-ba462a48-58d3-406e-a468-9b6eb1a0dd75.jpg'>
      </center>
      5개의 객체를 불러옵니다.
   - query string으로 page를 3, limt를 5개로 제한 했을 때
     <center>
         <img src='https://user-images.githubusercontent.com/94548914/170535729-53e0fac5-bbf6-42c4-8407-6f58771668c3.JPG'>
     </center>
     페이지 3에서 5개의 객체를 불러옵니다.
   일종의 규약으로 상세한 파라미터의 종류는 데이터 구조를 설계할 때 정해집니다.

2. async await 키워드
   - 기존의 콜백헬의 문제를 개선하기위해 ES6에서 도입한 개념이 Promise, 그리고 비동기적 함수인 Promise를 동기적함수 모양으로 작성할수 있게 ES7에서 도입된 syntactic sugar를 제공하는 키워드이다.
   - arrow function으로 async 함수를 선언하려면
     ```javascript
     const doSomething = async () => {};
     ```
     과 같이 가능하다.

## 배운점

1.  애니메이션 시간차이두기

    - 웹 페이지에서 자주 보이는 형태의 로딩 애니메이션을 구현하는 요령을 배웠다.
    - 세개의 div를 border-radis로 원을 만든 뒤 세개의 원을 시간차를 두고위 아래로 움직이는 형태로 구현했다.

      ```css
      @keyframes bounce {
        0%,
        100% {
          transform: translateY(0);
        }

        50% {
          transform: translateY(-10px);
        }
      }

      .circle {
        background-color: #fff;
        width: 10px;
        height: 10px;
        border-radius: 50%;
        margin: 5px;
        animation: bounce 0.5s ease-in infinite;
      }
      ```

2.  스크롤 이벤트 구현(전체 높이 도달 구현)

    - window 객체의 scroll event를 listen 할 수있다.
    - document.documentElement 객체의 scrollTop, scrollHeight, clientHeight를 이용하면 스크롤이 화면끝에 도달했는지 계산할 수있다.
    - scrollTop: 스크롤 되어 올라간 만큼의 높이를 픽셀로 반환한다.(참고: [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollTop))
    - scrollHeight: 스크롤 시키지 않았을 때의 전체 높이를 픽셀로 반환한다.(참고: [MDN](https://developer.mozilla.org/ko/docs/Web/API/Element/scrollHeight))
    - clientHeight: 눈에 보이는 만큼의 높이를 픽셀로 반환한다.(참고: [MDN](https://developer.mozilla.org/ko/docs/Web/API/Element/clientHeight))

      ```javascript
      // Fetch new posts

      window.addEventListener('scroll', () => {
        const { scrollTop, scrollHeight, clientHeight } =
          document.documentElement;
        // 스크롤된 전체높이 + 유저가 보고 있는 요소의 높이 >= 전체높이 - 5
        // 즉 전체 높이보다 더 아래로 스크롤 했을때를 말한다.
        if (scrollTop + clientHeight >= scrollHeight - 5) {
          showLoading();
        }
      });
      ```

3.  객체 디스트럭처링
    - 기존에는 배열을 인덱스 순으로 쪼개는 배열 디스트럭처링만 알고 있었는데, 객체 또한 키이름으로 바로 값을 할당하는게 가능하다는 걸 배웠다.
      ```javascript
      const { scrollTop, scrollHeight, clientHeight } =
        document.documentElement;
      ```
4.  Javasscript에서 display 속성 제어

    - 자바스크립트에서 CSSOM 제어도 가능하다는 것은 알고는 있었는데, 이 예제를 통해 직접 display의 속성을 바꾸어 보았다.

      ```javascript
      // Filter posts by input
      function filterPosts(event) {
        const term = event.target.value.toUpperCase();
        const posts = document.querySelectorAll('.post');

        posts.forEach((post) => {
          const title = post
            .querySelector('.post-title')
            .innerText.toUpperCase();
          const body = post.querySelector('.post-body').innerText.toUpperCase();

          if (title.indexOf(term) > -1 || body.indexOf(term) > -1) {
            post.style.display = 'flex';
          } else {
            post.style.display = 'none';
          }
        });
      }
      ```

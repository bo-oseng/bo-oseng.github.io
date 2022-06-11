---
layout: single

title: Sortable List, VanilaJS - (8)
categories:
  - VanilaJS

tag: [javascript, data attribute, closet, drag and drop, this]

toc: true

---

# Sortable List
- Drag and Drop api를 활용한 정렬가능 리스트 예제 입니다.
- <a href='https://codepen.io/kim7720/pen/QWQmNWr'>Live demo</a>
- <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Sortable%20List'>Source</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="Sortable list" src="https://codepen.io/kim7720/embed/QWQmNWr?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/QWQmNWr">
  Sortable list</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 배운점

1.  HTML data 속성.

    - HTML5 부터 생긴 개념으로 'data-' 로 시작하는 속성이고, 특정한 데이터를 DOM 요소에 저장하거나 수정할 수 있습니다.
    - HTML에서는 'data-code-name' 같이, JS에서는 'dataset.codeName' 와 같이 카멜케이스로 접근합니다.
    - getAttribute()로도 접근이 가능합니다.
    - CSS에서 속성 선택자로 접근도 가능합니다.

    ```javascript
    // Insert list items into Dom
    function createList() {
      [...richestPeople]
        .map((a) => ({ value: a, order: Math.random() }))
        .sort((a, b) => a.order - b.order)
        .map((a) => a.value)
        .forEach((person, index) => {
          const listItem = document.createElement('li');

          listItem.setAttribute('data-index', index);

          listItem.innerHTML = `
            <span class='number'>${index + 1}</span>
            <div class='draggable' draggable='true'>
              <p class='person-name'>${person}</p>
              <i class='fas fa-grip-lines'></i>
            </div>
            `;

          listItems.push(listItem);

          draggableList.appendChild(listItem);
        });

      addEventListeners();
    }
    ```

2.  closet api

    - Element.closet() 는 ()안에 주어진 CSS 선택자와 일치하는 요소를 찾을 때까지, 자기 자신을 포함해 부모 방향으로 탐색하여 일치하는 요소를 반환합니다.

```javascript
function dragStart(e) {
  dragStartIndex = +this.closest('li').getAttribute('data-index');
}
```

3.  drag and drop API.
    <table>
    <thead>
        <tr>
        <th>이벤트</th>
        <th>설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td>drag</td>
        <td>드래그할 때 계속 발생(드래그 하는 동안 계속 발생)</td>
        </tr>
        <tr>
        <td>dragstart</td>
        <td>드래그가 시작되는 순간만 발생</td>
        </tr>
        <tr>
        <td>dragend</td>
        <td>드래그가 끝날때 발생(마우스 버튼을 놓거나 ESC를 누를 때)</td>
        </tr>
        <tr>
        <td>dragenter</td>
        <td>드래그한 아이템이 해당 이벤트를 지정한 요소에 들어가면 발생</td>
        </tr>
        <tr>
        <td>dragleave</td>
        <td>
            드래그로 요소가 빠져나가면 발생 (높이 너비 반 이상 나갈 때)
        </td>
        </tr>
        <tr>
        <td>dragover</td>
        <td>드래그로 해당 이벤트를 지정한 요소 위를 지날 때 발생</td>
        </tr>
        <tr>
        <td>drop</td>
        <td>
            드래그로 해당 이벤트를 지정한 요소 위에서 드래그가 끝났을 때 발생
        </td>
        </tr>
    </tbody>
    </table>

    ```javascript
    // Add eventListners
    function addEventListeners() {
      const draggables = document.querySelectorAll('.draggable');
      const dragListItems = document.querySelectorAll('.draggable-list li');

      draggables.forEach((draggable) => {
        draggable.addEventListener('dragstart', dragStart);
      });

      dragListItems.forEach((item) => {
        item.addEventListener('dragover', dragOver);
        item.addEventListener('drop', dragDrop);
        item.addEventListener('dragenter', dragEnter);
        item.addEventListener('dragleave', dragLeave);
      });
    }
    ```

4.  Event Listenr에서 arrow function의 사용

    - 기존의 자바스크립트는 호출 방식에 따라 동적으로 this가 결정되는 특이한 규칙이 있다.
    - ES6 에서 도입된 arrow function에는 이런 규칙을 개선한 Syntactic sugar가 있다. 함수를 호출할 때가 아닌 선언할때 상위 스코프의 this를 가리킨다.(Lexical this)
    - 하지만 이런 Syntactic sugar가 문제를 일으키는 경우가 몇가지 있는데 그중 하나가 Event Listner의 콜백 함수이다.

    ```javascript
    const button = document.getElementById('btn');

    button.addEventListener('click', () => {
      console.log(this === window); // => true
      this.innerHTML = 'Clicked'; // => bug
    });
    ```

    다음과 같이 button이 아닌 window에 this가 바인딩 된다.

    이를 회피하기 위해선 function 키워드를 사용해 함수를 선언해야한다.

    ```javascript
    const button = document.getElementById('btn');

    button.addEventListener('click', fuction() {
      console.log(this === window); // => False
      this.innerHTML = 'Clicked';
    });
    ```

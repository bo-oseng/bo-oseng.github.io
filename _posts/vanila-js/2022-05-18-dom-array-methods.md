---
layout: single

title: DOM Array Methods, VanilaJS - (5)
categories:
  - VanilaJS

tag: [javascript, await, async, reduce, spread]

toc: true

---
# DOM Array Methods

- Array 고차 함수를 활용한 간단한 재산 계산 어플리케이션
- <a href='https://codepen.io/kim7720/pen/abqwwRN'>Live demo</a>
- <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/DOM%20Array%20Methods'>Source</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="DOM Array Methods" src="https://codepen.io/kim7720/embed/abqwwRN?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/abqwwRN">
  DOM Array Methods</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>
  

## 배운점

1. 비동기 함수를 다룰 때 유용한 async키워드와 await 키워드를 이론적으로는 알고 있었지만, 이번 예제를 통해 활용하는 법을 학습 했습니다.
   - async와 await 키워드는 비동기 함수를 동기함수처럼 선언할 수 있게 해주는 일종의 syntatic sugar이다.

```javascript
// Fetch random user and add money
const getRandomUser = async () => {
  const res = await fetch('https://randomuser.me/api');
  const data = await res.json();

  const user = data.results[0];

  const newUser = {
    name: `${user.name.first} ${user.name.last}`,
    money: Math.floor(Math.random() * 1000000),
  };
  addData(newUser);
};
```

2. Spread 를 활용해 축약적으로 특정 프로퍼티를 변경하는 법을 배웠습니다.
   - Spread로 파이썬의 딕셔너리 컴프리헨션처럼  
     `const changed = { ...{ x: 1, y: 2 }, y: 100 };` 와 같이 y프로퍼티를 변경하는 법을 학습했습니다.

```javascript
// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console.log(changed); // { x: 1, y: 100 }
```

```javascript
// Double eveveryones money
const doubleMoney = () => {
  console.log('1');
  userData = userData.map((user) => {
    return { ...user, money: user.money * 2 };
  });

  updateDOM();
};
```

3. reduce 메소드가 헷갈렸는데, 이번 예제를 통해 적절히 활용하는 법을 학습 했습니다.
   - arr.reduce(callback[, initialValue]) 에서 initialValue로 reduce를 시작하기 전 초기값을 설정 할 수 있고, callback 함수의 인자로는 (state: U, element: T, index: number, array: T[]) state는 누적값, element는 처리할 현재값, index는 처리할 현재값의 인덱스, array는 reduce를 호출한 배열을 나타낸다.

```javascript
// Calculate Wealth
const calculateWealth = () => {
  const totalWealth = userData.reduce((acc, user) => (acc += user.money), 0);

  const wealthEl = document.createElement('div');
  wealthEl.innerHTML = `<h3>Total Wealth: <strong>${formatMoney(
    totalWealth
  )}</strong></h3>`;
  main.appendChild(wealthEl);
};
```

4. 데이터를 정제하고, 조작하는 부분과 웹페이지에 조작한 데이터를 업데이트 하는 부분을 나눠 더 가독성이 좋게 코드를 짜는법을 학습했습니다.

```javascript
// Update DOM 웹페에지에 업데이트를 담당하는 함수
const updateDOM = (providedData = userData) => {
  // Clear main div
  main.innerHTML = '<h2><strong>Person</strong> Wealth</h2>';

  providedData.forEach((item) => {
    const element = document.createElement('div');
    element.classList.add('person');
    element.innerHTML = `<strong>${item.name}</strong>${formatMoney(
      item.money
    )}`;
    main.appendChild(element);
  });
};
```

```javascript
// Add new obj to data arr
const addData = (obj) => {
  userData.push(obj);
  updateDOM();
};

// Sort by richest
const sortByRichest = () => {
  userData.sort((userA, userB) => userB.money - userA.money);

  updateDOM();
};
```

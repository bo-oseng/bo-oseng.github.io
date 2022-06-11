---
layout: single

title: Exchange Rate Calculatorr, VanillaJS - (4)
categories:
  - VanillaJS

tag: [javascript, API]

toc: true

---

# Exchange Rate Calculator

- API를 활용한 환전시스템 예제
- <a href = 'https://codepen.io/kim7720/pen/bGLWGxZ'>Live demo</a>
- <a href = 'https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Exchange%20Rate%20Calculator'>Source</a>
<iframe height="300" style="width: 100%;" scrolling="no" title="Exchange Rate Calculator" src="https://codepen.io/kim7720/embed/bGLWGxZ?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/bGLWGxZ">
  Exchange Rate Calculator</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 배운점

1. fetch() 함수로 비동기적으로 API를 호출하고 다루는 법을 배웠습니다.

```javascript
const calculate = () => {
  const currency_one = currencyEl_one.value;
  const currency_two = currencyEl_two.value;

  let myHeaders = new Headers();
  myHeaders.append('apikey', 'pm6pDh9DAXyiCVOuIesoQJLQZwQ6NjBD');

  let requestOptions = {
    method: 'GET',
    redirect: 'follow',
    headers: myHeaders,
  };

  fetch(
    `https://api.apilayer.com/exchangerates_data/latest?symbols=${currency_two}&base=${currency_one}`,
    requestOptions
  )
    .then((res) => res.json())
    .then((data) => {
      console.log(data);
      const rate = data.rates[currency_two];

      rateEl.innerText = `1 ${currency_one} = ${rate} ${currency_two}`;

      amountEl_two.value = (amountEl_one.value * rate).toFixed(2);
    });
};
```

- 기존에 promise나 fetch 함수에 대해 이론적으로 알고 있었지만, 막상 실제로 써본적이 없었는데, 이 예제를 통해 Headers객체와 apikey로 API에 access 권한을 다루는법에 대해 학습했습니다.
- <a href='https://apilayer.com/marketplace/description/exchangerates_data-api?preview=true#details-tab'>API Info</a>

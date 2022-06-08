---
layout: single

title: API authentication by API key, API key로 API 인증하기

category:
  - HTTP

tag: [authentication, authorization, headers, Web]

toc: true

---

# API key access

### 배경

최근 Vanila Javascript 예제를 몇개 만들어 보고 있는데, <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Exchange%20Rate%20Calculator'>Exachange Rate Calculator</a> 에서 API를 다룬적이 있습니다. API를 써본적은 있었지만 API로의 접근에서 권한을 필요로 하는 API는 처음 만나게 되었습니다. <a href='https://apilayer.com/marketplace/description/exchangerates_data-api?preview=true#documentation-tab'>API를 제공하는 사이트의 스펙</a>에서 사용법과 API key를 친절히 제공해 주어 쉽게 다룰 수 있었지만 어떤 원리로 권한을 인가받고 API key은 어떻게 생성되는지 또 이 과정에서 쓰이는 Header의 정체 무엇인지 궁금해 추가적으로 조사하고 정리한 글 입니다.

### 사전지식 정리

1. Authentication(인증)과 Authorization(인가)의 차이

   Authentication(인증): 접근하는 유저의 신원을 확인하고 통과 시킬지 확인하는 절차.

   Authorization(인가): 접근하는 유저가 무엇을 할 수 있는지 권한을 허락하는 것.

   서비스를 누가 언제 어떻게 얼마나 쓰고 있는지를 추적하기 위해 반드시 필요하다.

2. HTTP headers

   HTTP 헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.

   소프트웨어 정보, 쿠키, 문자포멧등을 담고 있다.

3. Headers 객체

   fetch 함수에서 api를 요청할때 header를 다룰수 있게 하는 인터페이스 이다.
   (참고: <a href='https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers'>MDN</a>)

### API authentication(인증)의 대표적인 방법들

- HTTP Basic Authentication
- OAuth 2.0 (Token in HTTP Header)
- API Keys

  API 인증에 쓰이는 방법은 대표적으로 위의 세가지가 있고, 이 중 API key방식에 대해 알아 보겠습니다.

## API key

<a href='https://cloud.google.com/endpoints/docs/openapi/when-why-api-key?hl=ko#:~:text=API%20%ED%82%A4%EB%8A%94%20%EC%9D%B8%EC%A6%9D%20%ED%86%A0%ED%81%B0,%EC%9D%84%20%EC%A0%9C%ED%95%9C%ED%95%A0%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.'>구글 클라우드 문서</a>를 참고했습니다.

API key는 구글 클라우드 문서에서 제공한 사진을 보고 쉽게 이해 할 수 있었습니다.

<center>
  <img src='https://cloud.google.com/endpoints/docs/images/api_keys_overview.png?hl=ko' width='50%'>
</center>

사진에서 알 수 있듯 사용가 API를 요청할때 API 서비스 제공자에게 발급받은 API key를 헤더에 함께 보내면 제공자가 사용자를 인지하고 유효한 키라면 사용을 허가합니다.

API key를 이용하는 방식은 다음과 같은 특징이 있습니다.

- API key를 발급받은 유저의 API 사용량을 체크 할 수 있습니다.
- API를 호출하는 애플리케이션이나 프로젝트를 식별합니다. 호출을 특정 IP 주소 범위, Android 앱, iOS 앱과 같은 환경에서의 키 사용을 제한할 수 있습니다.
- 프로젝트를 식별할 뿐사용자를 구별하여 식별하지는 않습니다.

이런 특징들 때문에 API 사용량에 따른 요금부과가 가능하고, DDOS 공격으로 부터 서비스를 보호할 수 있습니다.  
그래서 사용량으로 사용자에게 요금을 책정하는 API 제공 서비스들이 많이 채택하고 있는 방식 입니다.

## API key 사용법

fetch()함수에서 API key를 사용하는 방법은 다음과 같습니다.

1. 서비스 제공자로부터 API key를 발급 받는다.
2. fetch() 함수의 두번째 파라미터 내부의 header 객체에 apikey의 정보가 포함시켜 호출한다.

다음 예시를 보면 쉽게 알 수 있습니다.

```javascript
const calculate = () => {
  const currency_one = currencyEl_one.value;
  const currency_two = currencyEl_two.value;

  // 새로운 Headers객체 선언
  let myHeaders = new Headers();
  // Headers 객체에 apikey프로퍼티와 서비스 제공자로부터 발급 받은 키값을 넣어줌
  myHeaders.append("apikey", "pm6pDh9DAXyiCVOuIesoQJLQZwQ6****");

  // fetch의 요청 방식과 헤더를 포함한 requestOptions 객체선언
  let requestOptions = {
    method: "GET",
    headers: myHeaders,
  };

  // fetch함수의 첫번째 파라미터로 API의 주소, 두번쨰 파라미터로 요청에 대한 추가 데이터를 전달해줌
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

#### 참고

> 잘못된 정보가 포함되어 있을 수 있습니다.
>
> 다음에는 카카오 로그인이나 구글 로그인에 활용 되는 OAuth에 대해 알아보겠 습니다.

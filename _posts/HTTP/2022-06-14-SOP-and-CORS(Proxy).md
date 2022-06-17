---
layout: single

title: SOP와 CORS, Proxy의 활용

category:
  - HTTP

tag: [Web, SOP, CORS, Proxy]

toc: true

---

# Proxy를 활용한 Cross Origin Resource Sharing

>배경  
 이전에 [Lyrics Search](https://bo-oseng.github.io/vanillajs/lyrics-search(CORS)/)라는 예제를 만들어보면서 SOP와 CORS 그리고 Proxy까지 활용해본 바 있다. 이번 글은 이 내용들을 나중에 다시 보기위해 보충 설명과 함께 정리 했다.  


## Same Origin Policy  

### SOP
- 보안상의 이유로, Origin이 모두 같은 Origin외에 다른 모든 [Origin](#origin)의 접근을 막는 정책. 잠재적으로 문제가 될만한 공격으로부터 보호하기위해 Origin을 고립화 하기 위한 메커니즘.  
### Origin
- Scheme(Protocol) + Hostname(Domain) + Port를 모두 합친 [URL](#urluniform-resource-locator).(세가지 중 하나라도 다르다면 다른 Origin이다.)
### URL(Uniform Resource Locator)
- 문자열로 이루어진 리소스의 네트워크상 주소지를 의미한다.

## Cross Origin Resource Sharing
### CORS
```
1. Frontend에서 headers 객체에 orign을 담아 Backend에 접근을 요청함
2. Bakend에서 지정된 URL이 담긴 Access-Control-Allow-Origin를 답장의 headers에 실어서 보냄
3. 이후 둘을 브라우저에서 비교한뒤, 접근 허가 여부를 판단함.
```
 - 다른 Origin의 접근을 막는 매커니즘인 SOP을 유동적으로 우회하기 위한 방법으로서 frontend에서 HTTP headers에 origin을 담아 backend로 전송하면 backend에서 headers에 담긴 origin 정보로 접근을 허가할지 말지 판단하는 방법이다. backend에서 허가할 orign을 미리 명시하는 식으로(whitelist) 설계할 수 있다.  
### Simple request vs Preflight request
 - Simple request는 서버에 영향을 주지 않는 요청. Preflight request는 서버에 영향을 줄 수도 있는 요청이다. 때문에 Preflight는 요청을 실행하기전 headers만을 전송받아 CORS을 확인하고 허용된다면 다시 동작을 요청받는 식으로 설계되었다.  


## Proxy
- 대리, 대신이라는 의미이다. 서버와 클라이언트가 통신을 할때 보안이나 캐싱을 위해 서버와 클라이언트가 직접 통신하지 않고 중계를 거쳐 통신하는데 이때 중계기 역할을 하는게 프록시이다.
- Forward Proxy
    - 클라이언트가 인터넷에 직접 접근하는게 아니라 프록시에 접근해 프록시가 요청을 받고 프록시가 인터넷을 통해 서버에 요청을 전달(forward)해준다. 캐싱이 가능해 성능 향상이 기대되며 특정 사이트에 접근을 제한할 수도 있다.
- Reverse Proxy
    - 클라이언트가 인터넷으로 서버에 요청을 하면 리버스 프록시가 이 요청을 받아 내부 서버에 데이터를 받은후 클라이언트에 전달 해준다. 사용자가 직접 내부 서버에 접근을 하는걸 제한 하므로 보안이 중요한 곳에서 쓰인다.  

---

## 프록시로 CORS 해결하기
[CORS-anywhere](https://github.com/Rob--W/cors-anywhere) fork해 [heroak](https://dashboard.heroku.com/)에 배포하면 간단하게 내가 관리할 수 있는 프록시를 만들 수 있다.



CORS-anywhere의 소스코드를 보면 친절하게 주석이 달려있다. 주석을 보고 간단한 코드를 바꿔보자.
```js
// Listen on a specific host via the HOST environment variable
var host = process.env.HOST || '0.0.0.0';
// Listen on a specific port via the PORT environment variable
var port = process.env.PORT || 8080;

// Grab the blacklist from the command-line so that we can update the blacklist without deploying
// again. CORS Anywhere is open by design, and this blacklist is not used, except for countering
// immediate abuse (e.g. denial of service). If you want to block all origins except for some,
//  |
//  |
//  |
//  | ===============================================================
//  |------> whitelits를 제외한 모든 사이트를 블락할때 쓸 수 있다. 여기를 수정해보자
//    ===============================================================

// use originWhitelist instead. 


var originBlacklist = parseEnvList(process.env.CORSANYWHERE_BLACKLIST);
var originWhitelist = parseEnvList(process.env.CORSANYWHERE_WHITELIST);
function parseEnvList(env) {
  if (!env) {
    return [];
  }
  return env.split(',');
}

// Set up rate-limiting to avoid abuse of the public CORS Anywhere server.
var checkRateLimit = require('./lib/rate-limit')(process.env.CORSANYWHERE_RATELIMIT);

var cors_proxy = require('./lib/cors-anywhere');
cors_proxy.createServer({
  originBlacklist: originBlacklist,
  originWhitelist: originWhitelist,
  requireHeader: ['origin', 'x-requested-with'],
  checkRateLimit: checkRateLimit,
  removeHeaders: [
    'cookie',
    'cookie2',
    // Strip Heroku-specific headers
    'x-request-start',
    'x-request-id',
    'via',
    'connect-time',
    'total-route-time',
    // Other Heroku added debug headers
    // 'x-forwarded-for',
    // 'x-forwarded-proto',
    // 'x-forwarded-port',
  ],
  redirectSameOrigin: true,
  httpProxyOptions: {
    // Do not add X-Forwarded-For, etc. headers, because Heroku already adds it.
    xfwd: false,
  },
}).listen(port, host, function() {
  console.log('Running CORS Anywhere on ' + host + ':' + port);
});
```  
---

처음 소스코드의 default는 모든 orgin에 대해 접근이 가능하게 되어있다.  
whitelist에 "https://www.example.com"을 등록 해보자.

<center>
  <img src="https://user-images.githubusercontent.com/94548914/173604629-5f4e117b-408b-4131-9e43-1443b406c8dd.jpg" height="50%">

  프록시를 활용한 예제가 잘 동작중인 모습(one으로 시작하는 노래를 외부 API로 검색하는예제)
</center>

<center>
  <img src="https://user-images.githubusercontent.com/94548914/173604615-da3f35d4-7228-40b8-a8be-0c92a9fc1aa7.JPG" >

  heroak에서 cors-anywhere 코드를 배포중인 나의 프록시 앱 설정에 들어간다.
</center>



<center>
  <img src="https://user-images.githubusercontent.com/94548914/173604627-e5be8819-1b5f-4733-a7af-98f9063b8a93.JPG">

  Confing Vars항목에서 <br>KEY: 'CORSANYWHERE_WHITELIST'<br> VALUE: 'https://www.example.com'<br>를 추가하면 VALUE의 사이트가 화이트 리스트로 등록된다.
</center>

<center>
  <img src="https://user-images.githubusercontent.com/94548914/173604633-9eebc4e7-f6c0-4745-80c0-3e1e519d3c2e.JPG" height="50%">

  whitelist에 등록된 https://www.example.com을 제외한 모든 Orgin에대해 CORS를 막아 노래 검색이 불가능해졌다.
</center>
---
layout: single

title: Lyrics Search (CORS), VanilaJS - (10)
categories:
  - VanilaJS

tag: [javascript, SOP, CORS, proxy, tagname, herokuapp]

toc: true

---

# Lyrics Search (CORS)
- API를 활용한 노래검색 예제
- <a href = 'https://codepen.io/kim7720/pen/XWZBMwz'>Live demo</a>
- <a href = 'https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Lyrics%20Search%20App'>Source</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="LyricsSearch" src="https://codepen.io/kim7720/embed/XWZBMwz?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/XWZBMwz">
  LyricsSearch</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 배운점

1. SOP와 CORS

   - Sop: Same Origin Policy
     - 보안상의 이유로, protocol + hostname + port -> origin (https//hostnameA:8080) 이 모두 같은경우 도메인외에 다른 도메인의 HTTP 요청을 막는 정책.
   - CORS: Cross Origin Resource Sharing
     - 다른 도메인의 HTTP요청을 막는 매커니즘인 SOP을 유동적으로 우회하기 위한 방법.
   - 백엔드에서는 CORS를 허용한 URL를 미리 명시 하는식으로 구현.(White Lists) 프론트엔드에서는 header에 Orgin 항목에 schme, domain, port 등을 추가해서 요청함.
     - 이후 백엔드에서 지정된 URL이 담긴 Access-Control-Allow-Origin를 답장의 헤더에 실어서 보내고, 이후 브라우저에서 둘을 비교함.
   - Simple request vs Preflight request
     - Simple request는 서버에 영향을 주지 않는 요청. Preflight request는 서버에 영향으 줄 수도 있는 요청이다. 때문에 Preflight는 요청을 실행하기전 헤더만을 전송받아 CORS을 확인하고 허용된다면 다시 동작을 요청받는 식으로 설계되었다.[더 자세한 설명](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EB%8B%A8%EC%88%9C_%EC%9A%94%EC%B2%ADsimple_requests)

2. Proxy

   - 서버 앞단에서 일을 처리하는 서버
     - 보안상의 문제로 직접 통신을 주고 받을 수 없는 사이에서 중계를 해줌
   - https 생성, 불 필요 요청 차단
   - 속도 저하가 생길 수 있음
   - 서버쪽의 CORS를 설정할 수 없을 때 클라이언트 쪽에서 프록시 서버를 활용하면 해결할 수 있음.
   - 이 예제에서는 간단한 프록시를 활용했다.
   - 이 방법으로 구현하면 속도가 저하 된다는 단점이 있다. 사용자 입장에서는 답답함을 느낄 정도라 다음에는 heroku를 제대로 활용하는 법을 공부해야 겠다.

   ```javascript
   // getMoreSongs
   const getMoreSongs = async (url) => {
     const res = await fetch(`https://cors-anywhere.herokuapp.com/${url}`);
     const data = await res.json();

     showData(data);
   };
   ```

3. .tagname

   - 해당 Element의 태그이름을 대문자로 반환하는 Web api이다.

   ```javascript
   // Get lyrics button click
   result.addEventListener('click', (e) => {
     const clickedEl = e.target;

     if (clickedEl.tagName === 'BUTTON') {
       const artist = clickedEl.getAttribute('data-artist');
       const songTitle = clickedEl.getAttribute('data-songtitle');

       getLyrics(artist, songTitle);
     }
   });
   ```

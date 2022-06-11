---
layout: single

title: Hangman Game, VanillaJS - (9)
categories:
  - VanillaJS

tag: [javascript, svg, keyEvent, regexp]

toc: true

---
# Hangman Game
- svg를 활용한 행맨게임
- <a href='https://codepen.io/kim7720/pen/QWQmzJq'>Live demo</a>
- <a href='https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Hangman%20Game'>Source demo</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="Hangman" src="https://codepen.io/kim7720/embed/QWQmzJq?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/QWQmzJq">
  Hangman</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 배운점

1.  svg

    - SVG란?

      - Scable Vector Graphics의 약자로 2차원 그래픽을 텍스트 편집기에서 픽셀 단위로 수정할 수 있는 XML 파일 형싱의 마크업 언어이다.
      - 픽셀단위로 조작하기 때문에 해상도가 깨지지 않는다.
      - 평면의 컬러 아이콘만 가능하다.
      - declarative하다.

    - svg태그로 선언을 시작하고, height width가 속성으로 주어지면 height \* width 내의 사각형의 픽셀을 다룰 수 있다.
    - 직선은 <line>으로 사각형은 <rect> 원은 <circle>태그가 등 여러 도형이 있다.

    ```html
    <svg height="250" width="200" class="figure-container">
      <!-- Rod -->
      <line x1="60" y1="20" x2="140" y2="20" />
    </svg>
    ```

2.  템플릿 리터럴에서 변수에 배열을 넣을때
    - 템플릿 리터럴 `${}` 에서 변수로 배열을 넣으면 배열의 원소간에 ,를 추가한 모양으로 출력된다. (arr.join(',')형태)
3.  e.key, keyCode

    - 강의에서는 사용자가 입력한 키를 알기위해 event.keyCode를 사용했는데 keyCode는 입력받은 키의 아스키코드를 반환하는 옛날 방법으로 deprecated된 방법이다.
    - e.key는 사용자가 입력한 키를 바로 문자열로 반환한다.

      ```javascript
      // Keydown letter press
      window.addEventListener('keydown', (e) => {
        // Filter special key(Ctrl, Alt, Shift)
        if (e.key.length > 1) {
          return false;
        }

        if (isAlpha(e.key)) {
          const letter = e.key;

          if (selectedWord.includes(letter)) {
            if (!correctLetters.includes(letter)) {
              correctLetters.push(letter);

              displayWord();
            } else {
              showNotification();
            }
          } else {
            if (!wrongLetters.includes(letter)) {
              wrongLetters.push(letter);

              updateWrongLettersEl();
            } else {
              showNotification();
            }
          }
        }
      });
      ```

4.  regular expression, replace 함수 옵션,

    - 자바스크립트에서의 정규표현식(regular expression) 이다.
    - 파이썬에서의 정규표현식과 거의 비슷하고 쓰는 함수나 형태만 다르다.
    - replace함수의 g옵션은 매칭되는 첫 문자만이 아닌 문자열 전체에 대해 replace를 하는 옵션이다.

    ```javascript
    // isAlpha
    const isAlpha = (strs) => {
      return /[a-zA-Z]/.test(strs);
    };
    ```

    ```javascript
    // Show hidden word
    const displayWord = () => {
      wordEl.innerHTML = `
             ${selectedWord
               .split('')
               .map(
                 (letter) => `
     <span class='letter'>
     ${correctLetters.includes(letter) ? letter : ''}
     </span>
     `
               )
               .join('')}
             `;

      const innerWord = wordEl.innerText.replace(/\n/g, '');

      if (innerWord === selectedWord) {
        finalMessage.innerText = 'Congratulations! You won! 👍';
        popup.style.display = 'flex';
      }
    };
    ```

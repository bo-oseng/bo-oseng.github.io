---
layout: single

title: Custom Video Player, VanillaJS - (3)
categories:
  - VanillaJS

tag: [javascript, HTMLMediaElemnet, progress]

toc: true

---

# Custom Video Player

- HTML video 속성을 활요한 커스텀 플레이어 예제
- <a href = 'https://codepen.io/kim7720/pen/ExQWgRe'>Live demo</a>
- <a href = 'https://github.com/bo-oseng/vanilla_javascript_pratice_projects/tree/main/Custom%20Video%20Player'>Source</a>

<iframe height="300" style="width: 100%;" scrolling="no" title="Custom Video Player" src="https://codepen.io/kim7720/embed/ExQWgRe?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/kim7720/pen/ExQWgRe">
  Custom Video Player</a> by KimBosung (<a href="https://codepen.io/kim7720">@kim7720</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## 배운점

1. HTMLMediaElemnet의 유용한 메소드들과 이벤트를 학습 했습니다.
   - 미디어를 실행 시키는 HTMLMediaElemnet.play() 메소드와 정지시키는 HTMLMediaElemnet.paused() 메소드를 알게 되었습니다.
   - 미디어의 현재 시간을 나타내는 HTMLMediaElemnet.currentTime 과, 미디어의 총 시간을 나타내는 HTMLMediaElemnet.duration 프로퍼티를 알게 되었습니다.
   - 미디어의 시간의 변화를 감지하는 timeupdate 이벤트를 알게 되었습니다.

```javascript
// Play & pause video
const toggleVideoStatus = (event) => {
  if (video.paused) {
    video.play();
  } else {
    video.pause();
  }
};
```

```javascript
video.addEventListener('timeupdate', updateProgress);
```

```javascript
// Set video time to progress
const setVideoProgress = () => {
  video.currentTime = (progress.value * video.duration) / 100;
};
```

2.  `<input type ='range'/>`를 간단하게 progress로 꾸며주는 프레임워크인 progress.css가 있다는걸 알게 되었습니다.
    - ::-webkit-slider-thumb을 통해 range 바의 thumb 부분을 커스텀 할 수 있음을 알게 되었습니다.
    - ::-webkit-slider-runnable-track 을 통해 range 바를 커스텀 할수 있음을 알게 되었습니다.

```css
input[type='range']::-webkit-slider-thumb {
  -webkit-appearance: none;
}

input[type='range']::-webkit-slider-runnable-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
  background: #3071a9;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}
```

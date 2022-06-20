---
layout: single

title: Termux를 쓰는 이유, 다운 및 설치
categories:
  - Termux

tag: [안드로이드, 터미널, terminal, linux, 리눅스]

toc: true

---
# Termux란?
>배경<br>
>일단 가장 큰 이유는 어디서든 스마트폰으로 리눅스에 접근할 수 있기 때문이다. 그래서 특수한 환경에 있는 사람들에게 아주 유용할 수 있다. 
>  
> 현역복무 중에 컴퓨터를 접하기가 무척 어려웠고, 예전에 얼핏 안드로이드가 유닉스 계열이란 말을 들었던게 생각나 구글링을 하다보니 나같은 사람들이 꽤나 많았는지 termux라는 오픈소스가 존재했다. 영어 자료들만 너무 많아서 오래걸렸지만 원하는 환경 까지는 모두 구성했다.   
> 
>하지만 분명한 단점도 있는게 속도도 느리고 타이핑 속도도 느려서 진짜 답답하다. 웬만큼 pc를 접하기 어려운 환경이 아니면 별로 추천하고 싶지는 않다.  
<br>

 안드로이드 운영체제는 스마트폰의 리눅스 커널 위에서 실행되는 환경이다. 하지만 사용자 레벨에서는 java나 kotlin으로 작성된 어플리케이션만 접근 가능하고 직접적으로 리눅스로 작성된 파일들은 접근이 어렵다.
 
 하지만 이런 접근을 가능하게 해주는게 바로 termux이다. termux는 안드로이드에서 사용 가능한 리눅스 유사환경 터미널 에뮬레이터이다.
 
## 다운받는 법
Fdroid 공식홈페이지에서 제공하는 Fdroid 에서 다운 받는다.[공식홈페이지](https://f-droid.org/ko/packages/com.termux/)
Fdorid 마켓에서 termux를 찾아 다운 받은뒤 단순히 설치하고 실행하면 터미널화면이 뜨게되고 리눅스 환경이 갖춰지게된다.

<center>
  <img alt="termux-img" src="https://user-images.githubusercontent.com/94548914/174541495-baefdf3f-f67e-4f9f-8152-90a2fe682bb1.jpeg" widht="50%">
</center>
<center>
  <img alt="termux-pwd-ls" src="https://user-images.githubusercontent.com/94548914/174541610-f60874b4-9a80-4ca6-a637-811634aceedc.jpeg" widht="50%">
</center>

pwd, ls 정상 작동
<br>
설치가 잘 되었으면 termux의 활용을 위해서 로컬 저장소 접근권을 설정해줘야한다.
```bash
termux-setup-storage
```  
<center>
  <img alt="termux-storage-setting" src="https://user-images.githubusercontent.com/94548914/174541767-0097297f-656a-423c-8ceb-4dde3c14aadb.jpeg" widht="50%">
</center>


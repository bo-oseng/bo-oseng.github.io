---
layout: single

title: Termux에서 python, Jupyter notebook 설치 및 활용
categories:
  - Termux

tag: [python, 리눅스, terminal, jupyter]

toc: true

---

# Termux의 활용  
 이번 글에서는 내가 termux를 어떻게 활용하고 있는지 소개하려고 한다. 요즘은 알고리즘 공부를 주로 하고 있다. 파이썬을 통해 학습하는데 PC 환경이 아닌 스마트폰에서 구글코랩을 통해 진행한다.  
 구글 코랩은 주피터노트북의 웹버전인데 한가지 아쉬운점이 파일을 .md 나 .html파일이 아닌 .ipynb 파일으로 밖에 다루지 못한다는 점이다. 그래서 스마트폰에서 Jupyter notebook을 쓰고 싶을때가 있는데 termux를 통해 이게 가능하다.

## 설치 및 실행 방법 
1. termux에서 저장소 접근 권한 부여 (2022-02-20 추가) 
termux 터미널에서 우선 로컬 저장소 접근권을 얻는다.  
```termux-setup-storage```  
 
![storage 접근 불가](https://ifh.cc/g/Mdz4jb.jpg)  
 
처음 termux에서 ls 명령어를 치면 저장소에 접근할 수 없어서 아무것도 뜨지 않는다.  
![storage 접근권 생김](https://ifh.cc/g/O9JrZA.jpg)  

2. 파이썬을 설치하기전 우선 터미널에서  
```apt upgrade && apt update```  
을 타이핑한다.명령어를 통해 사용 가능한 패키지들과 버전에 대한 정보를 업데이트하고 업그레이드 한다.  
  

![apt update](https://ifh.cc/g/tLHEdt.jpg)
설치 도중에 나오는 y/n은 읽어보고 대부분 y를 타이핑 하면된다.

3. pkg로 파이썬을 설치  
```pkg install python```  
을 타이핑한다. 이후 python이 설치되고나면 python을 타이핑 후 정상적으로 설치되었는지 확인한다.  

![python install](https://ifh.cc/g/wDg68G.jpg)  
![python check](https://ifh.cc/g/zPqIYm.jpg)

4. 이휴 Crtl d 나 exit() 로 빠져나온뒤  
```pip install --upgrade pip```  
를 통해 pip버전으로 업그레이드한다.  

![pip upgrade](https://ifh.cc/g/kFrxNK.jpg)  
![pip upgrade](https://ifh.cc/g/edETNt.jpg)  

5. pip로 주피터 노트북 설치  
```pip install jupyter```  
를 통해 주피터 노트북 설치.  

![jupyter install](https://ifh.cc/g/9pa7sx.jpg)  
설치 이후에 termux 터미널에서 jupyter notebook를 타이핑하면 로컬호스트의 8888포트로 주소가 생성되고 웹에서 접속하면 주피터 노트북을 쓸 수있다.  

![jupyter install](https://ifh.cc/g/rz1uAH.jpg)

![jupyter notebook](https://ifh.cc/g/wChCnh.jpg)  
  
   

### 활용하는 개인적인 방법  
나랑 비슷한 환경인 사람은 몇 없겠지만 그래도 내가 활용하는 방법을 소개하려고 한다. 일단 주피터 노트북이 모바일용은 아니라 스마트폰에서 활용하기에는 꽤나 불편하다 그래서 구글 코랩에서 코딩 및 ipynb파일 작성후에 로컬에 저장한다. 그 후 주피터노트북에서 로컬의 ipynb 파일을 열고 .md나 .html파일로 내려받은 뒤 깃허브에 커밋을 하거나 prose.io라는 웹 Markdown 에디터에서 수정을 하고 포스킹 한다.
![example](https://ifh.cc/g/QoHOcy.jpg)  

![example](https://ifh.cc/g/ShwiPO.jpg)  

![example](https://ifh.cc/g/r8wBWT.jpg)  

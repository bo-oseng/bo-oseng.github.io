---
layout: single

title: Termux에서 루팅없이 Gdrive 마운트, chroot, 데비안설치
categories:
  - Termux

tag: [gdrive, chroot, debian, fuse]

toc: true

published: false
---
# Termux에서 gdrive mount(rclone, fuse, proot )
  
  이전에 [Jupyter notebook](http://daringfireball.net/projects/markdown/)글에서 내가 블로그에 글을 올리는 환경에 대하 언급한 바 있다. 간략히 말하면 구글코랩에서 .ipynb 파일에서 코딩 및 간략한 포스팅을 한 후에, 구글 코랩과 연동된 gdrive어플로 일일이 파일을 로컬로 다운 받은 뒤 termux의 주피터노트북에서 .md 파일로 변환한 후에 깃허브에 업로드 하는 식이다.  
  
  매번 파일을 로컬로 받는 과정이 번거로웠고, 윈도우의 클라운드  공유 폴더 처럼 리눅스에서 gdrive를 설치할 수 있지 않을까 해서 구글링을 해보았고 rclone이라는 go언어로 짜여진 프로그램이 있었다. termux에서 rclone을 설치하는 법을 소개하려고 한다.

## rclone 설치 이전
rclone을 설치하기 전 rclone 공식 홈페이지의 [rclone mount](https://rclone.org/commands/rclone_mount/) 를 보면 fuse를 활용하므로 fuse설치가 필요함을 알 수 있다.    
(fuse는 file system in user space의 약자로 커널과 파일이 소통하는 filesytem을 유저가 커스텀해 쓸수 있게 해주는 모듈이다. [자세한 설명](https://ssup2.github.io/theory_analysis/Linux_FUSE/) )

fuse설치를 위한 명령어를 termux에서 실행하면
  
  ```sudo apt-get install fuse```
  
  여기서 부터 문제가 생기는데 루팅권한이 필요하다고 나온다.![fuse error](https://ifh.cc/g/z3hoob.jpg)
이를 우회하기 위해서 termux 내부에서 chroot중 일부기능만 구현한 바이너리인 proot를 이용한다.

## proot ( chroot )로 루트권한 우회후 설치
결론부터 말하자면

```
pkg update -y && pkg install wget curl proot tar -y && wget https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Debian/debian.sh -O debian.sh && chmod +x debian.sh && bash debian.sh
```  
![데비안 설치](https://ifh.cc/g/1x8fXx.jpg)  

![데비안 설치완료](https://ifh.cc/g/thKtAj.jpg)  

  
  위 명령어로 데비안이 루트 하위에 설치되고  
        ```./start-debian.sh```    
  
  위 명령어로 데비안이 실행되며 데비안 안에서 fuse설치를 진행하면 된다.  
  
  ```apt-get install fuse```  
![fuse설치](https://ifh.cc/g/dqZebD.jpg)

그 이후 rclone을 설치 해준다.   


### proot(chroot) 설명  
chroot는 change root directory의 줄임말로 루트의 디렉터리사이의 계층을 나누는   프로그램이다.
루트의 하위 디렉터리와 상위를 분리시켜 하위 디렉터리에서 더이사 상위 디렉터리로 가지 못하게 격리하고 하위 디렉터리를 새로운 루트로 하는 프로세스를 만들 수 있다. 내가 보고 이해한 다른 블로그 링크를 참조한다.
  [chroot 자세한 설명](https://www.44bits.io/ko/post/change-root-directory-by-using-chroot)
  



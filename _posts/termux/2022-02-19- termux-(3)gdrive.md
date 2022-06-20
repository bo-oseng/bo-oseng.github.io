---
layout: single

title: Termux에서 루팅없이 Gdrive 마운트, chroot, 데비안설치
categories:
  - Termux

tag: [gdrive, chroot, debian, fuse]

toc: true

published: false
---
# Termux에서 gdrive mount(rclone)
  > 배경  
  >이전에 [Jupyter notebook](http://daringfireball.net/projects/markdown/)글에서 내가 블로그에 글을 올리는 환경에 대하 언급한 바 있다. 간략히 말하면 구글코랩에서 .ipynb 파일에서 코딩 및 간략한 포스팅을 한 후에, 구글 코랩과 연동된 gdrive어플로 일일이 파일을 로컬로 다운 받은 뒤 termux의 주피터노트북에서 .md 파일로 변환한 후에 깃허브에 업로드 하는 식이다. 매번 파일을 로컬로 받는 과정이 번거로웠고, 윈도우의 클라우드 공유 폴더 처럼 리눅스에서 gdrive를 설치할 수 있지 않을까 해서 구글링을 해보았고 rclone이라는 프로그램이 있었다. termux에서 rclone을 설치하는 법을 소개하려고 한다.

#### rclone 설치 전
+ rclone을 설치하기 전 rclone 공식 홈페이지의 [rclone mount](https://rclone.org/commands/rclone_mount/) 를 보면 fuse를 활용하므로 fuse설치가 필요함을 알 수 있다.    
+ fuse는 File System in User Space의 약자로 커널과 파일이 소통하는 File Sytem을 유저가 커스텀해 쓸수 있게 해주는 모듈이다. [자세한 설명](https://en.wikipedia.org/wiki/Filesystem_in_Userspace))
+ File System은 컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제를 가리키는 말이다.

하지만 termux에서 fuse 설치를 시도하면 file system은 민감한 정보이므로 루트권한을 요구한다.<br>
```bash
sudo apt-get install fuse
```
<center>
  <img src="https://ifh.cc/g/z3hoob.jpg"/ width="50%">
</center>
termux는 루트 권한이 없고, 이를 우회하기 위해서 termux에서 chroot중 일부기능을 구현한 바이너리인 proot를 이용 해야한다.
<br>  


### chroot
chroot란 Change Root Directory의 줄임말로 현재 실행 중인 프로세스와 차일드 프로세스 그룹에서 루트 디렉터리를 변경하는 작업이다. 루트의 하위 디렉터리와 상위를 분리시켜 하위 디렉터리에서 더 이상 상위 디렉터리로 가지 못하게 격리하고 하위 디렉터리를 새로운 루트로 하는 프로세스를 만들 수 있다.
[chroot 자세한 설명](https://ko.wikipedia.org/wiki/Chroot)
  

### proot로 하위 디렉터리에 루트권한을 가진 debian 설치

```bash
pkg update -y && pkg install wget curl proot tar -y && wget https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Debian/debian.sh -O debian.sh && chmod +x debian.sh && bash debian.sh
``` 
<center>
  <img alt="데비안 설치" src="https://ifh.cc/g/1x8fXx.jpg" width="50%">
</center>
<center>
  <img alt="데비안 설치완료" src="https://ifh.cc/g/thKtAj.jpg" width="50%">
</center>


  
데비안이 하위 디렉터리에 설치되고  
```bash
./start-debian.sh
``` 
데비안이 실행 후

```bash
apt update
apt upgrade
apt-get install sudo
sudo apt-get install fuse
``` 
fuse설치를 진행하면 된다.   
<center>
  <img alt="fuse설치" src="src" width="50%">
</center>

```bash
curl https://rclone.org/install.sh | bash sudo
```
그 이후 rclone을 설치 해준다.   



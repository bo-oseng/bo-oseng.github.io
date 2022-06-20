---
layout: single

title: Termux에서 code-server(VScode) 실행하기 (루팅 없이 extension 가능)
categories:
  - Termux

tag: [termux, chroot, proot, debian, code-server, VScode]

toc: true

published: ture
---

# Code-server란
[code-server](https://github.com/coder/code-server)<br>
Visual Studio Code는 대부분 타입스크립트로 짜여진 프로그램이다. 이를 활용해서 VScode를 어느 환경에서든 웹앱으로서 실행할 수 있게 해주는 오픈소스 프로젝트이다.


<center>
  <img at="termux code-server-fail" src="https://user-images.githubusercontent.com/94548914/174535469-4b91af66-96f3-4f99-ac5d-87bf01234c97.jpeg" width="50%">
</center>
  



하지만 code-server를 termux에서 설치하고자 하면 root 권한이 없어 오류가 생긴다. 안드로이드에 직접 루트권한을 가진 터미널이 아닌 termux위에서 동작하는 터미널이기 떄문이다. 이를 우회하기 위해서 chroot를 활용해야한다.


## chroot란
chroot란 Change Root Directory의 줄임말로 현재 실행 중인 프로세스와 차일드 프로세스 그룹에서 루트 디렉터리를 변경하는 작업이다. 루트의 하위 디렉터리와 상위를 분리시켜 하위 디렉터리에서 더 이상 상위 디렉터리로 가지 못하게 격리하고 하위 디렉터리를 새로운 루트로 하는 프로세스를 만들 수 있다.([chroot 더 자세한 설명](https://ko.wikipedia.org/wiki/Chroot))
<center>
  <img at="chroot-tree" src="https://user-images.githubusercontent.com/94548914/174535110-2a245c0c-ab41-49a3-84ff-3e17e1c32467.png" width="50%">
</center>

그리고 기본적으로 termux는 chroot중 일부기능을 구현한 바이너리인 proot를 지원하다.

## proot distro 설치

pkg update를 하고, pkg로 proot-distro를 설치한다
```bash
pkg update -y && pkg install proot-distro
```
<center>
  <img at="proot-distro-install" src="https://user-images.githubusercontent.com/94548914/174535955-73da9c67-7317-492a-8292-f10bdbdad42d.jpeg" width="50%">
</center>

## debian 설치
proot-distro로 debian을 설치한다.
```bash
proot-distro install debian
```
<center>
  <img at="debian-install-1" src="https://user-images.githubusercontent.com/94548914/174536997-a0d07ef9-db20-4609-9bd9-43f40de4aa73.jpeg" width="50%">
</center>
<center>
  <img at="debian-install-2" src="https://user-images.githubusercontent.com/94548914/174536998-8ce09674-d487-4941-aa3b-6747c8e21cf3.jpeg" width="50%">
</center>

데비안 로그인후 root 확인, sudo vim git 설치

```bash
proot-distro login debian
```
```bash
apt update && apt upgrade -y && apt-get install sudo vim git -y
```
<center>
  <img at="sudo-vim-git-install" src="https://user-images.githubusercontent.com/94548914/174537549-c2a64f0d-5cda-4cbb-ae7f-430a30f06718.jpeg" width="50%">
</center>

## nodejs 설치
code-server의 dependency인 nodejs 설치가 필요하다. lts버전으로 설치하자
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt-get install -y nodejs
```
<center>
  <img at="nodejs-lts-install" src="https://user-images.githubusercontent.com/94548914/174538011-151b039f-9718-4081-b9ed-594342141046.jpeg" width="50%">
</center>

## code-server 설치
node 설치가 잘 되었는지 확인하고 code server를 설치
<center>
  <img at="code-server-install" src="https://user-images.githubusercontent.com/94548914/174538680-7047902d-4b0d-490b-873d-f577b64197dd.jpeg" width="50%">
</center>


## code-server 실행, 익스텐션, 터미널 작동 확인
code-server 설치가 완료됐다면 실행해보자. 패스워드 설정도 가능하지만 ```--auth none``` 옵션으로 실행하면 패스워드 없이 로컬호스트의 8080포트로 실행된다.
```bash
code-server --auth none
```
<center>
  <img at="code-server-exec" src="https://user-images.githubusercontent.com/94548914/174539718-d1ab88d1-5f44-4d42-b34d-d15d48aa26af.jpeg" width="50%">
</center>

웹에서 접속하면 vscode를 볼 수 있다.
<center>
  <img at="code-server-img" src="https://user-images.githubusercontent.com/94548914/174538948-6e39cf07-0969-43f1-8fff-c525673c1915.jpeg" width="50%">
</center>

터미널과 익스텐션 모두 잘 실행된다. 삼성 DEX를 활용하면 PC에서 처럼 활용할 수 있다.
<center>
  <img at="code-server-img" src="https://user-images.githubusercontent.com/94548914/174538937-d8b7cd75-e9a2-4250-bb70-97f9f8e12b6d.jpeg" >
</center>





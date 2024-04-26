---
layout: post
title: 네이버클라우드 우분투 서버 기본세팅
tags: [네이버클라우드, Ubuntu]
---

예전부터 데비안 계열의 리눅스가 취향에 맞거니와 요즘은 대세로 자리잡기도 해서 리눅스는 [우분투](https://ubuntu.com/)를 적극 추천하고 있다. 상용 서비스는 클라우드를 추천하는데 [AWS](https://aws.amazon.com/)는 비용이 넘사벽이라 거의 모방(?)해서 만든 [네이버클라우드](https://www.ncloud.com/)를 주로 사용한다. 네이버클라우드의 Object Storage 서비스도 Amazone의 S3 API와 Browser를 활용하고 있다.

네이버클라우드 기준으로 우분투는 서버생성시 기본으로 설치해 주니까 기본 업데이트, NAS연결, Java 설치 정도만 해주면 되고 여기에 필요한 소프트웨어를 올리면 된다. 아직까지 Classic 환경에서 Ubuntu 18.04 까지만 지원하는 것은 아쉽다.

## 1. ACG(Access Control Group) 설정

서버생성 전에 먼저 ACG를 생성하고 서버생성시 해당 ACG를 선택하는 것이 좋다. 미리 생성하지 않으면 기본 ACG와 매칭이 되는데 여러 서버를 설정할 경우 네이밍 관리에서 불편하다. 보통 HTTP의 80, SSL의 443, SSH의 22번 정도만 오픈하면 웹사이트를 운영할 수 있다.

## 2. NAS(Network Attached Storage) 생성

서버의 기본 디스크용량은 50G인데 운영 운영중에 용량이 부족하면 귀찮은 작업을 해야 하니 디스크 사용량이 지속적으로 늘어날 것이라 예상되면 NAS를 생성해서 연결해서 사용하면 관리가 편하다. 비용도 500G/36,000원/월로 저렴하다.

## 3. Ubuntu 서버 생성

네이버클라우드 관리페이지(콘솔)에서 UI로 클릭만하면 약 2분 정도에 생성된다. 외부 서비스를 한다면 공인IP도 생성해서 연결한다.

## 4. OS 업데이트

```
# 최신 패키지 목록 받아오기
$ apt update

# 최신 패키지 업그레이드
$ apt upgrade

# 참고 - 의존성 있는 패키지 업그레이드
$ apt dist-upgrade

# 참고 - 의존성 문제로 설치가 안될 경우 (you have held broken packages)
$ aptitude install application-name

# 참고 - 필요없는 패키지 제거하기
$ apt autoremove
$ apt clean

# 참고 - 설정에 남아있는 패키지 완전 제거하기
$ dpkg --list |grep "^rc" | cut -d " " -f 3 | xargs sudo dpkg --purge
```

## 5. NAS 마운트

```
# nfs-common 패키지 설치 및 데몬 기동
$ apt install nfs-common
$ systemctl start rpcbind.service

# 마운트 포인트 생성 및 마운트 (마운트정보 예:10.10.10.10:/n1234567_nasname)
$ mkdir /mnt/nas
$ mount -t nfs -o vers=3 10.10.10.10:/n1234567_nasname /mnt/nas

# 마운트 정보 유지 (파일오픈 후 텍스트 추가)
$ vi /etc/fstab

# 볼륨 마운트 정보 추가 시작 ---
10.10.10.10:/n1234567_nasname /mnt/nas nfs vers=3, defaults 0 0
# 볼륨 마운트 정보 추가 끝 ---
```

## 6. 심볼링 링크 생성

개인취향이겠지만 디렉토리는 /app과 /data 만 생성해서 application은 /app에 디스크용량이 늘어가는 데이터 등은 /data에 사용하고 있다. 상황에 따라 심볼링 링크를 /data/xxxx 처럼 하위에 해도 되고 /data 전체를 사용한다.

```
# /data를 마운트된 NAS로 심볼링 링크 사용 (NAS에서 사용할 디렉토리 예:server-01)
$ mkdir /mnt/nas/server-01
$ ls -s /mnt/nas/server-01 data
```

## 7. Java 설치

어플리케이션이 OS의 Java를 사용하지 않으면 설치할 필요는 없다.

```
# 원하는 버전의 Java 설치
$ apt install openjdk-17-jre-headless

# Java 환경변수 설정 (파일오픈 후 텍스트 추가)
$ vi /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib:$CLASS_PATH
```

참고로, 서버 비밀번호 확인 등에 사용되는 인증키는 동일하게 발급받을 수 없으므로 반드시 안전한 곳에 보관하고 관리해야 한다.

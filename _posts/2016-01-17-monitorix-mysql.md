---
layout: post
title: Monitorix & MySQL
tags: [모니터링, MySQL]
---

우분투에서 MySQL을 모니터링 할 수 있는 프리웨어를 찾다가 선정한 Monitorix.

사실 Monitorix는 MySQL 모니터링 툴이라기 보다는 그래픽 시스템 모니터링 툴에 가깝지만 홈페이지에서 lightweight하다고 하니 세팅을 했는데 MySQL의 Perl 인터페이스 드라이브가 없으면 추가로 설치해야 한다.

![monitorix-mysql](/assets/images/monitorix-mysql.png)

홈페이지 : [http://www.monitorix.org/](http://www.monitorix.org/)  
우분투설치 : [http://www.monitorix.org/doc-debian.html](http://www.monitorix.org/doc-debian.html)

설치가 한방에 안되서 구글링해서 정리해 본다.  
설치참조-1 : [http://forum.falinux.com/zbxe/index.php?document_srl=832159](http://forum.falinux.com/zbxe/index.php?document_srl=832159)  
설치참조-2 : [http://blog.alghost.co.kr/개인-서버-및-nas-자원-관리를-위한-monitorix-설치/](http://blog.alghost.co.kr/개인-서버-및-nas-자원-관리를-위한-monitorix-설치/)  
설치참조-3 : [https://znil.net/index.php?title=Monitorix_unter_Ubuntu_14.04.x_LTS_installieren](https://znil.net/index.php?title=Monitorix_unter_Ubuntu_14.04.x_LTS_installieren)

### 1. 저장소 추가

sudo vi /etc/apt/sources.list 에서  
deb http://apt.izzysoft.de/ubuntu generic universe 추가

### 2. 저장소 GPG키 설치

```
wget http://apt.izzysoft.de/izzysoft.asc
sudo apt-key add izzysoft.asc
rm izzysoft.asc
```

### 3. 설치

```
sudo apt-get update
sudo apt-get install monitorix
```

### 4. MySQL의 Perl 인터페이스 드리이브 추가(MySQL을 위한 옵션)

```
sudo apt-get install libdbd-mysql-perl
```

### 5. 환경설정

/etc/monitorix/monitorix.conf 에서 포트변경, 계정설정, MySQL 접속정보설정, UI 및 HTML 변수 설정  
/etc/monitorix/conf.d/00-debian.conf 에서 MySQL 접속정보설정

### 6. 서비스 재시작

```
sudo service mysqld restart
sudo service monitorix restart
```

실행방법 : http://serverip:port/monitorix

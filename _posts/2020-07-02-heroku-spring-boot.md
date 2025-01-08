---
layout: post
title: Heroku 가입부터 Spring Boot 배포까지
tags: [Heroku]
---

Java Web Application을 가볍게 테스트하기 좋은 PaaS(Platform as a Service)인 Heroku에 Spring Boot로 배포해본다.

DB도 무료로 제공하니 연결해보고, hello world만 찍어보면 심심하니까 Bing 배경화면 API와 과 Google 웹폰트 API를 파싱해서 접속할 때마다 배경과 폰트를 랜덤하게 출력해본다.

### 회원가입

[heroku.com](https://www.heroku.com/)에서 회원가입을 진행하면 인증 이메일이 오고 해당메일에서 계정 활성화 링크를 클릭하면 된다.

### App 생성

App Name을 입력하고 지역을 선택한다.  
App 생성할때 개발언어를 지정하는 것이 아니고, 배포시 개발언어는 자동으로 인지하게 된다.  
Domain, SSL 등을 특별히 설정허지 않는다면 최초 [https://baenam.herokuapp.com](https://baenam.herokuapp.com/) (예.app-name:baenam)으로 세팅된다.  
![heroku-app-info](/assets/images/heroku_app_info.png)  
(참고. App 생성 후 대시보드의 정보)

### Database 생성

대시보드에서 생성한 App의 Resources 탭에서 ClearDB MySQL을 추가한다.  
생성 후에 Settins 탭의 Config Vars에 DB 접속정보를 볼 수 있다.  
접속정보로 Spring Boot의 접속 URL로 설정하면 되고, DB관리툴에서 접속정보로 사용하면 된다.

```
mysql://b75edac79f845f:0c4ae07b@us-cdbr-east-05.cleardb.net/heroku_fcd419078efa524?reconnect=true  (예시)
```

host-name: us-cdbr-east-05.cleardb&#46;net  
user-name: b75edac79f845f  
user-password: 0c4ae07b  
database-name: heroku_fcd419078efa524

### Spring Boot 프로젝트 생성

[start.spring.io](https://start.spring.io/) 또는 사용하는 IDE에서 jar 패키징으로 프로젝트를 생성한다.  
Heroku를 이용하기 위해 특별하게 설정할 것은 없다.  
간단히 테스트를 위한 프로젝트라 운영환경을 구분하지 않고 환경설정 파일(application.yml)에 port를 80으로 했다.

```
spring:
  profiles:
    active: default
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://b75edac79f845f:0c4ae07b@us-cdbr-east-05.cleardb.net/heroku_fcd419078efa524?reconnect=true
    username: b75edac79f845f
    password: 0c4ae07b
  transaction:
    rollback-on-commit-failure: true

mybatis:
  mapper-locations: classpath:sqlmap/mysql/**/*.xml
  config-location: classpath:sqlmap/sql-mapper-config.xml

server:
  port: 80
  servlet:
    session:
      tracking-modes: COOKIE
```

### Heroku 환경을 위한 세팅

프로젝트 실행을 위한 파일을 생성한다.  
프로젝트 루트 폴더에 Procfile 이라는 확장자 없는 파일명을 생성하고 실행옵션을 작성한다.

```
web: java -Dserver.port=$PORT $JAVA_OPTS -jar target/hello-heroku-0.1.jar
```

Heroku가 Java 1.8이 기본이라 프로젝트에 사용한 OpenJDK 11로 설정하기 위해 프로젝트 루트 폴더에 system.properties 파일을 생성하고 작성한다.

```
java.runtime.version=11
```

### Bing 배경화면 및 Google 폰트 활용

Bing과 Google에서 GET으로 호출하면 Json으로 리턴해주는 API를 제공하므로 적당히 파싱해서 사용한다.  
Google 폰트는 배열로 리턴하므로 한글폰트만 먼저 추출하고 랜덤하게 1개만 사용하면 된다.  
참고. Bing 배경화면: `https://www.bing.com/HPImageArchive.aspx?format=js&idx= + (int)(Math.random() * 7) + &n=1&mkt=en-US`  
참고. Google 폰트: `https://www.googleapis.com/webfonts/v1/webfonts?key=user_key`

### Heroku 배포

대시보드의 Deply 탭에 보면 Heroku에 배포하는 방법은 현재 3가지를 제공하고 각각의 방법이 설명되어 있다.  
간단하게 Heroku Git으로 배포하려면 먼저 Heroku CLI(Command Line Interface)를 운영체제에 맞게 설치하고 가이드에 맞게 진행하면 된다.  
참고. Heroku CLI: [https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

```
heroku login
```

브라우저에서 로그인 페이지가 열리고 로그인 후 터미널에서 Heroku에 로그인이 된다.

```
git init
```

git 저장소로 사용할 폴더로 이동해서 repository를 생성하면 .git 폴더가 생성된다.

```
heroku git:clone -a baenam
```

Heroku Git의 저장소로 복사하고 push 할 수 있게 한다.  
기존 Spring Boot 프로젝트 폴더를 .git 폴더를 생성한 곳으로 복사한다.

```
git add .
```

변경된 모든 파일을 스테이지에 올린다.  
윈도우에서 CRLF 오류가 나면 'git config --global core.autocrlf true'로 설정을 변경하고 다시 진행한다.

```
git commit -am "commit message"
```

커밋 메시지를 포함해서 커밋한다.

```
git push heroku master
```

터미널에서 배포로그를 볼 수 있고, 완료되면 제공된 도메인에서 확인해 볼 수 있다.  
배포가 정상적으로 완료해도 경로를 찾을 수 없다는 오류가 나오면 dyno 옵션을 지정해 준다.

```
heroku ps:scale web=1
```

### 어플리케이션 확인

![baenam-herokuapp-com](/assets/images/baenam_herokuapp_com.png)  
메인페이지: [https://baenam.herokuapp.com](https://baenam.herokuapp.com)  
DB접속페이지: [https://baenam.herokuapp.com/testdb](https://baenam.herokuapp.com/testdb)

참고로, Heroku 서비스는 30분 이상 접속이 없으면 sleep 모드로 들어가는데 이때 접속하면 활성화되는 시간만큼 delay가 생기므로 무료로 Cronjob을 제공하는 [cron-job.org](https://cron-job.org/)에서 30분마다 접속하는 것으로 설정하면 sleep 모드없이 접속할 수 있다.

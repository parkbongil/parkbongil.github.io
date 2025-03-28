---
layout: post
title: 포터블 STS 4.29.1 만들기
tags: [STS]
---

Java 개발환경이 많이 바뀌고 AI Assistant도 범용화 되어 포터블 STS 환경을 다시 정리해 본다.
몇 년 전과 비교해 보면 프레임워크가 Spring에서 Spring Boot로 바뀌어 로컬에 Tomcat 세팅이 필요없어졌고 개인적으로 템플릿엔진을 JSP에서 Thymeleaf로 바꾸었고 Copilot4Eclipse가 무료로 풀리면서 필수로 설치하는 플러그인이 된 듯 하다.

시스템과 종속성을 없애서 USB로 다른 컴퓨터에 복사해서 바로 사용할 수 있도록 하는 것이 목적이지만 개인적인 취향의 설치방법 및 세팅도 포함해서 공유해 본다.

### 개요

- Spring Boot 기반의 Java 프로젝트 개발환경임
- Windows 11 64bit 기준이며 git은 설치 및 세팅된 상태라고 가정함
- 다른 컴퓨터에 D:\STS를 복사만으로 바로 실행할 수 있도록 세팅함

### 다운로드

세팅에 필요한 파일을 OS에 맞게 다운로드 받아 놓는다.

- [Spring Tools](https://spring.io/tools)
- JDK ([Amazon Corretto](https://aws.amazon.com/ko/corretto/)의 OpenJDK를 선호함)
- [Maven](https://maven.apache.org/)
- [lombok.jar](https://projectlombok.org/) (수동설치를 선호함)
- jp.gr.java_conf.ussiy.app.propedit_6.0.5.jar (수동설치를 선호함)
- Roboticket fixed.xml (선호하는 [이클립스 컬러테마](https://eclipse-color-themes.web.app/))

### 설치위치

D:\STS에 java, maven, workspace 폴더를 생성하고 다운받은 파일을 해당 폴더에 복사함

- D:\STS\java\ - jdk21.0.6_7 폴더 복사 (필요시 여러버전 복사)
- D:\STS\maven\ - apache-maven-3.9.9 폴더 복사
- D:\STS\sts-4.29.1.RELEASE - Spring Tool 압축해제 후 이동함
- D:\STS\workspace\ - 프로젝트 워크스페이스 위치

### Maven 설정

maven repository는 C:\User…가 기본 위치인데, STS 하위 폴더로 설정  
D:\STS\maven\apache-maven-3.9.9\conf\settings.xml 에서 55라인 추가 후 저장  
세팅한 repository 폴더를 해당 디렉토리에 생성

```
<localRepository>D:\STS\maven\apache-maven-3.9.9\repository</localRepository>
```

### STS 초기화 설정

STS 초기화 파일인 D:\STS\sts-4.29.1.RELEASE\SpringToolSuite4.ini 을 수정 및 추가

```
-Dfile.encoding=UTF-8
-Xms4096m (적당한 사이즈로 수정)
-Xmx8192m (적당한 사이즈로 수정)
-javaagent:D:\STS\sts-4.29.1.RELEASE\lombok.jar (lombok.jar 수동설치)
```

### Properties Editor 수동설치

신규 프로젝트는 인코딩 이슈가 없으나 기존 프로젝트는 한글깨짐 방지를 위해 수동설치함  
\{Eclipse Install Path}\plugins 에 해당 jar 파일을 복사하고 \{Eclipse Install Path}\configuration\org.eclipse.equinox.simpleconfigurator\bundles.info 하단에 설정을 추가함

```
jp.gr.java_conf.ussiy.app.propedit,6.0.5,plugins/jp.gr.java_conf.ussiy.app.propedit_6.0.5.jar,4,false
```

여기까지가 STS 실행 전에 설정이여 STS를 실행하고 플러그인 설치 및 설정을 진행한다.

### Perspective 및 View 세팅

개인취향으로 Perspective는 Debug, Git, Team Synchronizing를 추가하고 텍스트 보이기로 설정하며 View에는 Console, Progress, Search를 추가한다.
Problems 필터속성에서 Error/Warnings on Project 체크해서 해당 프로젝트만 볼 수 있도록 설정한다.

### 플러그인 설치

- Eclipse Color Theme 설치: Help > Install New Software 에서 https://eclipse-color-theme.github.io/update/ 입력 후 설치
- Eclipse Web Developer Tools 설치: Help > Eclipse Marketplace 에서 검색
- Thymeleaf Plugin for Eclipse 설치: Help > Eclipse Marketplace 에서 검색
- Copilot4Eclipse 설치: Help > Eclipse Marketplace 에서 검색

### Peeferences 설정

- General > Appearance > Colors and Fonts 에서 기본폰트를 Cascadia Nono 으로 변경 (개인취향)
- General > Appearance > Color Theme 에서 다운받은 Roboticket fixed.xml 으로 설정 (선호테마)
- General > Editors > Text Editors 에서 Use find/replace overlay 체크해제 (팝업창으로 변경)
- General > Editors > Text Editors > Annotations 에서 infos 항목의 Vertical ruler 와 Text as 항목 비활성 (어노테이션 주의표시 비활성)
- Install/Update > Automatic Updates 에서 설정 해제 (수동 업데이트를 선호함)
- Java > Installed JREs 에서 다운로드 받은 로컬폴더의 JDK 추가 (필요하면 여러개 추가)
- Maven > User Settings 에서 세팅파일을 로컬폴더의 settings.xml 으로 변경
- Run/Debug > Console 에서 Console buffer size 를 80000에서 800000 으로 변경 (콘솔 로그 늘리기)
- Version Control > Git 에서 기본폴더를 D:\STS\workspace 로 설정
- Web > HTML Files > Validation에서 Attributes > Undefined Attribute name을 Ignore로 수정 (HTML5 속성 경고 비활성)
- Package Explorer 뷰에서 필터속성에 .\*resources 체크 해제 (.gitignore 파일 보기)
- Package Explorer 뷰에서 보기모드를 Hierarchical로 변경 (트리구조로 보기)

여기까지가 설정하면 웹사이트 개발시 에러나 주의표시 하나없이 개발할 수 있고 D:\STS 폴더만 복사해서 다른 컴퓨터에서 바로 실행해서 개발할 수 있다.

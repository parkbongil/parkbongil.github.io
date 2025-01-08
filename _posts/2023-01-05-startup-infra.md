---
layout: post
title: 개발 스타트업의 프리웨어 인프라
tags: [Redmine, Github, Jenkins, Scouter, Sonarqube]
---

내가 생각하는 개발 스타트업에서 거의 필수로 사용해야 하는 인프라를 프리웨어 기준으로 정리해 본다.

우선 개발 인프라 이외에 필수로 사용해야 하는 유료 서비스는 2가지 이다.

- Google Workpace 또는 Microsoft 365
- Cloud Server Hosting

Google Workpace 나 Microsoft 365의 선택기준은 데스크탑 오피스의 활용유무이다. Google Workpace를 기본으로 고민하되 데스크탑 엑셀과 파워포인트를 주로 사용한다면 Microsoft 365를 선택하는게 좋지 않을까 생각된다.

서버호스팅은 AWS가 최고이지만 가격도 최고이다. 네이버클라우드가 AWS를 거의 모방해서 제공하고 있으니 가격이 부담되면 네이버클라우드도 나쁘지 않다고 생각된다.

개발부터 포팅까지 주요 인프라는

- 이슈관리
- 소스형상관리
- 코드품질관리
- 배포관리
- 모니터링

이 필수이며 [Atlassian](https://www.atlassian.com/)에서 모두 서비스로 제공하고있고 최고라고 생각되지만 비용을 고려한다면 프리웨어로 구성해도 생산성에 그리 나쁘지 않다. 사내에 구축하면 호스팅 및 인프라 비용도 아낄 수 있다. 현재 모두 우분투에 설치해서 사용하고 모니터링만 제외하고 모두 LDAP으로 로그인 하고 있다.

## Redmine - 이슈관리

[https://www.redmine.org/](https://www.redmine.org/)

![Redmine](/assets/images/infra-redmine.png)

(이미지 출처 - 공식홈 캡춰)

기획이나 디자이너는 UI가 불편하다고 Yona로 전환도 해봤지만 결국 안쓰고 Trello를 가장 좋아하던데 개발자 입장에서는 일감이나 위키는 매우 유용하다고 판단된다.  
UI가 한국적이지 않아 약간의 수정이 필요하긴 하지만 HTML 수정하는 수준이라 Ruby언어의 장벽도 거의 없고 DTHM 에디터나 GItLab Hook 플러그인은 거의 필수이고 이외에 매우 다양한 플러그인을 제공하지만 업데이트에 꼬일 수 있으므로 최소한의 플러그인만 사용하도록 하자.

## Gitlab - 소스형상관리

[https://about.gitlab.com/](https://about.gitlab.com/)

![Gitlab](/assets/images/infra-gitlab.png)

(이미지 출처 - 위키피디아)

소스형상관리에서는 독보적인 툴이 아닌가 싶다. 현재는 이슈트래커와 위키 등도 제공되어 전체적인 개발 라이프사이크를 모두 지원하는 방향으로 가고 있지만 Git의 원격저장소로는 충분한 기능을 제공한다.  
Github이 많은 기능이 무료로 전환되었지만 회사에서 프로젝트로 사용하기에는 그룹 및 프로젝트 설정 등에서 훨씬 유용하다.

## Jenkins - 배포관리

[https://www.jenkins.io/](https://www.jenkins.io/)

![Jenkins](/assets/images/infra-jenkins.png)

(이미지 출처 - 위키피디아)

빌드 및 배포관리를 UI로 제공해서 백엔드개발자 뿐만 아니라 퍼블리싱이나 프론트개발자도 부담없이 사용할 수 있다.  
당연히 Gitlab과 연동해서 사용하며 Java 뿐만 아니라 Vue, React도 배포관리가 가능하다.

## Scouter - 모니터링

[https://github.com/scouter-project/scouter](https://github.com/scouter-project/scouter)

![Scouter](/assets/images/infra-scouter.png)

(이미지 출처 - 공식 Github)

LG CNS에서 만들어 공개한 오픈소스로 유료인 제니퍼 만큼 편하지는 않지만 어렵지 않게 실행쿼리와 응답속도를 확인할 수 있다.
대부분의 모니터닝 툴이 그렇듯 데이터가 많이 저장되므로 서버의 경우 별도의 디스크가 필수적이다.

코드품질관리는 Sonarqube를 도입했지만 언어 및 룰셋 버전관리가 부담되어 지금은 사용하지 않고 있다. 언제일지 모르지만 회사에서 여유가 조금 생긴다면 다시 도입을 해보고 싶다.

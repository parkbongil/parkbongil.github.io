---
layout: post
title: Spring Boot에 무료 SSL 인증서 적용 및 갱신
tags: [Certbot]
---

Spring Boot의 내장 웹서버와 무료 SSL 인증서를 활용 하다보니 3개월마다 갱신을 해줘야 하는데 자세한 내용은 구글링하면 많이 나와 있으므로 인증서 갱신시 리마인드 하려고 정리해 본다.

### 환경 및 목표

- Spring Boot, YML 사용
- [Oracle Cloud Free Tier](https://www.oracle.com/kr/cloud/free/)에 Ubuntu 20.04.1 LTS 서버
- [Certbot](https://certbot.eff.org/)을 활용한 [Let's Encrypt](https://letsencrypt.org/) 인증서 발급 및 갱신
- Undertow 사용을 위한 jar 배포
- 인증서 갱신시 jar 재배포 없이 p12 파일만 재배포

### Certbot을 활용한 Let's Encrypt 인증서 발급

```
$ sudo apt-get update #apt-get 업데이트
$ sudo apt-get install software-properties-common #선행 소프트웨어 설치
$ sudo add-apt-repository ppa:certbot/certbot #certbot 저장소 추가
$ sudo apt-get update #apt-get 업데이트
$ sudo certbot certonly –standalone -d [도메인명] #Standalone 방식으로 도메인 소유권 확인
```

Standalone 방식으로 도메인 소유권을 확인할 경우 Certbot의 내장 웹서버가 80, 443 포트를 사용하므로 해당 포트 서비스 임시 중지 후 실행  
기본적으로 /etc/letsencrypt/live 디렉토리에 인증서 파일 생성됨(접근시 roor 권한 필요)

### Certbot을 활용한 Let's Encrypt 인증서 확인

```
$ sudo certbot certificates
```

인증서의 도메인, 유료기간, 설치 디렉토리 등을 확인 가능

### pem 인증서를 p12 인증서로 변경

Spring Boot에서 인증서를 적용할 경우 JKS 또는 PCKS12 인증서가 필요하므로 openssl 명령어로 PCKS12 키를 생성

```
# openssl pkcs12 -export -in cert.pem -inkey privkey.pem -out keystore.p12 -name ttp -CAfile chain.pem -caname root
```

생성된 keystore.p12 파일을 java 실행파일(jar) 위치로 이동

### Spring Boot 환경 설정 - application.yml

```
server:
  port: 443 #https 포트
  ssl:
    key-store: file:keystore.p12 #java 실행파일 위치
    key-store-type: PKCS12
    key-store-password: ******** #키 생성시 입력한 비밀번호
```

### Certbot 인증서 갱신

인증서 갱신을 위해 80, 443 포트 서비스 임시 중지

```
$ sudo certbot certificates #인증서 만료일 확인
$ sudo certbot renew --dry-run #인증서 모의 갱신
$ sudo certbot renew #인증서 실제 갱신
```

pem 인증서를 p12 인증서로 변경 및 java 실행파일(jar) 위치로 이동, 서비스 시작

### 참고 - 서비스 시작 및 중지 쉘

서비스 시작

```
#!/bin/sh

sudo nohup java -jar ./baenam-oraclecloud-1.0.jar>/dev/null 2>&1 &
```

서비스 중지

```
#!/bin/sh

pid=`ps -ef | grep baenam-oraclecloud-1.0.jar | grep -v 'grep' | awk '{print $2}'`
sudo kill -9 $pid
```

인증서를 자동으로 갱신하려면 crontab을 활용해서 위의 과정을 쉘로 작성하면 될 듯 한데 당연히 상용 서비스에서는 이렇게 하면 안되므로 귀찮아 지면 적용 해봐야 겠다.  
확인 : [https://oci.baenam.com](https://oci.baenam.com)

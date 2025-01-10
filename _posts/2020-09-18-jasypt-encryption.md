---
layout: post
title: Jasypt를 이용한 환경설정 텍스트 암호화
tags: [Jasypt]
---

소스레벨에서 환경설정 텍스트를 (양방향)암호화 하는 것은 장단점이 있기에 프로젝트마다 충분히 고민하고 진행해야 한다.  
개발환경이 Spring Boot 경우 [jasypt-spring-boot](https://github.com/ulisesbocchio/jasypt-spring-boot) 오픈소스를 이용하면 아주 편하게 적용이 가능하다. 깃헙의 README 파일에 아주 자세히 설명되어 있지만 팁을 포함해서 정리해 본다.

우선 암복호화 테스트를 위해 [Jasypt CLI Tools](https://github.com/jasypt/jasypt)을 다운받는다.(현재버전: 1.9.3)

### 의존성 추가

Maven을 사용한다면 pom.xml에 추가하고 업데이트 한다.

```
<dependency>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
```

의존성만 추가하고 환경설정파일(application.yml 등)에서 평문 대신 ENC(암호화문)으로 대체하면 된다.  
암호화문은 Jasypt CLI Tools을 이용해서 생성한다. 암복호화를 위한 키는 jasypt.encryptor.password이며 실행시에 VM arguments로 값을 넘겨주는 것이 간편하고 안정하게 관리하는 방법일 것이다.

![jasypt-cli-tools](/assets/images/jasypt_cli_tools.png)

참고 - application.yml

```
spring:
  profiles:
    active: local
  datasource:
    driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
    url: ENC(PUOQXQSZ2FflyLh0x/dHuJlLJ5IcWDfTIKYexcgtwP/kEZfJafqes5xqAlRgqbIWVKqgkpGtJOsN0udCXgi8I5+)
    username: ENC(RN5BT3ThApCz5FXq9tE+iV/8eA+iPwweH0Z5/ZDFcvv8fPYsoZzIh9ex9qPK59et)
    password: ENC(l0sBeLEx5SlPrptItkVfVK/pzMPjJJh6SUGv7oASyR0dh2eFd7dxHoArGMEMzoS3)
```

참고 - 어플리케이션 설정 실행

```
java -jar myapplication.jar -Dspring.profiles.active=local -Djasypt.encryptor.password=mykey
```

### 커스텀 Encryptor 사용

Jasypt에서 사용하는 StringEncryptor는 설정되어 있지 않으면 기본값으로 로딩되지만, 소스 가독성과 버전문제 등으로 기본값을 사용하더라도 오버라이딩 해서 사용하는 것이 유지보수에 좋을 것 같다.

```java
@Configuration
public class JasyptConfig {

	@Autowired
	private Environment environment;

	@Bean("jasyptStringEncryptor")
      public StringEncryptor stringEncryptor() {
        PooledPBEStringEncryptor encryptor = new PooledPBEStringEncryptor();
        SimpleStringPBEConfig config = new SimpleStringPBEConfig();
        config.setPassword(environment.getProperty("jasypt.encryptor.password"));
        config.setAlgorithm("PBEWITHHMACSHA512ANDAES_256");
        config.setKeyObtentionIterations("1000");
        config.setPoolSize("1");
        config.setProviderName("SunJCE");
        config.setSaltGeneratorClassName("org.jasypt.salt.RandomSaltGenerator");
        config.setIvGeneratorClassName("org.jasypt.iv.RandomIvGenerator");
        config.setStringOutputType("base64");
        encryptor.setConfig(config);
        return encryptor;
    }
}
```

Jasypt CLI Tools 파일도 프로젝트에 형상관리해서 암복호화가 필요한 경우 바로 사용할 수 있도록 한다.

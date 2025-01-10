---
layout: post
title: Undertow 설정으로 http에서 https로 리다이렉트
tags: [Undertow]
---

Spring Boot의 내장 WAS이면서 핫(?)한 Undertow로 개인프로젝트를 진행하고 있는데 Tomcat과는 다르게 멀티포트 및 SSL 리다이렉트 예제가 그리 많지 않은것 같다. 포트번호는 환경설정 파일에서 설정하고 @Configuration 어노테이션으로 Undertow에 80포트 추가와 리다이렉트를 설정한다.

### application.yml

기본포트를 443으로 설정하고 HTTP/2도 사용할 수 있도록 설정한다. 물론 SSL 인증서를 설정해야 가능한데 [Certbot](https://certbot.eff.org/)에서 무료 인증서를 쉽게 발급 받을 수 있다.

```
http:
  port: 80
server:
  port: 443
  http2:
    enabled: true
  ssl:
    ...
```

### ServletConfig.java

@Configuration 어노테이션으로 환경구성을 할 수 있게 하고 application.yml에서 설정한 포트번호 추가 및 리다이렉트 하도록 설정한다.

```java
@Configuration
public class ServletConfig {

	@Value("${http.port:0}")
	private int httpPort;

	@Value("${server.port:0}")
	private int sslPort;

	@Bean
    public ServletWebServerFactory serverFactory() {
        UndertowServletWebServerFactory factory = new UndertowServletWebServerFactory();

        factory.addBuilderCustomizers((UndertowBuilderCustomizer) builder -> {
            builder.addHttpListener(httpPort, "0.0.0.0");
        });

        factory.addDeploymentInfoCustomizers(deploymentInfo -> {
            deploymentInfo.addSecurityConstraint(
                new SecurityConstraint()
                  .addWebResourceCollection(new WebResourceCollection().addUrlPattern("/*"))
                  .setTransportGuaranteeType(TransportGuaranteeType.CONFIDENTIAL)
                  .setEmptyRoleSemantic(SecurityInfo.EmptyRoleSemantic.PERMIT))
                  .setConfidentialPortManager(exchange -> sslPort);
        });

        return factory;
	}
}
```

기한제한 없이 가상서버호스팅을 무료로 제공하는 [Oracle Cloud Free Tier](https://www.oracle.com/kr/cloud/free/)로 간단히 테스트 해볼 수 있다.  
메인페이지: [http://oci.baenam.com](http://oci.baenam.com)  
DB접속페이지: [http://oci.baenam.com/testdb](http://oci.baenam.com/testdb)

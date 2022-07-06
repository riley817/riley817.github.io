# Spring Framework OAuth2 Provider 서버 구성하기(작성중)


+ spring-security-oauth2 프로젝트는 `deprecated` 되었다고 한다. 나는 그냥 적용했다... 
+ [OAuth 2.0 Migration Guide](https://github.com/spring-projects/spring-security/wiki/OAuth-2.0-Migration-Guide)


## 스프링 멀티모듈 프로젝트 구성 
+ `module-api`를 Resource 서버로 사용하고 특정 API를 요청할때 마다 토큰 인증이 필요하게끔 구성한다.
+ `oauth` 서버에서는 사용자 인증 처리와 토큰 발급을 한다.

```
spring-demo
└─ module-domain : DB 설정, Entity, 공통 Util을 정의.
└─ module-api    : 클라이언트(회원가입, 주문) 비즈니스 로직과 관련된 Controller / Service / Repository 정의. (Resource Server)
└─ oauth  : 스프링 시큐리티 / OAuth2 서버 기능을 정의한 모듈 
```

#### **build.gradle.kts**
+ `JWT` 토큰을 사용하기 위해 `spring-security-jwt` 라이브러리 의존성을 추가 하였다.

```groovy
project("oauth") {
	dependencies {
		implementation(project(":module-domain"))
		implementation("org.springframework.boot:spring-boot-starter-web")

		// OAuth2 라이브러리 관련
		implementation ("org.springframework.boot:spring-boot-starter-security")
		implementation ("org.springframework.security.oauth:spring-security-oauth2:2.1.1.RELEASE")
		implementation ("org.springframework.security:spring-security-jwt:1.0.9.RELEASE")
	}
}
```

## Authorization Server Configuration

### ClientDetailsServiceConfigurer
`ClientDetailsService`는 in-memory 방식과 JDBC 방식으로 선택하여 구현할 수 있다. 나는 회원별로 ClientDetail 정보를 구성할 것이기 때문에 DB에서 가져오는 방식을 선택하였다. \
하지만 운영하다보니 `ClientDetailsService`를 여러번 호출해서 사용하기 때문에 그때마다 DB에 접근하여 조회하는 것은 별로인 것 같아 추후에 `ClientDetailsService`에서 조회하는 정보는 캐시를 적용할 예정이다. 캐시를 적용하기 위해서 나는 `ClientDetailsService`를 재정의 하였다.
+ [DB 스키마](https://github.com/spring-projects/spring-security-oauth/blob/master/spring-security-oauth2/src/test/resources/schema.sql)  


#### PlatformClientDetailService.java
{{<highlight java>}}
@Service
public class PlatformClientDetailService implements ClientDetailsService {

    @Autowired
    private OAuthClientDetailsRepository oAuthClientDetailsRepository;

    @Override
    public ClientDetails loadClientByClientId(String clientId) throws ClientRegistrationException {
        ClientDetails client = oAuthClientDetailsRepository.getOAuthClient(clientId);
        if(client == null) {
            throw new ClientRegistrationException("Unauthorized client");
        }
        return client;
    }
}
{{</highlight>}}

#### OAuthConfiguration.java

{{<highlight java>}}
@Configuration
public class OAuthConfiguration extends AuthorizationServerConfigurerAdapter {

    @Bean("clientDetailService")
    public ClientDetailsService getPlatformClientDetailService() {
        return new PlatformClientDetailService();
    }

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.withClientDetails(getPlatformClientDetailService());
    }
}

{{</highlight>}}

### Configuring Client Details
---





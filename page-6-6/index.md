# [Mastering Spring 5.0] 6.6 Spring Security - OAuth 2.0


{{<admonition type="note" title="스프링 5.0 마스터 스터디">}}
스프링 5.0 마스터 스터디 학습 내용 정리입니다.
{{</admonition>}}

## OAuth2 인증
OAuth 2는 어플리케이션과 Facebook, GitHub 및 DigitalOcean 과 같은 **HTTP 서비스의사용자 계정에 대한 제한된 액세스 권한을 얻을 수있게 해주는 인증 프레임 워크**이다. 이는 사용자 계정을 호스팅하는 서비스에 사용자 인증을 위임하고 타사 응용 프로그램에 사용자 계정에 대한 액세스 권한을 부여하여 작동하게 된다. OAuth 2는 웹 및 데스크톱 응용 프로그램 및 모바일 장치에 대한 인증 흐름을 제공하게 된다.

{{<admonition tip "OAuth2 주체">}}
+ **리소스 소유자 (사용자)** : 리소스 소유자는 자신의 계정에 액세스하기 위해 어플리케이션 을 인증하는 사용자 이다. 응용 프로그램의 사용자 계정의 액세스 권한은 부여 된 권한 (예 : 읽기 또는 쓰기 권한) 의 "범위"로 제한된다.
+ **리소스 서버** : 리소스 서버는 사용자의 계정을 호스트하며 보안 유지가 필요한 리소스가 있는 서버이다.
+ **클라이언트** : 클라이언트는 사용자 계정(사용자 계정을 호스트하는 리소스 서버) 액세스하려는 어플리케이션이다. 
+ **권한 서버** : OAuth 서비스를 제공하며, 사용자의 신원을 인증하며 클라이언트가 리소스 서버에 액세스 할 수 있는 권한을 부여한다.
{{</admonition>}}

{{<figure src="/posts/images/spring/abstract_flow.png">}}

1. 어플리케이션은 사용자에게 리소스 서버 자원에 대한 액세스 권한을 요청한다.
2. 사용자가 액세스 권한을 제공하면 어플리케이션은 권한을 부여 받는다.
3. 어플리케이션은 사용자 권한 부여 및 자체 클라이언트 권한 정보를 권한 서버에 제공한다.
4. 인증에 성공하면 인증을 위한 액세스 토큰을 제공한다.
5. 어플리케이션은 인증을 위해 액세스 토큰을 제공하는 리소스 서버를 호출한다.
6. 액세스 토큰이 유효하면 리소스 서버는 리소스 세부 정보를 반환한다.

## Springboot OAuth 2 인증 구현하기
### 의존성 추가
`spring-security-oauth2` 는 스프링 시큐리티에 OAuth2 지원을 제공하기 위한 모듈이다. `pom.xml` 또는 `build.gradle` 에 추가한다.

**pom.xml**
```xml
<dependency>
    <groupId>org.springframework.security.oauth</groupId>
    <artifactId>spring-security-oauth2</artifactId>
</dependency>

```

**build.gradle**
```groovy
implementation('org.springframework.security.oauth:spring-security-oauth2:2.3.4.RELEASE')
```

### 권한 및 리소스 서버 설정하기
일반적으로 권한 서버와 리소스 서버를 분리하지만, 예제 소스는 권한 서버와 리소스 서버를 동일하게 지정하였다.

```java
@EnableResourceServer
@EnableAuthorizationServer
@SpringBootApplication
public class Chapter06Application {
	public static void main(String[] args) {
		SpringApplication.run(Chapter06Application.class, args);
	}
}
```
- **@EnableResourceServer** : 들어오는 OAuth 2 토큰을 통해 요청을 인증하는 스프링 시큐리티를 사용하는 OAuth2 리소스 서버에 대한 어노테이션이다.
- **@EnableAuthorizationServer** : `DispatcherServlet` 콘텍스트인 현재 어플리케이션 콘텍스트에서 `AuthorizationEndpoint` 및 `TokenEndPoint` 를 사용해 권한 부여 서버를 사용할 수 있도록 하는 어노테이션이다.

### 권한 서버 세부 정보 구성하기
예제 소스에는 `application.properties` 를 통해 세부 구성을 설정을 하였지만, 자동구성 설정이 잘 되지 않아 `@Configuration` 을 통하여 수동 구성으로 작성하였다.

+ [Spring Boot 2.x 버전에서 OAuth2가 안될때](https://hue9010.github.io/spring/OAuth2/)
+ `AuthorizationServerConfigurerAdapter` 를 상속받아 권한서버의 세부 사항을 구성한다.


**/src/main/java/com/mastering/spring/springboot/config/OAuthConfiguration.java**
```java
@Configuration
public class OAuthConfiguration extends AuthorizationServerConfigurerAdapter {

    @Autowired @Qualifier("authenticationManagerBean")
    private AuthenticationManager authenticationManager;

    @Autowired
    private TestUserDetailService clientDetailsService;

    @Override
    public void configure(ClientDetailsServiceConfigurer configurer) throws Exception {
        configurer.inMemory()
                .withClient("clientId")
                .secret("{noop}clientSecret")
                .authorizedGrantTypes("authorization_code", "refresh_token", "password")
                .scopes("openid")
                .authorities("ROLE_MY_CLIENT");
    }

    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        endpoints.tokenServices(getDefaultTokenServices())
                 .authenticationManager(authenticationManager)
                 .userDetailsService(clientDetailsService);
    }

    @Bean
    @Primary
    public DefaultTokenServices getDefaultTokenServices() {
        DefaultTokenServices tokenServices = new DefaultTokenServices();
        tokenServices.setTokenStore(new InMemoryTokenStore());
        return tokenServices;
    }
}
```

+ `void configure(ClientDetailsServiceConfigurer configurer)` : 
클라이언트 ID 와 Secret 자격증명 정보를 메모리에 설정한다. _Spring Security 5.0.0.RC1_ 이후 암호변환정책이 변경되었으므로, 기본값인 `DelegatingPasswordEncoder` 에는 패드워드 암호화 메소드 접두어가 필요하다. **(bcrypt/noop/pbkdf2/scrypt/sha256)** 중 하나를 사용할 수 있으며, 예제는 따로 인코딩을 지정하지 않았기 때문에 {noop} 접두어를 설정함. 
+  `void configure(AuthorizationServerEndpointsConfigurer endpoints)` : /oauth/token 엔드포인트에 대한 상세 서비스를 지정할 수 있다.
+ [spring-security 5.0 에서 달라진 암호변환정책, DelegatingPasswordEncoder](https://java.ihoney.pe.kr/498)
+ [Password Encoding](https://spring.io/blog/2017/11/01/spring-security-5-0-0-rc1-released#password-encoding)

**/src/main/java/com/mastering/spring/springboot/config/WebSecurityConfigurer.java**
```java
@Configuration
public class WebSecurityConfigurer extends WebSecurityConfigurerAdapter {
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```

**/src/main/java/com/mastering/spring/springboot/service/TestUserDetailService.java**
```java
public class TestUserDetailService implements UserDetailsService {
    @Value("${spring.security.user.name}")
    private String myUserName;

    @Value("${spring.security.user.password}")
    private String myUserPassword;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        if(!myUserName.equals(username)) {
            throw new UsernameNotFoundException("UsernameNotFound [" + username + "]");
        }

        PasswordEncoder encoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
        User.UserBuilder userBuilder = User.builder().passwordEncoder(encoder::encode);

        UserDetails user = userBuilder
                .username(myUserName)
                .password(myUserPassword)
                .roles("USER")
                .build();

        return user;
    }
}
```
+ `UserDetailsService` : 실제 DB 나 혹은 사용자 정보를 조회하여 리턴한다. 아래는 `UserDetailsService` 를 사용자 정의로 구현하여, 이전 예제에서 설정한 `application.yml` 에 있는 회원정보를 가져와 검증하도록 작성하였다.

### OAuth 요청 실행
{{<admonition success "API 에 액세스 하려면 다음 2단계 프로세스가 필요하다.">}}
1. 액세스 토큰을 얻는다.
2. 액세스 토큰을 사용해 요청을 실행한다.
{{</admonition>}}

#### 액세스 토큰 얻기
{{<figure src="/posts/images/spring/page6-6-1.png">}}

#### 액세스토큰을 이용한 요청 실행
{{<figure src="/posts/images/spring/page6-6-2.png">}}

### 통합테스트
```java
private OAuth2RestTemplate getOAuthTemplate() {
    ResourceOwnerPasswordResourceDetails resource = new ResourceOwnerPasswordResourceDetails();
    resource.setUsername("user-name");
    resource.setPassword("user-password");
    resource.setAccessTokenUri(createUrl("/oauth/token"));
    resource.setClientId("clientId");
    resource.setClientSecret("clientSecret");
    resource.setGrantType("password");
    OAuth2RestTemplate oauthTemplate = new OAuth2RestTemplate(resource, new DefaultOAuth2ClientContext());
    return oauthTemplate;
}

@Test
public void retrieveTodo() throws Exception {
    String expected = "{id:1,user:Jack,desc:\"Learn Spring MVC\",done:false}";
    ResponseEntity<String> response = getOAuthTemplate().getForEntity(createUrl("/users/Jack/todos/1"), String.class);
    JSONAssert.assertEquals(expected, response.getBody(), false);
}
```
+ `ResourceOwnerPasswordResourceDetails resource = new ResourceOwnerPasswordResourceDetails()` :
사용자 자격증명과 클라이언트 자격증명으로 ResourceOwnerPasswordResourceDetails 설정한다.

+ `resource.setAccessTokenUri(createUrl("/oauth/token"))` : 인증서버의 URL 을 구성한다.

+ `OAuth2RestTemplate oauthTemplate = new OAuth2RestTemplate(resource, new DefaultOAuth2ClientContext())` : 
`OAuth2RestTemplate` 은 OAuth2 프로토콜을 지원하는 `Resttemplate` 의 확장이다.



## 참고
+ https://oauth.net/2/
+ https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2




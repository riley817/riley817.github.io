# [스프링 5.0 마스터] 6.5 Spring Security - Basic authentication


## 스프링 시큐리티로 REST 서비스 보호
최근에는 서비스 시스템들끼리 REST API 기반의 통신이 많이 이루어지고 있다. 네이티브 앱과 서버 간 통신뿐만 아니라 자바스크립트 웹 클라이언트 와 서버간에도 REST API 통신을 많이 사용하기 때문에 REST 서비스(리소스) 에 대한 보안이 중요해 지고 있다. 

#### 보안에서 중요한 기본 개념 - 인증(Authentication) 과 권한 (Authorization)
+ 소비자(클라이언트) 가 서비스(리소스) 에 접근이 가능한 소비자인지 ? **인증(Authentication)**
+ 접근이 가능하지만 해당 작업을 소비자(클라이언트) 에게 허용할것인지 ? **인가/권한부여(Authorization)**

인증방식은 다양하며, 전통적인 인증방식으로는 사용자명(Principle)과 비밀번호(Credential) 로 인증하는 **Credential 기반 인증 방식** 과 OTP 등과 같이 추가적인 인증방식을 도입해 2가지 방법으로 인증하는 **이중 인증 방식** 과 최근에는 **OAuth2 인증방식** 도 필수적으로 사용되고 있다.

스프링에서는 `Spring Security` 를 사용하여 인증과 권한 프로세스 구현할 수 있다. 

## 스프링부트에서 인증방식 구현하기
스프링 시큐리티로 아래 두 가지 타입의 인증을 각각 구현해 본다.
+ **기본인증 (Basic Authentication)**
+ **OAuth 2.0 인증**

### 1. 의존성 추가하기
`spring-boot-starter-security` 의존성을 `pom.xml` 또는 `build.gradle` 에 추가한다. 

#### pom.xml
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
#### build.gradle
```groovy
implementation('org.springframework.boot:spring-boot-starter-security')
```
`spring-boot-starter-security` 는 아래 세가지 의존성을 가져온다.
![dependency](/categories/images/mastering-spring5/page6-5-1.png)

### 2. 기본인증
+ `spring-boot-starter-security` 는 기본적으로 모든 서비스에 대해 기본 인증을 자동으로 설정한다. 
+ 브라우저에서는 기본 로그인 페이지로 이동되거나, REST API 요청 시 응답 코드 401 을 리턴하게 된다. 
+ 기본 인증시 사용자 ID 는 **user** 이며, 패스워드는 보통 스프링 어플리케이션 기동 시 로그에 표시된다.

![defaultpassword](/categories/images/mastering-spring5/page6-5-2.png)

특정 ID 와 패스워드를 설정하려면 `application.properties` (또는 `application.yml`) 에 아래와 같이 구성할 수 있다.

#### application.yml
```yml
spring:
  security:
    user:
      name: devtest
      password: devtest!@#
```

### 3. 통합 테스팅

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Chapter06Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class TodoControllerIT {

    @LocalServerPort
    private int port;

    private String createUrl(String uri) {
        return "http://localhost:" + port + uri;
    }

    private TestRestTemplate template = new TestRestTemplate();

    HttpHeaders headers = createHeaders("devtest", "devtest!@#");

    private HttpHeaders createHeaders(String username, String password) {
        return new HttpHeaders() {
            {
                String auth = username + ":" + password;
                byte[] encodedAuth = Base64.getEncoder().encode(auth.getBytes(Charset.forName("US-ASCII")));

                String authHeader = "Basic " + new String(encodedAuth);
                set("Authorization", authHeader);
            }
        };
    }

    @Test
    public void retrieveTodos() throws Exception {
        String expected = "[{\"id\":1,\"user\":\"Jack\",\"desc\":\"Learn Spring MVC\",\"targetDate\":\"2018-12-26T14:57:05.021+0000\",\"done\":false},{\"id\":2,\"user\":\"Jack\",\"desc\":\"Learn Struts\",\"targetDate\":\"2018-12-26T14:57:05.021+0000\",\"done\":false}]";

        ResponseEntity<String> response = template.exchange(createUrl("/users/Jack/todos"), HttpMethod.GET, new HttpEntity<String>(null, headers), String.class);
        JSONAssert.assertEquals(expected, response.getBody(), false);
    }
}
```
+ HTTP 에서 Basic Authentication 인증은 HTTP 사용자 Agent (ex. 웹 브라우저) 가 요청할 때 사용자 이름과 암호를 제공하는 방법이다. 기본 HTTP 인증 요청 Header 필드에 `Authorization: Basic <credentials>` 형태로 포함된다. 여기서 `credentials` 은 콜론으로 결합 된 ID 및 암호의 base64 인코딩 형태이다.
+ Authorization : https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Authorization

### 4. 단위 테스팅
단위 테스트에 보안을 사용하고 싶지 않다면 `@WebMvcTest` 어노테이션에 `secure=false` 파라미터를 추가한다.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(TodoController.class, secure = false)
public class TodoControllerTest {
```



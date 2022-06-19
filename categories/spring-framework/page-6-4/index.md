# [스프링 5.0 마스터] 6.4 Swagger


## REST 서비스 문서의 자동화
+ REST API 심플하게 설계되면 좋겠지만, 소비자의 요구사항 또는 서비스가 커짐에 따라 API 가 점점 복잡해지고 관리해야할 API 개수도 점점 늘게 된다.
+ 협업을 위해서는 API 는 반드시 문서화 되어야 하는데, 소스 변경사항과 동기화 시키기가 매번 번거롭다.
+ 이러한 문제를 해결하기 위해 REST API 서비스 문서(스펙)을 자동화 하는 툴이 등장하게 되었다.
 
### 주요 API Spec 자동화 라이브러리
+ SLATE : https://github.com/lord/slate
+ Swagger : https://swagger.io/
+ API Blueprint : https://apiblueprint.org/

## Swagger2
**Swagger 2** 는 RESTful API 를 설명하고 문서화하는데 사용되는 오픈소스 라이브러리이다. 특정 언어에 구애 받지 않으며, HTTP 이외에 새로운 기슬과 프로토콜 확장이 가능하다. HTML, Javasript CSS 를 통하여 API 문서를 동적으로 생성할 수 있는 **Swagger UI** 라이브러리가 함께 제공된다. 
Swagger2 Spec 은 몇 가지 구현체가 있는데, Springboot 프로젝트의 경우 `Springfox` 구현체가 주로 사용된다. 

## 스프링부트 프로젝트에 swagger 적용하기
### 1. 의존성 추가
`Springfox` Spec 의 Swagger 와 Swagger UI 를 `pom.xml` 또는 `build.gradle` 에 추가한다.

#### pom.xml
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
    <scope>compile</scope>
</dependency>
```
#### build.gradle
```groovy
implementation('io.springfox:springfox-swagger2:2.9.2')
implementation('io.springfox:springfox-swagger-ui:2.9.2')
```

### 2. 스웨거 프로젝트 설정
#### /src/main/com/mastering/sping/springboot/config/SwaggerConfig.java

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any()).build();
    }   
}
```
+ `@Configuration` : 스프링 구성파일을 정의한다.
+ `@EnableSwagger2` : 스웨거 지원을 가능하게 하는 어노테이션.
+ `Docket` : 스웨서 스프링 MVC 프레임워크를 사용해 스웨거 문서 생성을 설정하는 간단한 Builder Class
+ `new Docket(DocumentationType.SWAGGER_2)` : 스웨거 2를 사용할 스웨거 버전으로 설정한다.
+ `.apis(RequestHandlerSelectors.any()).paths(PathSelectors.any())` : 문서의 모든 API 와 경로를 포함한다.

### 3. 스웨거 API 명세 확인하기
Swagger 설정이 완료되면 서버를 기동 후, http://localhost:8080/v2/api-docs 로 스웨거 API 문서를 시작할 수 있다.

### 4. 스웨거 UI 문서 확인하기
Swagger UI 는 의존성 라이브러리 ``io.springfox:springfox-swagger-ui`` 를 통해 활성화 된다. http://localhost:8080/swagger-ui.html 로 문서를 확인 할 수 있다.

![swaggeruiexample](/categories/images/mastering-spring5/page6-4-1.png)





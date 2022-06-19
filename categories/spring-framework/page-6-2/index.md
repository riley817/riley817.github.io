# [스프링 5.0 마스터] 6.2 HATEOAS


## REST 성숙도 모델 (Richardson Maturity Model)
Richardson Maturity Model 에서는 Restful Web Service 를 다음의 단계로 나누어 성숙도를 정의하고 있다.
![Steps toward REST](https://martinfowler.com/articles/images/richardsonMaturityModel/overview.png)

+ **Level 0** : **원격 프로시저 호출 (Remote Procedure Invocation)** 에 기반한 형태로 resource 구분 없이 설계된 HTTP API
    > ex) http://server/getPosts, http://server/deletePosts, http://server/doThis, http://server/doThat 등

+ **Level 1** : resource 를 URI 통해 나타낸다. (명사 사용) 그러나, HTTP METHOD(GET,POST,PUT,DELETE 등) 사용하지 않는다.
    > ex) http://server/accounts, http://server/accounts/10

+ **Level 2** : resource 를 **URI + HTTP Method** 를 사용하여 접근한다.
    > 계정을 수정하려면 PUT, 계정을 생성하려면 POST 메서드를 수행한다.

+ **Level 3** : HATEOAS. 요청한 정보 뿐만 아니라 요청한 정보에 관련한 URI 를 포함함으로써, 서비스 소비자가 할 수 있는 다음 조치에 대해서도 제공한다. 

## HATEOAS
**HATEOAS (Hypermedia as the Engine of Application State)** 는 RESTful 아키텍쳐를 고유하게 유지하는 REST 응용 프로그램 아키텍쳐의 제약 사항이다. 

**Hypermedia** 라는 용어는 이미지, 텍스트, 동영상 등 다른 형식의 미디어에 대한 링크가 포함된 것을 의미한다. Hypermedia 의 유사한 개념을 RESTful 서비스에도 적용하여, 요청한 리소스에 대한 데이터 뿐만 아니라 **관련 리소스 또는 의존 리소스의 URI 링크** 를 응답에 포함시켜 서비스 소비자에게 제공하는 형태라고 볼 수 있다.

### 기존 RESTful API 의 단점 
+ API 의 엔드포인트가 정해지면 이를 쉽게 변경하기가 어렵다. API 가 변경됨에 따라 이를 사용하는 모든 클라이언트 들이 함께 수정되어야 한다.
+ API 가 수정되어야 하는 경우 API URL 에 버전명을 추가하거나 다른 API 를 지속적으로 추가하게 된다. 그렇게 되면 API URL 관리가 어려워진다.
+ REST API 에 특정 작업을 수행하기 위해 데이터를 수집해야 한다거나, 해당 작업이 가능한지 여부를 판단하는 로직 모두 클라이언트에서 가져가야 한다.

## HATEOAS 스프링부트에 적용하기
### 1. 의존성 추가하기
스프링 부트에는 `spring-boot-starter-hateoas` 라는 **HATEOAS** 를 위한 스타터를 제공한다. 따라서 관련 종속성을 `pom.xml` 또는 `build.gradle` 에 추가한다.

#### pom.xml
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

#### 또는 build.gradle
```groovy
implementation ('org.springframework.boot:spring-boot-starter-hateoas')
```
아래는 `spring-boot-starter-hateoas` 의 중요한 의존성 중 하나는 HATEOAS 기능을 제공하는 `spring-hateoas` 이다.
```xml
<dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <version>2.1.1.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.hateoas</groupId>
      <artifactId>spring-hateoas</artifactId>
      <version>0.25.0.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.plugin</groupId>
      <artifactId>spring-plugin-core</artifactId>
      <version>1.2.0.RELEASE</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
```
### 2. 리소스 링크를 반환하는 컨트롤러 구성
Response 값에 **{name}** 에 관련된 모든 응답을 검색하기 위한 링크를 반환하도록 설정한다. 기존 `ResponseEntity` 대신 `Resource<Todo>` 객체를 리턴하도록 소스를 수정한다.

#### `Resource` class 로 도메인 객체를 wrapping 해주고 link 를 추가할 수 있다.
```java
@GetMapping("/users/{name}/todos/{id}")
public Resource<Todo> retrieveTodo(@PathVariable String name, @PathVariable int id) {
    Todo todo = todoService.retrieveTodo(id);
    if( todo == null ) {
        throw new TodoNotFoundException("Todo Not Found.");
    }

    // Todo 객체에 대한 리소스 객체를 생성한다.
    Resource<Todo> todoResource = new Resource<Todo>(todo);     
    
    // 현재 컨트롤러의 Name 관련한 모든 할일 목록을 조회하는 링크를 parent 항목으로 추가한다.
    ControllerLinkBuilder linkBuilder = linkTo(methodOn(this.getClass()).retrieveTodos(name)); 
    todoResource.add(linkBuilder.withRel("parent"));

    return todoResource;
}
```

#### 3. 응답에 HATEOAS 링크 정보 확인하기
curl 명령어 혹은 POSTMAN 으로 요청한다.
```curl
curl http://localhost:8080/users/Jack/todos/1
```
```json
{
  "id": 1,
  "user": "Jack",
  "desc": "Learn Spring MVC",
  "targetDate": "2018-12-25T07:59:23.073+0000",
  "done": false,
  "_links": {
    "parent": {
      "href": "http://localhost:8080/users/Jack/todos"
    }
  }
}
```
해당 URL 을 요청하면 ``_links`` 키 값에 모든 할일을 조회할 수 있는 링크가 포함된다.

---
**[참고]**
+ Building a Hypermedia-Driven RESTful Web Service - https://spring.io/guides/gs/rest-hateoas/
+ Hypermedia-driven REST API : https://m.blog.naver.com/tmondev/220391644590
+ [한글화 프로젝트] 1. Richardson 성숙도 모델(Richardson Maturity Model) : http://jinson.tistory.com/190
+ On choosing a hypermedia type for your API - HAL, JSON-LD, Collection+JSON, SIREN, Oh My! - https://sookocheff.com/post/api/on-choosing-a-hypermedia-format/

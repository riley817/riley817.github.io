# [Mastering Spring 5.0] 6.3 Bean Validation


{{<admonition type="note" title="스프링 5.0 마스터 스터디">}}
스프링 5.0 마스터 스터디 학습 내용 정리입니다.
{{</admonition>}}

## Bean Validation
**데이터 유효성 검증 (Validation)** 은 모든계층에서 공통적으로 발생하는 작업이다. 만약 모든 계층에서 동일한 내용의 Validation 로직이 각각의 레이어별로 구현되어 있다면 코드 중복과 함께 각 계층별로 중구난방으로 구현된 검증로직간 불일치로 인하여 오류가 발생하기도 쉽다.

{{<image src="/posts/images/spring/application-layers.png#center" width="100%" caption="[출처] https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/images/application-layers.png">}}


이러한 Validation 중복을 피하기 위해 도메인의 검증 로직을 도메인 모델 자체에 묶어서 정의하기도 한다. 하지만 도메인 모델에 실제 코드로 Validation 로직을 표현한다면 도메인 모델 자체가 장황하지고 복잡해지게 된다.
{{<image src="/posts/images/spring/application-layers2.png#center"  width="100%" caption="[출처] https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/images/application-layers2.png">}}

Java 에서는 위와 같은 문제를 해결하기 위해 어노테이션을 통한 Entity 와 Method 를 검증하기 위한 API 를 제공하고 있다.

### Bean Validation
+ **어노테이션을 통해** 객체 모델에 대한 제약 조건을 표현 할 수 있다.
+ 확장 가능한 방식으로 **사용자 정의 제약 조건**을 작성할 수 있다.
+ 개체 및 개체 그래프를 검증하기 위한 API 를 제공한다.
+ 메서드 및 생성자의 매개 변수를 확인하고 반환 값을 리턴하는 API 를 제공한다.
+ **현지화 된 언어**로 위반 사항을 보고한다.

## Hibernate Validator
**Hibernate Validator** 는 **Bean Validation** 명세에 대한 구현체이다. Bean Validation 2.0 에 대한 구현은 Hibernate Validator 6.0.1.Final 이며 Spring Boot 2.0 이상에서 이것을 사용하고 있다.

## 스프링 부트로 Bean Validation 시작하기
### 프로젝트 설정
Hibernate Validator 는 `spring-boot-web-start` 프로젝트의 의존성으로 정의 된다.

+ **org.hibernate.validator:hibernate-validator:6.0.13.Final**
+ **javax.validation:validation-api:2.0.1.Final**

{{<figure src="/posts/images/spring/page6-3-1.png#center">}}

### 컨트롤러 메서드에 Bean Validation 활성화
Controller 메서드의 매개 변수에 `@Valid` 어노테이션을 추가함으로 유혀성 검사를 트리거 할 수 있도록 활성화 시킬 수 있다.
아래의 경우 POST 요청으로 들어온 매개변수를 바인딩 한 뒤 Todo 빈에 정의된 Validation 에 따라 유효성 검증을 하게 된다.

```java
@RequestMapping(method = RequestMethod.POST, path = "/users/{name}/todos")
public ResponseEntity<?> add(@PathVariable String name, @Valid @RequestBody Todo todo) {
  Todo createTodo = todoService.addTodo(name, todo.getDesc(), todo.getTargetDate(), todo.isDone());
  if(createTodo == null) {
      return ResponseEntity.noContent().build();
  }
  URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}").buildAndExpand(createTodo.getId()).toUri();
  return ResponseEntity.created(location).build();
}
```

### 도메인 객체에 Bean Validation 정의
```java
public class Todo {
    private int id;
    @NotNull
    private String user;
    @Size(min = 9, message = "Enter at least 10 Characters.")
    private String desc;
    
    // getter setter
}
```
+ **@NotNull** - user 필드의 값이 비어 있지 않은지 확인한다.
+ **@Size(min = 9, message = "Enter at least 10 Characters.")** - desc 필드 값의 문자가 9 자 이상인지 확인한다.

Bean 을 검증하는 데 사용할 수 있는 어노테이션은 많다. 다음은 몇 가지 Bean Validation 어노테이션이다.

+ **@AssertTrue, @AssertFalse** - 어노테이션 정의 된 필드 값이 true 혹은 false 인지 확인 한다.
+ **@Future** - 어노테이션 정의 필드 된 값이 미래 날짜여야 한다.
+ **@Past** - 어노테이션 정의 필드 값이 과거의 날짜여야 한다.
+ **@Max** -  어노테이션 정의 필드 값이 지정된 최대값 보다 작거나 같은 숫자여야 한다.
+ **@Min** -  어노테이션 정의 필드 값이 지정된 최소값 보다 크거나 같은 숫자여야 한다.
+ **@NotNull** - 어노테이션 정의 필드 값이 null 이면 안된다.
+ **@Pattern** - 어노테이션 정의 필드 값의 `{@code CharSequence}` 요소는 지정된 정규 표현식과 일치해야 한다. 정규표현식은 자바 정규 표현식 규칙을 따른다.
+ **@Size** - 어노테이션 정의 필드 값의 크기는 지정된 범위 내에 있어야 한다.

## 참고

+ beanvalidation.org - https://beanvalidation.org/

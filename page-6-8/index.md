# [Mastering Spring 5.0] 6.8 캐싱


## 캐싱
+ 캐싱은 **많은 시간이나 연산이 필요한 일데 대한 결과를 저장해 두는 것** 이라고 할 수 있다. 
+ 서비스의 데이터 캐싱은 어플리케이션의 성능과 확장성을 향상시키는데 중요한 역할을 한다.
+ 스프링은 어노테이션에 기반을 둔 캐싱 추상화를 제공한다.
+ `JSR-107(JCahce)` 구현체 들은 모두 지원한다.
+ `EhCache`, `Hazelcast`, `Infinispan`, `Couchbase`, `Redis` 등이 기본적으로 자동설정에 포함되어 있다.

## 스프링 부트 프로젝트에 캐싱 적용하기
### 의존성 추가
`spring-boot-starter-cache` 를 `pom.xml` 또는 `build.gradle` 에 추가한다. 이 의존모듈을 추가하면 `JSR-107` 및 스프링 캐싱 어노테이션을 사용하는데 필요한 의존성이 생긴다.

**pom.xml**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**build.gradle**
```groovy
implementation('org.springframework.boot:spring-boot-starter-cache')
```

### 캐싱 활성화

```java
@EnableCaching // 어플리케이션에서 캐싱을 가능하게 설정한다. 
@SpringBootApplication
public class Chapter06Application {
```

### 데이터 캐싱하기

```java
@Cacheable("todos")
public List<Todo> retrieveTodos(String user) {
    List<Todo> filteredTodos = new ArrayList<Todo>();
    for (Todo todo : todos) {
        if (todo.getUser().equals(user)) {
            filteredTodos.add(todo);
        }
    }
    return filteredTodos;
}

@Cacheable(cacheNames = "todos", condition = "#user.length < 10")
public Todo retrieveTodo(int id) {
    for (Todo todo : todos) {
        if (todo.getId() == id) {
            return todo;
        }
    }
    return null;
}
```

+ `@CachePut` : 데이터를 캐시에 명시적으로 추가하는데 사용된다.
+ `@CacheEvict` : 캐시에서 오래된 데이터를 제거하는데 사용된다.
+ `@Caching` : 여러 개의 중첩된 `@Cacheable`, `@Cacheput`, `@CacheEvict` 어노테이션을 동일한 메서드에서 사용할 수 있다.

## JSR-107 캐싱 어노테이션
+ JSR-107 의 목표는 캐싱 어노테이션을 표준화 하는것.
+ 스프링 캐싱 어노테이션과 JSR-107 이 제공하는 기능은 유사하다. 
+ 동일한 프로젝트에서 둘다 사용해서는 안된다.

### 주요 JSR-107 어노테이션

+ `@CacheResult` : @Cacheable 과 유사하다.
+ `@CacheRemove` : @CacheEvit 와 유사하다. @CacheRemove 는 예외가 발생한 경우 조건부 제거를 지원한다.
+ `@CacheRemoveAll` : @CacheEvict 와 유사 (allEntries = true) 캐시에서 모든 항목을 제거하는데 사용된다.




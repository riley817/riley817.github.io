# [자바 ORM 표준 JPA 프로그래밍] 2장 JPA 시작


_김영한님의 책 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리_

## 2.2 H2 데이터베이스 설치
H2DB 는 자바 기반의 오픈소스 관계형 데이터 베이스이다. 별도의 설치과정이 필요하지 않고 용량도 1.7M 로 가볍다. SQL 문법은 다른 DBMS 와 마찬가지로 표준 SQL 이 대부분 지원된다. 

### H2 데이터 베이스 설치방법
아래 링크에서 `zip` 파일을 내려받아 압축을 푼다.

+ [링크](http://www.h2database.com/html/main.html) - http://www.h2database.com/html/main.html

압축을 푼 곳에서 `bin/h2.sh` 를 실행한다. 실행이 완료되면 [http://localhost:8082](http://localhost:8082) 로 접속하면 H2에 접속할 수 있는 화면이 나온다.

![H2DB](/categories/images/jpa/20190625215123.png?width=100px)

```sql
-- 회원테이블을 생성
CREATE TABLE MEMBER (
    ID VARCHAR(255) NOT NULL,   -- 아이디(기본키)
    NAME VARCHAR(255),          -- 이름
    AGE INTEGER NOT NULL,       -- 나이
    PRIMARY KEY (ID)
)
```

## 2.3 라이브러리와 프로젝트 구조
### JPA 구현체인 하이버네이트 사용을 위한 라이브러리

- `hibernate-core` : 하이버네이트 라이브러리
- `hibernate-entitymanager` : 하이버네이트가 JPA 구현체로 동작하도록 JPA 표준을 구현한 라이브러리. 이 라이브러리를 지정하면 **hibernate-core** 와 **hibernate-jpa-2.1-api** 라이브러리도 함께 내려 받는다.
- `hibernate-jpa-2.1-api` : JPA 2.1 표준 API를 모아둔 라이브러리

#### build.gradle
```groovy
dependencies {

    // h2 접속을 위한 라이브러리
    implementation ('com.h2database:h2:1.4.199')
    implementation ('org.hibernate:hibernate-entitymanager:5.4.3.Final')

    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

## 2.4 객체 매핑 

- `@Entity` : 엔티티 클래스를 나타내며 이 클래스를 테이블과 매핑한다고 JPA 에 알려준다.
- `@Table`  : 엔티티 클래스에서 매핑할 테이블 정보를 명시한다. **name** 속성을 사용하여 매핑할 수 있으며, 이 어노테이션을 생략할 경우 클래스 이름을 테이블 이름으로 매핑한다.
- `@Id`     : 엔티티 클래스 필드의 테이블 기본키 `Primary Key` 에 매핑한다. 이렇게 `@Id` 가 사용된 필드를 식별자 필드라 한다.
- `@Column` : 필드를 컬럼에 매핑한다. 
- 매핑정보가 없는 필드 : 매핑 어노테이션을 생략하면 필드명을 사용해서 컬럼명을 매핑한다. 만약 대소문자를 구분하는 데이터베이스를 사용하면 명시적으로 매핑해야만 한다.

```java
package jpabook.start;

// JPA 가 제공하는 매핑 어노테이션을 추가
import javax.persistence.*;

@Entity
@Table(name = "MEMBER")
public class Member {

    @Id
    @Column(name = "ID")
    private String id;          // 아이디

    @Column(name = "NAME")
    private String username;    // 이름

    private Integer age;        // 나이

    // Getter, Setter
    // ...
}
```

## 2.5 persistence.xml 설정
- JPA 는 `persistence.xml` 을 사용해서 필요한 설정정보를 관리한다. 이 설정 파일이 `META-INF/persistence.xml` 클래스 패스 경로에 있으면 별도의 설정없이 JPA 가 인식할 수 있다.
```xml
<persistence-unit name="jpabook">
```
JPA 설정은 영속성 유닛 `persistence-unit` 이라는 것부터 시작하는데 일반적으로 연결할 데이터베이스당 하나의 영속성 유닛을 등록한다.

### 데이터베이스 방언
- JPA 는 특정 데이터베이스에 종속적이지 않은 기술이다.
- 각 데이터베이스는 `데이터 타입`, `제공 함수명`, `페이징처리` 등 제공하는 SQL 문법과 함수가 조금씩 다르다. 
- SQL 표준을 지키지 않거나 특정 데이터베이스만의 고유한 기능을 JPA 에서는 방언`Dialect` 라 한다.
- 하이버에니트를 포함한 대부분의 JPA 구현체들은 다양한 데이터베이스 방언 클래스를 제공한다.

### 각 데이터베이스별 방언클래스
+ [https://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html_single/#configuration-optional-dialects](https://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html_single/#configuration-optional-dialects)

## 2.6 애플리케이션 개발
<script src="https://gist.github.com/stoptheworld99/7ea6ea35fee74c3deed010a4ae191401.js"></script>

### 2.6.1 엔티티 매니저 설정

#### 엔티티 매니저 팩토리 생성
- JPA 를 시작하기 위해서는 `persistence.xml` 설정 정보를 사용하여 엔티티 매니저 팩토리를 생성해야한다. 엔티티 매니저 팩토리는 `persistence.xml`설정 정보를 읽어 JPA 동작시키기 위한 기반 객체를 만들고 JPA 구현체에 따라서 커넥션 풀도 생성한다.
- 엔티티 매니저 팩토리는 생성 비용이 매우크므로 **애플리케이션 전체에서 딱 한 번만 생성하고 공유해서 사용해야 한다.**

#### 엔티티 매니저 생성
- JPA 대부분의 기능을 엔티티 매니저에서 제공한다.
- **엔티티 매니저를 사용해서 엔티티를 데이터베이스에 등록/수정/삭제/조회 할 수 있다.**
- **엔티티 매니저는 데이터베이스 커넥션과 밀접한 관련이 있으므로 스레드 간 공유하거나 재사용하면 안된다.**

#### 종료
- 사용이 끝난 엔티티 매니저와 엔티티 매니저 팩토리는 종료해야 한다.

### 2.6.2 트랜잭션 관리
- JPA 를 사용하면 항상 트랜잭션 안에서 데이터를 변경해야 한다.
- 트랜잭션을 시작하려면 엔티티 매니저 에서 트랜잭션 API 를 받아와야 한다.
- 트랜잭션 API 를 사용하여 비즈니스 로직이 정상 동작하면 트랜잭션을 커밋 `commit` 하고 예외가 발생하면 `rollback` 한다.

```java
 // 트랜잭션 기능 획득
EntityTransaction tx = em.getTransaction();
```

### 2.6.4 JPQL
- JPA 는 SQL 을 추상화한 JSQP 이라는 객체 지향 쿼리 언어를 제공한다.
- JPQL 은 SQL 과 문법이 거의 유사하다. 

#### 차이점
- JPQL 은 **엔티티 객체** 를 대상으로 쿼리한다. 
- SQL 은 **데이터베이스 테이블** 을 대상으로 쿼리한다.

```java

// 목록 조회
TypedQuery<Member> query = em.createQuery(" SELECT m FROM Member m ", Member.class);
List<Member> members = query.getResultList();
```



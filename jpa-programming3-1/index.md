# [자바 ORM 표준 JPA 프로그래밍] 3장 영속성 관리 - EntityManagerFactory, EntityManager, 영속성컨텍스트, 엔티티 생명주기, 영속성컨텍스트의 특징


> 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리
## JPA 가 제공하는 기능
1. 엔티티와 테이블을 매핑하는 설계 부분
2. 매핑하는 엔티티를 실제 사용하는 부분

## 3.1 엔티티 매니저 팩토리와 엔티티 매니저
### EntityManagerFactory

+ `EntityManagerFactory` 는 이름 그대로 `EntityManager` 를 만드는 공장이다.
+ 공장을 만드는 비용은 상당히 크다. 그러므로 한 개만 만들어서 애플리케이션 전체에 공유하도록 설계해야 한다.
+ **`EntityManagerFactory` 는 여러 스레드에서 동시에 접근해도 안전하므로 서로 다른 스레드간 공유해도 된다.**
+ JPA 구현체들은 EntityManagerFactory 생성시 커넥션 풀을 만든다.

```java
// MET-INF/persistence.xml 의 정보를 바탕으로 생성한다.
 EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
```

### EnitityManager
+ **`EntityManager` 의 경우 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드간 절대 공유해서는 안된다.**
+ 엔티티 매니저는 데이터베이스 연결이 꼭 필요한 시점까지 커넥션을 얻지 않는다. (ex. 트랜잭션을 시작할 때 커넥션을 획득한다.)

```java
// 공장에서 엔티티 매니저 생성, 비용이 거의 들지 않는다.
EntityManager em = emf.createEntityManager();
```

## 3.2 영속성 컨텍스트
+ `영속성 컨텍스트 (persistence context)` 는 **엔티티를 영구 저장하는 환경** 이다.
+ 영속성 컨텍스트는 엔티티를 조회, 보관 등 관리하는 곳으로 눈에 보이지 않는 논리적인 개념이다. 
+ 엔티티 매니저를 생성할 때 하나 만들어지며 엔티티 매니저를 통해 영속성 컨텍스트에 접근 및 관리 할 수 있다.

```java
// 엔티티 매니저를 사용해서 회원 엔티티를 영속성 컨텍스트에 저장한다.
 em.persist(member);
```

+ 여러 엔티티 매니저가 같은 영속성 컨텍스트에 접근 할 수도 있다.

## 3.3 엔티티의 생명주기
### 엔티티의 4가지 상태
+ **비영속 (new/transient)** : 영속성 컨텍스트와 전혀 관계가 없는 상태
+ **영속 (managed)** : 영속성 컨텍스트에 저장된 상태
+ **준영속 (detached)** : 영속성 컨텍스트에 저장되었다가 분리된 상태
+ **삭제 (removed)** : 삭제된 상태

![image3-1](/posts/images//jpa/08fig01.jpg?classes=border,shadow)

<p class="image-area"> [그림] 엔티티 생명주기</p>

### 비영속
+ 비영속 상태는 영속성 컨텍스트나 데이터베이스와는 전혀 관련이 없다.

```java

// 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("memberId");
member.setUsername("홍길동");
```

### 영속
+ 엔티티 매니저를 통해 엔티티를 영속성 컨텍스트에 저장한 상태. **영속성 컨텍스트가 관리하는 엔티티를 영속 상태** 라 한다.
+ `em.find()` 나 `JPQL` 을 사용해서 조회한 엔티티도 영속성 컨텍스트가 관리하는 영속 상태다.

```java
// 객체를 저장한 상태(영속)
em.persist(member);
```

### 준영속
+ 영속성 컨텍스트가 관리하던 영속상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 준영속 상태가 된다.
+ 준영속 상태로 만들려면 `em.detach()` 나 `em.close()` 를 호출하여 영속성 컨텍스트를 닫거나 또는  `em.clear()` 영속성 컨텍스트를 초기화 하는 방법이 할때 준영속 상태가 된다.

```java
// 회원 엔티티를 영속성 컨텍스트에서 분리 (준영속 상태)
em.detach(member);
```

### 삭제
+ 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.

```java
// 객체를 삭제한 상태(삭제)
em.remove(member);
```

## 3.4 영속성 컨텍스트의 특징

#### 영속성 컨텍스트와 식별자 값
+ **영속상태는 식별자 값이 반드시 있어야 한다.**
+ 영속성 컨텍스트는 엔티티를 식별자 값(`@Id` 로 테이블의 기본키와 매핑한 값) 으로 구분한다.

#### 영속성 컨텍스트와 데이터 베이스 저장
+ JPA 는 보통 트랜잭션을 commit 하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터 베이스에 반영한다. 이것을 **플러시(flush)** 라고 한다.

#### 영속성 컨텍스트가 엔티티를 관리하면 다음과 같은 장점이 있다.
+ 1차 캐시
+ 동일성 보장
+ 트랜잭션을 지원하는 쓰기 지연
+ 변경 감지
+ 지연 로딩


### 3.4.1 엔티티 조회
+ 영속성 컨텍스트는 내부에 캐시를 가지고 있으며 이것을 1차 캐시라고 한다. **영속 상태의 엔티티는 모두 이곳에 저장된다.**
+ 영속성 컨텍스트 내부에 Map 이 하나 있는데 키는 `@Id` 로 매핑한 식별자고 값은 엔티티 인스턴스 이다. 
+ 1차 캐시의 키는 식별자 값이며 기본적으로 데이터베이스의 기본키와 매핑되어 있다.

#### 영속성 컨텍스트 1차 캐시
+ 회원 엔티티는 아직 데이터베이스에 저장되지 않았다.

```java
// 엔티티를 생성한 상태 (비영속)
Member member = new Member();
member.setId("member1");
member.setUserName("회원"); 

em.persist(member); // 엔티티를 영속
```
![jpa3.7](/posts/images//jpa/jpa3.5.png)
<p class="image-area"></p>


#### 1차 캐시에서 조회
+ `em.find()` 를 호출하면 우선 1차 캐시에서 식별자 값으로 엔티티를 찾는다.

```java
Member findMember = em.find(Member.class, "member1");
```

![jpa3.7](/posts/images//jpa/jpa3.6.png)
<p class="image-area"></p> 


#### 데이터베이스에서 조회
```java
// 1차 캐시에 없는 member2 인스턴스를 조회
Member findMember2 = em.find(Member.class, "member2");
```

+ 만약 `em.find()` 를 호출했는데 엔티티가 1차 캐시에 없으면 엔티티 매니저는 데이터 베이스를 조회하여 엔티티를 생성한다. 그리고 1차 캐시에 저장한 후 반환한다.

![jpa3.7](/posts/images//jpa/jpa3.7.png)

1. `em.find(Member.class, "member2")` 를 실행
2. `member2` 가 1차 캐시에 없으므로 데이터베이스에서 조회
3. 조회한 데이터로 `member2` 엔티티를 생성하여 1차 캐시에 저장 (영속상태)
4. 조회한 엔티티 인스턴스를 반환


#### 영속성 엔티티의 동일성 보장

```java
    Member a = em.find(Member.class, "member1");
    Member b = em.find(Member.class, "member1");

    // 결과값은 true. 
    System.out.println(a == b); 
```
+ `em.find(Member.class, "member1")` 를 반복해서 호출해도 영속성 컨텍스트는 1차 캐시에 있는 같은 엔티티 인스턴스를 반환한다.
+ **영속성 컨텍스트는 성능상 이점과 엔티티의 동일성을 보장한다.**

### 3.4.2 엔티티 등록

```java
    // 엔티티 매니저 생성
    EntityManager em = emf.createEntityManager();

    // 트랜잭션 기능 획득
    EntityTransaction transaction = em.getTransaction();

    transaction.begin();    // 트랜잭션 시작
        
    em.persist(memberA);    
    em.persist(memberB);    
    // 이때까지는 INSERT SQL 을 데이터베이스에 보내지 않는다.
        
    // 커밋하는 순간 INSERT SQL 을 보낸다.
    transaction.commit(); // 트랜잭션 커밋
```

+ 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 내부 쿼리 저장소에 INSERT SQL 을 모아둔다. 그리고 트랜잭션을 커밋할때 모아둔 커밋을 데이터 베이스로 보낸다.
+ 트랜잭션을 커밋할때 데이터베이스에 `flush` 가 되며 이것을 **쓰기 지연**`transactional write-behind` 이라 한다. 
+ `flush` 란 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화 하는 작업이며 이때 등록, 수정, 삭제한 엔티티를 데이터베이스에 반영한다.

#### 트랜잭션을 지원하는 쓰기 지연이 가능한 이유
+ 등록 쿼리를 그때 그때 데이터베이스에 전달해도 트랜잭션을 커밋하지 않으면 데이터베이스에 반영되지 않는다. 
+ 어떻게든 커밋 직전에만 데이터베이스에 SQL 을 전달해도 되기 때문에 쓰기 지연이 가능하다.
+ 내부 쿼리 저장소에 저장한 쿼리를 데이터베이스에 한 번에 전달함으로써 성능을 최적화 할 수 있다.

### 3.4.3 엔티티 수정
+ 엔티티의 변경사항을 감지하여 데이터베이스에 자동으로 반영하는 기능을 변경감지`dirty checking` 이라고 한다.

```java
    // 엔티티 매니저 생성
    EntityManager em = emf.createEntityManager();
    EntityTransaction transaction = em.getTransaction();
    transaction.begin();

    // 영속 엔티티 조회
    Member memberA = em.find(Member.class, "memberA");

    memberA.setUsername("riley");
    memberA.setAge(10);

    // em.update(memberA); // 이런 코드는 필요하지 않다.
    transaction.commit();
```

+ JPA 는 엔티티를 영속성 컨텍스트에 보관할때, 최초 상태를 복사해서 저장해며 이것을 **스냅샷** 이라고 한다.
+ 플러시 시점에 스냅샷과 엔티티를 비교하여 변경된 엔티티를 찾는다.
+ **변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티만 적용된다.**

#### _JPA 를 통하여 모든 필드를 업데이트 했을때_
+ 데이터베이스에 보내는 데이터 전송량이 증가하는 단점이 있다.
+ 모든 필드를 사용하면 수정쿼리가 항상 같다. (물론 바인딩되는 데이터는 다름) 따라서 애플리케이션 로딩 시점에 수정 쿼리를 미리 생성해두고 재사용이 가능하다.
+ 데이터베이스에 동일한 쿼리를 보내면 데이터베이스에서는 이전에 파싱한 쿼리를 재사용한다.

#### _필드가 많거나 저장되는 내용이 너무 크다면_
+ 수정된 데이터만 사용해서 동적으로 UPDATE SQL을 생성하는 전략을 사용한다.

```java

// org.hibernate.annotations.DynamicUpdate
@Entity
@org.hibernate.annotations.DynamicUpdate
@Table(name = "Memeber")
public class Member {...}

```

+ `org.hibernate.annotations.DynamicUpdate` 어노테이션을 사용하면 수정된 데이터만 사용해서 동적으로 UPDATE SQL 을 생성한다.
+ 저장할때 데이터가 존재하는 필드만 INSERT SQL 을 동적으로 생성시에는 `@DynamicInsert` 를 사용한다.

### 3.4.4 엔티티 삭제

```java
// 엔티티를 삭제하려면 먼저 삭제 대상 엔티티를 조회해야 한다.
Member memberA = em.find(Member.class, "memberA");
em.remove(memberA);

```

+ 엔티티 삭제도 엔티티 등록과 비슷하게 삭제 쿼리를 쓰기 지연 데이터베이스에 삭제 쿼리를 전달한다.
+ `em.remove(memberA)` 를 호출하는 순간 memberA 는 영속성 컨텍스트에서 제거된다.

## 이미지 출처
+ http://ptgmedia.pearsoncmg.com/images/chap8_9780131587564/elementLinks/08fig01.jpg


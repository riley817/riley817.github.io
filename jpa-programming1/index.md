# [자바 ORM 표준 JPA 프로그래밍] 1장 JPA 소개



> 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리

## 1.1 SQL을 직접 다룰 때 발생하는 문제점
### 코드의 반복작성

+ 데이터베이스는 객체 구조와는 달리 데이터 중심의 구조를 갖는다.
+ 객체를 데이터베이스에 CRUD 하기 위해서는 너무 많은 SQL 과 JDBC API 를 사용하여 변환작업을 직접 해주어야 한다.

### SQL 에 의존적인 개발
+ 객체들이 어떤 엔티티를 참조하고 있는지 DAO(데이터 접근 계층) 을 열어 SQL 를 확인해야만 한다.
+ SQL 과 JDBC API 를 DAO 에 은닉화하였지만 논리적으로는 엔티티와 아주 강한 의존관계를 가지고 있다.

### JPA 와 문제해결
JPA 를 사용하면 객체를 데이터베이스에 저장하고 관리할 때, 개발자가 직접 SQL 을 작성하는 것이 아닌 JPA 가 제공하는 API 를 사용하면 된다.

#### JPA 저장기능
```java
jpa.persist(member); // 저장
```

#### JPA 조회기능
JPA 는 객체와 매핑정보를 보고 적절한 SELECT SQL 을 생성하여 데이터베이스에 전달하고 그 결과를 Member 객체를 생성하여 반환한다.
```java
String memberId = "id0001";
 Member member = jpa.find(Member.class, memberId); // 조회
```

#### JPA 수정기능
JPA 는 별도의 수정 메소드를 제공하지 않으며, 객체를 조회하여 값을 변경하면 트랜잭션을 커밋할때 데이터베이스에 적절한 UPDATE SQL 구문을 전달한다.
```java
Member member = jpa.find(Member.class, memberId);
 member.setName("이름변경"); // 수정
```

#### 연관된 객체 조회
JPA 는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL 을 실행한다.
```java
Member member = jpa.find(Member.class, memberId);
 Team team = member.getTeam(); // 연관된 객체 조회
```

## 1.2 패러다임의 불일치

+ 관계형 데이터 베이스는 데이터 중심으로 구조화되어 있고, 집합적인 사고를 요구한다. 그리고 객체지향의 추상화, 상속, 다형성 같은 개념이 없다.
+ 객체와 관계형 데이터베이스는 지향하는 목적이 서로 다르며 기능과 표현방법도 다르다. 이것을 `객체와 관계형 데이터베이스의 패러다임 불일치 문제`라 한다.
+ 패러다임 불일치 문제로 객체 구조를 테이블 구조에 저장하는 데에는 한계가 있다.

### 패러다임 불일치 문제들
#### 상속
+ 객체는 상속이라는 기능을 갖고 있지만 테이블은 상속이라는 기능이 없다.
+ 데이터베이스 모델링에서 슈퍼타입 서비타입 관계를 사용하면 객체 상속과 유사한 형태로 테이블을 설계 할 수 있다.

```java
// 객체 모델 코드 
abstract class Item {
    Long id;
    String name;
    int price;
}

class Album extends Item {
    String artist;
}

class Movie extends Item {
    String director;
    String actor;
}

class Book extends Item {
    String author;
    String isbn;
}
```

##### 데이터베이스와 객체모델의 상속 패러다임
+ `Album` 객체를 저장하려면 `Item` 과 `Album` 객체에 대한 INSERT SQL 를 작성해야 하며 슈퍼타입 서브타입 모델링의 경우 부모객체에 자식타입을 저장해야한다.
+ `Album` 조회하려면 `Item` 과 `Album` 을 조인하여 조회 후 그 결과로 `Album` 를 생성해야 한다.
+ **JPA 에서는 자바 컬렉션에 객체를 저장하듯이 JPA 에게 객체를 저장하면된다.**


### 연관관계
#### 객체
- `참조`에 접근하여 연관된 객체를 조회한다.
- 객체는 **참조가 있는 방향으로만 조회** 할 수 있다.

#### 테이블
- `외래키와 조인`을 사용하여 다른 테이블과 연관관계를 가지고 조인을 사용하여 연관된 테이블을 조회한다.

#### 데이터베스와 객체모델의 연관관계 패러다임
- JPA 개발자가 연관관계를 설정하면 JPA 에서 참조를 외래키를 변환하여 데이터베이스에 전달한다.

```java
// 회원과 팀 연관관계를 설정
member.setTime(team);
jpa.persist(member); // 회원과 연관관계 함께 저장

```

### 객체 그래프 탐색
+ SQL 을 직접 다루면 실행하는 SQL 에 따라 객체 그래프를 어디까지 탐색할 수 있는지 정해진다.
+ 모든 연관 객체 그래프를 조회하여 메모르에 올려두는 것 또한 현실성이 없으므로 회원을 조회하는 메소드를 상황에 따라 여러벌 작성해야 한다.

```java
// 객체 그래프를 탐색할 수 있을지 없을 시 코드만 보고는 예측할 수 없다.
class MemberService {
    ...
    public void process () {
        Member member = memberDAO.find(memberId);
        memberId.getTema(); // member->team 객체 그래프 탐색이 가능한가?
        member.getOrder().getDelivery() // ???
    }    
}

// 상황에 따라 여러벌 작성해야 한다.?!!
memberDAO.getMember();  // Member 조회
memberDAO.getMemberWithTeam(); // Member 와 Team 조회
memberDAO.getMemberWithOrderWithDeliver(); // Member 와 Order 와 Delivery 조회

```

#### JPA 와 객체 그래프 탐색
+ JPA 는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL 을 실행한다. 실제 객체를 사용하는 시점까지 조회를 미룬다고 하여 `지연로딩` 이라 한다.
+ JPA 는 지연 로딩을 `투명(transparent)` 하게 처리한다.
+ JPA 는 연관 객체를 즉시 함께 조회할 것인지 사용되는 시점에서 지연하여 조회할지 간단하게 설정할 수 있다.

### 비교
+ `동일성 비교(idnetity)` : == 비교이며, 객체 인스턴스의 참조 값(주소)을 비교한다.
+ `동등성 비교(equals)` : 메서드를 사용하여 객체 내부의 값을 비교한다.

#### JPA 와 비교
- JPA 는 같은 트랜잭션일 때 같은 객체가 조회되는 것을 보장한다.

```java
// JPA 비교
String memberId = "100";
Member member1 = jpa.find(memberId);
Member member2 = jpa.find(memberId);

member1 == member2; // 같다.

```
## 정리
+ 객체 모델과 관계형 데이터베이스 모델은 지향하는 패러다임이 서로 다르다.
+ JPA 는 이러한 패러다임의 불일치 문제를 해결해주고 정교한 객체 모델링을 유지하게 도와준다

## 1.3 JPA 란 무엇인가.
+ **JPA `Java Persistence API`** 는 자바 진영의 ORM 기술 표준이다.
+ **ORM `Object-Relationship Mapping`** 은 객체와 관계형 데이터베이스를 매핑하며, 단순히 CRUD 를 제공하는 것 뿐만아니라 ORM 프레임워크는 _객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결_ 해준다.

### 1.3.1 JPA 소개
+ JPA 는 자바 ORM 기술에 대한 API 표준 명세이다. (쉽게 말하면, 인터페이스를 모아 둔 것)
+ JPA 2.1 을 구현한 구현체로는 `Hibernate`, `EclipseLink`, `DataNucleus` 이며 이중 `Hibernate` 가 가장 대중적이다.

### 1.3.2 JPA 를 사용해야 하는 이유
#### 생산성
+ SQL 을 작성하고 JDBC API 를 사용하는 지루하고 반복적인 CRUD SQL 을 개발자가 직접 작성하지 않아도 된다.

#### 유지보수
+ 필드 추가나 수정 삭제 시에도 개발자가 작성해야 했던 SQL 과 JDBC API 코드를 JPA 가 대신 처리함으로 유지보수 해야하는 코드 수가 줄어든다.
+ 객체 지향 모델의 장점들을 활용하여 유연하고 유지보수하기 좋은 도메인 모델을 설계할 수 있다.

#### 패러다임의 불일치 해결
+ JPA 는 객체와 관계형 모델 간 패러다임의 불일치 (상속, 연관관계, 객체 그래프 탐색, 비교) 문제를 해결

#### 성능
+ JPA 는 어플리케이션과 데이터베이스 사이에 다양한 성능 최적화 기능을 제공한다.
+ 하이버네이트의 경우 SQL 힌트를 넣을 수 있는 기능도 제공한다.

#### 데이터 접근 추상화와 벤더 독립성
+  관계형 데이터베이스는 같은 기능도 벤더마다 사용법이 다른 경우가 존재한다. JPA 는 애플리케이션과 데이터 베이스 사이에 추상화된 데이터 접근 계층을 제공하여, 특정 DBMS 에 종속되지 않도록 한다.

#### 표준
+ JPA 는 자바 진영의 ORM 기술 표준으로, 표준을 사용하면 다른 구현 기술로 손쉽게 변경할 수 있다.



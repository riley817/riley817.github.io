# [자바 ORM 표준 JPA 프로그래밍] 4장 엔티티 매핑


_김영한님의 책 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리_

## JPA 의 매핑 어노테이션
+ **객체와 테이블 매핑** : `@Entity`, `@Table`
+ **기본 키 매핑** : `@Id`
+ **필드와 컬럼 매핑** : `@Column`
+ **연관관계 매핑** : `@ManyToOne`, `@JoinColumn`

## 4.1 @Entity
+ 테이블을 매핑할때 `@Entity` 어노테이션을 필수로 붙어야 한다.

속성  | 기능 | 기본값
--------|------|-------
name | JPA 에서 사용할 엔티티 이름을 지정한다. 보통 기본값인 클래스 이름을 사용한다. | 설정하지 않으면 클래스 이름을 그대로 사용.

### @Entity 적용시 주의할 점
+ 매개변수가 없는 기본 생성자는 필수.
+ `final`, `enum`, `interface`, `inner` 클래스에는 사용할 수 없다.
+ 저장할 필드에 `final` 을 사용하면 안된다.

## 4.2 @Table
+ `@Table` 은 엔티티와 매핑할 테이블을 지정한다.


속성  | 기능 | 기본값
--------|------|-------
name | 매핑할 테이블 이름 | 엔티티 이름을 사용.
catalog | catalog 기능이 있는 데이터베이스에서 catalog 를 매핑 | 
schema | shcema 기능이 있는 데이터베이스에서 schema 를 매핑 |
uniqueConstrains(DDL) | DDL 생성 시 유니크 제약조건을 만든다. 2개 이상의 복합 유니크 제약조건도 만들 수 있다. 이 기능은 스키마 자동생성 기능을 사용해서 DDL 를 만들때만 사용 |

## 4.3 다양한 매핑 사용
{{< gist riley817 2a96911fa2b79ec5da154a77ad7949d3 >}}

## 4.4 데이터베이스 스키마 자동생성
+ JPA 는 데이터베이스 스키마를 자동으로 생성하는 기능을 지원
+ 스키마 자동생성 기능을 사용하려면 `persistence.xml` 에 다음 속성을 추가해야 한다.

```xml
<property name="hibernate.hbm2ddl.auto" value="create" />
```
+ 위 속성을 추가하면 애플리케이션 실행 시점에 데이터베이스 테이블을 자동으로 생성.
+ 자동으로 생성되는 DDL 은 지정한 데이터베이스 방언에 따라 달라진다.

### 장점
+ 개발자가 테이블을 직접 생성하는 수고를 덜 수 있다.

### 주의사항
+ 운영 서버에서 `create`, `create-drop`, `update` 처럼 DDL 을 수정하는 옵션은 절대 사용하면 안됨.

#### [표] hibernate.hbm2ddl.auto 속성
옵션         | 기능
--------------|------
create      | 기존 테이블을 삭제하고 새로 생성. (DROP + CREATE)
create-drop | create 속성에 추가로 어블리케이션을 종료할때 생성한 DDL 제거
update | 데이터베이스를 테이블과 엔티티 매핑정보를 비교해서 변경 사항만 수정
validate | 데이터베이스 테이블과 엔티티 매핑정보를 비교. 차이가 있으면 경고를 남기고 어플리케이션을 실행하지 않음. 이 설정은 DDL 을 수정하지 않는다.
none | 자동생성 기능을 사용하지 않음. (none 은 사실 유요하지 않은 옵션 값)

### 개발 환경에 따른 추천 전략
+ 개발 초기 단계는 create 또는 update
+ 초기화 상태로 자동화된 테스트 진행시 개발자 환경과 CI 서버는 create 또는 create-drop
+ 테스트 서버는 update 또는 validate
+ 스테이징과 운영 서버는 validate 또는 none

### 이름 매핑 전략 변경하기
+ 단어와 단어를 구분시 자바 언어는 관례상 카멜(Camel) 표기법을 사용하고 데이터베이스는 언더스코어(_) 를 주로 사용하기 때문에 엔티티를 매핑하려면 `@Column.name` 속성을 명시해야 함.
+ `hiberante.ejb.naming_strategy` 속성을 사용하면 이름 매핑 전략을 변경 할 수 있다. 하이버네이트에서는 `org.hibernate.cfg.ImprovedNamingStrategy` 클래스를 제공.
+ 위 클래스는 테이블 명이나 컬럼 명이 생략되면 자바의 카멜 표기법을 테이블의 언더스코어 표기법으로 매핑.

```xml
    <property name="hibernate.ejb.naming_strategy" value="org.hibernate.cfg.ImprovedNamingStrategy"/>
```
## 4.5 DDL 생성 기능
+ `@Column` 매핑정보에 nullable 속성 값을 false 로 지정하여 NOT NULL 제약 조건 추가.
+ length 를 지정하면 문자의 크기를 지정할 수 있다.

```java
// Member.java 추가
@Column(name = "NAME", nullable = false, length = 10)
private String username;
```

![image4](/categories/images/jpa/4/20190703205224.png)

### 유니크 제약조건
+ `@Table` 에 `uniqueConstraints` 속성을 지정.
```java
// Member.java
@Entity
@Table(name = "MEMBER", uniqueConstraints = {
        @UniqueConstraint (name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"})
})
public class Member {...}
```
![image4](/categories/images/jpa/4/20190703205343.png)

+ DDL 을 자동생성할 때만 사용하고 JPA 의 실행 로직에는 영향을 주지 않는다.


## 4.6 기본키 매핑
+ 데이터베이스마다 기본키를 생성하는 방식이 다르기때문에 JPA 에서는 기본 키 생성전략을 제공한다.

### JPA 에서 제공하는 데이터베이스 기본 키 생성 전략
+ 직접할당 : 기본 키를 애플리케이션에 직접 입력
+ 자동생성 : 대리 키 사용 방식
    - **IDENTITY** : 기본 키 생성을 데이터베이스에 위임
    - **SEQUENCE** : 데이터베이스 시퀀스를 사용해서 기본키 할당
    - **TABLE** : 키 생성 테이블 사용.

### 주의사항
+ 키 생성 전략을 사용하려면 `persistence.xml` 에 `hibernate.id.new_generator_mapping=true` 속성을 반드시 추가 해야 한다.
+ 기존 하이버네이트 시스템을 유지보수 하는 것이 아니라면 반드시 true 로 설정할것.
+ true 로 설정하면 키 생성을 최적화하는 `allocationSize` 속성을 사용하는 방식이 달라진다.

#### persistence.xml
```xml
<property name="hibernate.id.new_generator_mappings" value="true" /> <!-- 키 생성 최적화 전략 -->
```

### 4.6.1 기본 키 직접 할당 전략
+ 기본키를 직접 할당하려면 `@Id` 로 매핑하면 된다.
+ `@Id` 의 적용 가능 자바 타입
    - 자바 기본형
    - 자바 Wrapper 형
    - String
    - java.util.Date
    - java.sql.Date
    - java.math.BigDecimal
    - java.math.BigInteger

```JAVA
// 기본키직접할당
Board board = new Board();
board.setId("id"); 
em.persistence(board);
```
### 4.6.2 IDENTITY 전략
+ IDENTITY 전략은 기본 키 생성을 데이터베이스에 위임하는 전략이다.
+ 주로 `Mysql`, `PostgreSQL`, `SQL Server`, `DB2` 에서 사용한다.
+ Mysql 의 AUTO_INCREMENT 기능

```JAVA
@Entity
public class Board {
    @Id
    @GeneratedValue(starategy = GenerationType.IDENTITY)
    private Long id;
}
```
+ `@GeneratedValue` 어노테이션을 사용하고 식별자 생성 전략을 명시한다.

### 참고
+ IDENTIY 전략은 데이터베이스에 INSERT 후에 기본 키 값을 조회할 수 있다.
+ JDBC3 의 `Statement.getGeneratedKeys()` 를 사용하면 데이터를 저장하면서 동시에 생성된 키 값도 얻어 올 수 있다.
+ 엔티티가 영속상태가 되려면 반드시 식별자가 필요한데, IDENTITY 전략의 경우 엔티티를 데이터베이스에 저장해야 식별자를 구할 수 있으므로 **이 전략은 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.**

### 4.6.3 SEQUENCE 전략
+ 데이터베이스의 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터 베이스 오브젝트이다.

```SQL
-- SEQUENCE DDL
CREATE TABLE BOARD (
    ID BIGINT NOT NULL PRIMARY KEY,
    DATE VARCHAR(255)
) 

-- 시퀀스 생성
CREATE SEQUENCE BOARD_SEQ START WITH 1 INCREMENT BY 1;
```
{{< gist riley817 9b3e2d535bb024448a6fdddaf5f2a69a >}}

- SEQUENCE 전략은 em.persist() 를 호출할 때 먼저 데이터베이스 시퀀스를 사용해 식별자를 조회한다. 
- 조회한 식별자를 엔티티에 할당한 후 엔티티를 영속성 컨텍스트에 저장
- 트랜잭션 커밋해서 플러시가 일어나면 엔티티를 데이터베이스에 저장

### @SequenceGenerator
속성  | 기능 | 기본값
--------|------|-------
name | 식별자 생성기 이름 | 필수
sequenceName | 데이터베이스에 등록되어 있는 시퀀스 이름 | hibernate_sequence
initialValue | DDL 생성 시에만 사용. 시퀀스 DDL 을 생성할 때 처음 시작하는 수를 지정 | 1
allocationSize | 시퀀스 한 번 호출에 증가하는 수 (성능 최적화에 사용) | 50
catalog, schema | 데이터베이스 catalog, schema 이름

### SequenceGenerator.allocationSize
+ SequenceGenerator.allocationSize 기본값이 50 이다.
+ JPA 는 **시퀀스에 접근하는 횟수를 줄이기 위해 allocationSize 사용**한다.
+ 여기에서 설정한 값만큼 한 번에 시퀀스 값을 증가시키고 나서 그만큼 메모리에 시퀀스 값을 할당한다.
+ 이방법은 시퀀스 값을 선점하므로 JVM 이 동시에 접근해서 데이터를 등록시 시퀀스 값이 한번에 증가하는 점을 염두.
+ INSERT 성능이 중요하지 않거나 시퀀스가 많이 증가하는 게 부담될 경우 allocationSize 를 1로 설정한다. 


### 4.6.4 TABLE 전략
+ TABLE 전략은 키 생성 전용 테이블을 하나 만들고 이름과 값으로 사용할 컬럼을 만들어 데이터 시퀀스처럼 사용하는 전략이다.
+ 이 전략은 모든 데이터베이스에 적용이 가능하다.

{{< highlight sql >}}
-- TABLE 전략 시퀀스 테이블 DDL
create table MY_SEQUENCES (
  sequence_name varchar(255) not null,
  next_val bigint,
  primary key (sequence_name)
)
{{< /highlight >}}
{{< gist riley817 39fa0c8f362081ea2f3c89446c62b702>}}

### @TableGenerator

속성  | 기능 | 기본값
--------|------|-------
name | 식별자 생성기 이름 | 필수
table | 키생성 테이블명 | hibernate_sequence
pkColumnName | 시퀀스 컬럼명 | sequence_name
valueColumnName | 시퀀스 값 컬럼명 | next_val
initialValue | 초기 값, 마지막으로 생성된 값이 기준이다 | 0
allocationSize | 시퀀스 한 번 호출에 증가하는 수 (성능최적화 이용) | 50
catalog, schema | 데이터베이스 catalog, schema 이름 |
uniqueConstrains(DDL) | 유니크 제약 조건을 지정할 수 있다. 

### 4.6.5 AUTO 전략
+ `GenerationType.AUTO` 는 선택한 데이터베이스 방언에 따라 `IDENTITY`, `SEQUENCE`, `TABLE` 전략 중 하나를 자동으로 선택.
+ AUTO 는 데이터베이스를 변경해도 코드를 수정할 필요가 없는 장점이 있다.

### 4.6.6 기본 키 매핑 정리
+ 영속성 컨텍스트는 엔티티를 식별자 값으로 구분하므로 반드시 식별자 값이 있어야 한다.

#### 권장하는 식별자 전략
1. null 값은 허용하지 않는다.
2. 유일해야 한다.
3. 변해서는 안된다.

#### 테이블의 기본 키를 선택하는 전략은 크게 2 가지가 있다.
+ **자연키 (natural key)** : 비즈니스에 의미가 있는 키. ex) 주민등록번호, 이메일, 전화번호
+ **대리키 (surrogate key)** : 비즈니스와 관련없는 임의로 만들어진 키. ex) 오라클 시퀀스, auto_increment, 키생성 테이블 
+ 자연키보다는 대리 키를 권장한다.

## 4.7 필드와 컬럼 매핑 : 레퍼런스
### 4.7.1 @Column 
+ `@Column` 은 객체 필드를 테이블 컬럼에 매핑한다. 
+ 자바 기본 타입에 `@Column` 을 사용하면, `nullable=false` 로 지정하는 것이 안전하다.

속성  | 기능 | 기본값
--------|------|-------
name | 필드와 매핑할 테이블의 컬럼 이름 | 객체의 필드 이름
nullable(DDL) | null 값의 허용 여부 설정. false 로 설정시 not null 제약조건 | true
unique(DDL) | 한 컬럼에 간단한 유니크 제약조건을 걸 때 사용 
columnDefinition(DDL) | 데이터베이스 컬럼 정보를 직접 줄 수 있다.
length(DDL) | 문자 길이 제약조건, String 타입에만 사용 | 255
precision, scale(DDL) | BigDecimal/BigInteger 타입에서 사용. precision 은 소수점을 포함한 전체 자리수, scale 은 소수의 자리수 | precision = 19, scale = 2

### 4.7.2 @Enumerated
+ 자바의 `enum` 타입을 매핑할 때 사용한다.
+ `EnumType.ORDINAL` : enum 에 정의된 순서대로 데이터베이스에 저장된다.
    - 장점 : 저장되는 데이터 크기가 작다.
    - 단점 : 이미 저장된 enum 순서를 변경할 수 없다.
+ `EnumType.STRING` : enum 이름을 데이터베이스에 저장. (권장)
    - 장점 : 저장된 enum 순서가 바뀌거나 enum이 추가되어도 안전
    - 단점 : 데이터베이스에 저장되는 크기가 ORINAL 에 비해 크다.

### 4.7.3 @Temporal
+ 날짜 타입 (`java.util.Date`, `java.util.Calendar`) 을 매핑할 때 사용.
+ `TemporalType.DATE`, `TemporalType.TIME`, `TemporalType.TIMESTAMP`

### 4.7.4 @Lob
+ 데이터베이스의 BLOB, CLOB 타입과 매핑된다.
+ `@Lob` 에는 속성을 지정할 수 없으므로, 매핑하는 필드 타입이 문자면 CLOB, 나머지는 BLOB 으로 매핑한다.
{{<highlight java>}}
  @Lob
  private String lobString; // CLOB

  @Lob
  private byte[] lobByte; // BLOB
{{</highlight>}}
+ **CLOB** : String, char[], java.sql.CLOB
+ **BLOB** : byte[], java.sql.BLOB

### 4.7.5 @Transient
+ 데이터베이스에 저장하지도 조회하지도 않는다. 객체에 어떤 값을 임시로 보관하고 싶을 때 사용한다.

### 4.7.6 @Access
+ JPA 가 엔티티 데이터에 접근하는 방식을 지정
+ **필드접근** : `AccessType.FIELD` 로 지정한다. 필드에 직접 접근하며, 필드 접근 권한이 private 이어도 접근 가능하다.
+ **프로퍼티 접근** : `AccessType.PROPERTY` 로 지정. 접근자를 사용한다.

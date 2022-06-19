# [자바 ORM 표준 JPA 프로그래밍] 5장 연관관계 매핑 기초 - 단방향 연관관계


_김영한님의 책 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리_

{{< gist riley817 e1a4f8c19ec50637e9363bbf76b59450>}}

# 단방향 연관관계
+ 객체는 참조(주소) 를 사용해서 관계를 맺고, 테이블을 외래 키를 사용해서 관계를 맺는다.
+ 방향(`Direction`) : 단방향과 양방향이 있으며 **방향은 객체관계에만 존재하고 테이블 관계는 항상 양방향** 이다.
+ 다중성(`Multiplicity`) : 다대일(N:1), 일대다(1:N), 일대일(1:1), 다대다 (N:N)
+ 연관관계 주인 (`owner`) : 객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해야 한다.

## 5.1 단방향 연관관계
### 객체 연관관계
+ 회원 객체와 팀 객체는 **단방향 관계** 다. 
+ 회원은 Member.team 필드를 통해서 팀을 알 수 있지만 반대로 팀은 회원을 알 수 없다.

### 테이블 연관관계
+ 회원 테이블과 팀 테이블은 **양방향 관계** 다.
+ 회원 테이블의 TEAM_ID 외래키를 통해 회원팀과 조인 할 수 있고 반대로 팀과 회원도 조인할 수 있다.

```sql
SELECT * FROM MEMBER M JOIN TEAM T ON M.TEAM_ID = T.ID;
SELECT * FROM TEAM T JOIN MEMBER M ON T.ID = M.TEAM_ID;
```

### 정리
+ 참조를 사용하는 객체의 연관관계는 단방향이다. `A -> B (a.b)`
+ 외래키를 사용하는 테이블의 연관관계는 양방향이다. `A JOIN B` 도 가능 하고 `B JOIN A` 도 가능하다.
+ 객체를 양항뱡으로 참조하려면 단방향 연관관계를 2개 만들어야 한다.
    - `A->B (a.b)` 
    - `B->A (b.a)`

### 5.1.1 순수한 객체 연관관계
+ **객체그래프** : 객체는 참조를 사용해서 연관관계를 탐색할 수 있다.

```java
// 객체그래프 탐색 동작코드
public static void main(String[] args) {

    Member member1 = new Member ("member1", "회원1");    
    Member member2 = new Member ("member2", "회원2");    
    Team team1 = new Team ("team1", "팀1");

    member1.setTeam(team1);
    member2.setTeam(team1);

    // 참조를 사용하여 연관관계를 탐색한다.
    Team findTeam = member1.getTeam();
}
```

### 5.1.2 테이블 연관관계
+ **조인** : 데이터베이스는 외래 키를 사용해서 연관관계를 탐색할 수 있다.

### 테이블 생성
```sql
-- MEMBER
CREATE TABLE MEMBER (
    MEMBER_ID VARCHAR(255) NOT NULL,
    TEAM_ID VARCHAR(255),
    USERNAME VARCHAR(255),
    PRIMARY KEY (MEMBER_ID)
);

-- TEAM
CREATE TABLE TEAM(
    TEAM_ID VARCHAR(255) NOT NULL,
    NAME VARCHAR(255),
    PRIMARY KEY (TEAM_ID)
);

ALTER TABLE MEMBER ADD CONSTRAINT FK_MEMBER_TEAM
    FOREIGN KEY (TEAM_ID)
    REFERENCES TEAM
;
```

#### SQL 을 통하여 연관관계를 정의하기
```sql
INSERT INTO TEAM (TEAM_ID, NAME) VALUES  ('team1', '팀1');
INSERT INTO MEMBER (MEMBER_ID, TEAM_ID, USERNAME) VALUSE ('member1', 'team1', '회원1');
INSERT INTO MEMBER (MEMBER_ID, TEAM_ID, USERNAME) VALUSE ('member2', 'team1', '회원2');
```

### 5.1.3 JPA 로 객체 관계매핑

#### Member.java
```java
@Entity
public class Member {
    
    @Id
    @Column(name = "MEMBER_ID")
    private String id;
    
    private String username;
    
    // 연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    
    // 연관관계 설정
    public void setTeam(Team team) {
        this.team = team;
    }
    // Getter, Setter...
}
```

#### Team.java
```java
@Entity
public class Team {
    @Id
    @Column(name = "TEAM_ID")
    private String id;
    
    private String name;
    
    // Getter, Setter ...
}
```
+ `@ManyToOne` : **다대일(N:1) 관계 매핑 정보를 나타낸다.** 회원객체를 기준으로 팀객체와는 다대일 관계.
+ `@JoinColumn(name = "TEAM_ID")` : 조인 컬럼은 외래키를 매핑할 때 사용.


### 5.1.4 @JoinColumn
속성  | 기능 | 기본값
--------|------|-------
name | 매핑할 외래 키 이름  | 필드명 + _ + 참조하는 테이블의 기본키 컬럼명
referencedColumnName | 외래 키가 참조하는 대상 테이블의 컬럼명 | 참조하는 테이블의 키 컬럼명
foreignKey(DDL) | 테이블 생성시 사용. 외래키를 직접 지정 가능 |
unique, nullable, inserable, updateable, columnDefinition, table| @Column 속성과 동일 

### 5.1.5 @ ManyToOne
속성  | 기능 | 기본값
--------|------|-------
optional | false 로 설정시 연관된 엔티티가 항상 있어야 한다. | true
fetch | 글로벌 페치 전략 | @ManytoOne = FetchType.EAGER, @OneToMany=FetchType.LAZY
cascade | 영속성 전이 기능 사용 | 
targetEntity | 연관된 엔티티 타입 정보를 설정. (이 기능은 거의 사용 안함) | 

```java
@OneToMany
private List<Member> members; // 제네릭타입으로 정보를 알 수 있다.
```

## 5.2 연관관계 사용
### 5.2.1 저장
```java
public static void testSave(EntityManager em) {

    // 팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    // 회원1 저장
    Member member1 = new Member("member1", "회원1");
    member1.setTeam(team1); // 연관관계 설정 member1 -> team1
    em.persist(member1);

    // 회원2 저장
    Member member2 = new Member("member2", "회원2");
    member2.setTeam(team1);
    em.persist(member2);
}
```
+ JPA 는 참조한 팀의 식별자 (`Team.id`) 를 외래 키로 사용해서 적절한 등록 쿼리를 생성

![image5](/categories/images/jpa/5/20190721213200.png)

### 5.2.2 조회
#### 연관관계가 있는 엔티티를 조회하는 방법
+ 객체 그래프 탐색 (객체 연관관계를 사용한 조회) 
+ 객체 지향 쿼리 사용 (`JPQL`)


#### 객체그래프 탐색
+ `member.getTeam()` 을 사용하여 Member 와 연관된 Team 객체를 조회할 수 있다.

```java
Member member = em.find(Member.class, "member1");
Team team = member.getTeam(); // 객체 그래프 탐색
```

#### 객체지향 쿼리 사용
```java
public static void queryLogicJoin(EntityManager em) {
    String jpql = " SELECT m from Member m JOIN m.team t WHERE t.name = :teamName";

    List<Member> resultList = em.createQuery(jpql, Member.class)
            .setParameter("teamName", "팀1")
            .getResultList();

    for (Member member : resultList) {
        System.out.println("[query] member.username = " + member.getUsername());
    }
}
```

### 5.2.3 수정
+ 트랜잭션을 커밋할 때 플러시가 일어나면서 변경 감지 기능이 작동한다. 이것은 연관관계를 수정할 때도 동일하며, 참조하는 대상만 변경하면 나머지는 JPA 에서 자동으로 처리한다.

```java
public static void updateRelation(EntityManager em) {
        // 새로운 팀
        Team team2 = new Team("team2", "팀2");
        em.persist(team2);

        // 회원1 을 팀2로 설정한다.
        Member member = em.find(Member.class, "member1");
        member.setTeam(team2); // 플러시 발생시 자동으로 업데이트
}
```

### 5.2.4 연관관계 제거
```java
private static void deleteRelation(EntityManager em) {
    Member member1 = em.find(Member.class, "member1");
    member1.setTeam(null);
}
```

### 5.2.5 연관된 엔티티 삭제
+ 연관된 엔티티를 삭제하려면 기존에 있던 연관관계를 먼저 제거하고 삭제해야 한다.

```java
member1.setTeam(null); // 회원1 연관관계 제거
member2.setTeam(null); // 회원2 연관관계 제거
em.remove(team);       // 팀 삭제
```

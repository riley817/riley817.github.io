# [자바 ORM 표준 JPA 프로그래밍] 6장 다양한 연관관계 매핑 - 일대다

_김영한님의 책 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리_

## 6.2 일대다
+ 일대다 관계는 엔티티를 하나 이상 참조할 수 있으므로 자바 컬렉션 (`Collection, List, Set, Map `) 중 하나를 사용한다.

### 6.2.1 일대다 단방향 [1:N]
+ 일대다 단방향 관계는 JPA 2.0 부터 지원한다.
+ 일대다 단방향의 경우 반대편에서 테이블의 외래키를 관리하는 특이한 모습이 나타난다.

#### Team.java
```java
@Entity
public class Team {
    
    @Id @GeneratedValue
    @Column(name = "TEAM_ID")
    private String id;
    
    private String name;
    
    @OneToMany
    @JoinColumn (name = "TEAM_ID") // MEMBER 테이블의 TEAM_ID (FK)
    private List<Member> members = new ArrayList<Member>();

}
```

#### Member.java
```java
@Entity
public class Member {
    
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    private String username;
    
    // Getter, Setter...
}
```
+ 일대다 단방향 관계를 매핑할때는 `@JoinColumn` 을 명시.

### 일대다 단방향 매핑의 단점
+ 매핑한 객체가 관리하는 외래 키가 다른 테이블에 있다는 점.

```java
public void testSave() {

    Member member1 = new Member("member1");
    Member member2 = new Member("member2");

    Team team1 = new Tema("team1");
    team1.getMembers().add(member1);
    team2.getMembers().add(member2);

    em.persist(member1);
    em.persist(member2);

    em.persist(team1);
}
```

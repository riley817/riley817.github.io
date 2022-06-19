# [자바 ORM 표준 JPA 프로그래밍] 5장 연관관계 매핑 기초 - 양방향 연관관계



_김영한님의 책 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리_
{{< gist riley817 e1a4f8c19ec50637e9363bbf76b59450>}}

<br/>

## 5.3 양방향 연관관계
+ 일대다 관계에서는 여러 건과 연관관계를 맺을 수 있으므로 컬렉션(`Collection`, `Set`, `Map`, `List` ..) 을 사용한다.

### 5.3.1 양방향 연관관계 매핑
#### Member.java 
+ 회원 엔티티에는 변경할 사항이 없다.

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
    
    @OneToMany (mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();

    // Getter, Setter ...
}
```
+ 팀과 회원은 1:N 이며 해당 다중성을 매핑하기 위하여 `@OneToMany` 매핑 정보를 사용한다. 또한 `mappedBy` 속성은 양방향 매핑일 때 사용하며, 반대쪽 매핑되는 필드의 이름을 값으로 설정하면 된다.

### 5.3.2 일대다 컬렉션 조회
```java
public static void biDirection(EntityManager em) {

    Team team = em.find(Team.class, "team1");
    List<Member> members = team.getMembers();   // 팀 -> 회원 방향으로 객체 그래프를 탐색한다.

    for (Member member : members) {
        System.out.println("member.username : " + member.getUsername());
    }
}
```

## 5.4 연관관계 주인
+ 테이블은 외래 키 하나로 두 테이블의 연관관계를 관리하지만, 객체의 경우 엔티티를 양방향으로 매핑하기 위해서는 객체의 참조를 통해서 서로 참조해야만 한다. (ex 회원->팀, 팀->회원)
+ 엔티티를 양방향 연관관계로 설정하면 객체 참조는 둘인데 외래키는 하나이므로 둘 사이에 차이가 발생
+ **두 객체 연관관계 중 하나를 정해서 테이블의 외래키를 관리**해야 하는데 이것을 연관관계의 주인 이라고 한다.

### 5.4.1 양방향 매핑의 규칙 : 연관관계의 주인
+ 연관관계의 주인만이 데이터베이스 연관관계와 매핑되고 외래 키를 관리(등록, 수정, 삭제) 할 수 있다. 주인이 아닌 쪽은 읽기만 가능하다.
+ 주인은 `mappedBy` 속성을 사용하지 않는다.
+ 연관관계의 주인을 정한다는 것 -> **외래키 관리자를 선택하는 것**

### 5.4.2 연관관계의 주인은 외래 키가 있는 곳
+ 연관관계의 주인만 데이터베이스 연관관계와 매핑되며, 외래키를 관리할 수 있다.
+ 주인이 아닌 반대편 (`inverse`, `non-owning side`) 은 읽기만 가능 외래키를 변경하지는 못한다.

> N:1, 1:N 에서는 항상 N 쪽이 외래 키를 갖는다. `@ManyToOne` 은 항상 연관관계의 주인이 되므로, mappedBy 속성이 존재하지 않는다.

## 5.6 양방향 연관관계의 주의점
+ 연관관계의 주인에는 값을 입력하지 않고, 주인이 아닌곳에만 입력하는 실수를 유의해야 한다.

```java
public static void testSave(EntityManager em) {

    // 회원1 저장
    Member member1 = new Member("member1", "회원1");
    em.persist(member1);

    // 회원2 저장
    Member member2 = new Member("member2", "회원2");
    em.persist(member2);

    // 팀1 저장
    Team team1 = new Team("team1", "팀1");

    // 주인이 아닌곳에 연관관계 설정
    team1.getMembers().add(member1);
    team1.getMembers().add(member2);

    em.persist(team1);
}
```
+ 예제코드에서 Member 가 연관관계의 주인인데 Member.team 의 연관관계에 대해서 설정하지 않았다. 따라서 TEAM_ID 외래키의 값도 null 이 저장된다.

### 5.6.1 순수한 객체까지 고려한 양방향 연관관계
+ **객체 관점에서 양쪽 방향에 참조 값을 입력해 주는 것이 가장 안전하다.**
+ `Member.team` : 연관관계의 주인. 이 값으로 외래키를 관리
+ `Team.members` : 연관관계 주인 아님. 따라서 저장시에는 사용되지 않음.

+ **결론** : 객체의 양방향 연관관계는 양쪽 모두 관계를 맺어주자.

```java
// 양방향 연관관계 사용하여 저장
public static void testORM_양방향(EntityManager em) {

    Team team2 = new Team("team2", "팀2");
    em.persist(team2);

    Member member3 = new Member("member3", "회원3");

    member3.setTeam(team2);
    team2.getMembers().add(member3);    // 연관관계 설정 member1 -> team2
    em.persist(member3);                // 연관관계 설정 team2 -> member1

    Member member4 = new Member("member4", "회원4");

    member4.setTeam(team2);             // 연관관계 설정 member4 -> team2
    team2.getMembers().add(member4);    // 연관관계 설정 team2 -> member4
    em.persist(member4);

}
```

### 5.6.2 연관관계 편의 메소드
+ 양방향 연관관계는 양쪽 다 신경 써야 한다. 그러나 각각 호출하다보면 실수로 둘 중 하나만 호출해서 양방향이 깨질 수 있다.
+ `setTeam()` 메소드 하나로 양방향 모두를 설정하도록 변경한다.
+ 한 번에 양항뱡 관계를 설정하는 메소드를 **연관관계 편의 메소드**라고 한다.

```java
// Member.java 의 setTema() 메소드 수정
public void setTeamNew(Team team) {
    this.team = team;
    team.getMembers().add(this);
}
```

```java
public static void testORM_양방향_리펙토링(EntityManager em) {

    Team team3 = new Team("team3", "팀3");
    em.persist(team3);

    Member member5 = new Member("member5", "회원5");
    member5.setTeam(team3);
    em.persist(member5);

    Member member6 = new Member("member6", "회원6");
    member6.setTeam(team3);
    em.persist(member6);
}
```

### 5.6.3 연관관계 편의 메소드 작성 시 주의 사항
+ 연관관계를 변경할 경우 기존 연관관계를 삭제하는 코드를 추가해야 한다.
+ 객체에서 양방향 연관관계를 사용하려면 로직을 견고하게 작성해야 한다.

```java
// 연관관계 편의 메소드
public void setTeam(Team team) {

    // 기존 팀과 관계를 제거
    if(this.team != null) {
        this.team.getMembers().remove(this);
    }

    this.team = team;
    team.getMembers().add(this);
}
```

## 5.7 정리
+ 단방향 매핑과 비교했을때 양방향 매핑은 복잡하고, 연관관계의 주인도 정해야하고 단방향 연관관계를 양방향으로 만들기 위해 로직도 잘 관리해야한다.
+ 양방향의 장점은 반대방향으로 객체 그래프 탐색 기능이 추가 된것 뿐이다.
+ 양방향 연관관계를 매핑하려면 객체에서 양쪽 방향을 모두 관리해야한다.

## 연관관계의 주인을 정하는 기준
+ 비즈니스 로직의 중요도가 크다고 해서 무조건 연관관계의 주인이 되는 것은 아니다.
+ 비즈니스 중요도를 배제하고 단순히 외래 키 관리자 정도의 의미만 부여해야 한다.
+ 연관관계의 주인은 외래키의 위치와 관련해서 정해야 한다.



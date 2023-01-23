# [자바 ORM 표준 JPA 프로그래밍] 6장 다양한 연관관계 매핑 - 다대일


> 자바 ORM 표준 JPA 프로그래밍 학습 내용 정리

## 다양한 연관관계 다루기

### 엔티티의 연관관계를 매핑할때 고려할점
+ 다중성
+ 단방향, 양방향
+ 연관관계의 주인

### 다중성
+ `다대일(@ManyToOne)`, `일대다(@OneToMany)`, `일대일(@OneToOne)`, `다대다(@ManyToMany)`
+ 다중성을 판단하기 어려울 때는 반대방향을 생각해보자.
+ 보통 다대일과 일대다 관계를 가장 많이 사용하고 다대다 관계는 실무에서는 거의 사용하지 않음.

### 단방향, 양방향
+ 객체 관계에서 한쪽만 참고하는 것을 단방향 관계라고하며, 양쪽이 서로 참조하는 것을 양방향 관계라 한다.

### 연관관계의 주인
+ 외래 키를 가진 테이블과 매핑한 엔티티가 외래키를 관리하는게 효율적이므로 보통 이곳을 연관관계의 주인으로 선택한다.
+ 연관관계의 주인이 아니면 `mappedBy` 속성을 사용하며, 연관관계 주인의 필드 이름을 값으로 입력한다.

## 6.1 다대일
+ 다대일 관계에서 **외래키는 항상 다(N) 쪽**에 있다.
+ 객체 양방향 관계에서 연관관계의 주인은 항상 다쪽이다.

### 6.1.1 다대일 단방향 [N:1]
```java
@Entity
public class Member {
    
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    private String username;
    
    // 연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    
    // Getter, Setter...
}
```

#### Team.java
```java
@Entity
public class Team {
    
    @Id @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;
    
    private String name;
    
    // Getter, Setter ...
}
```
+ 회원은 Member.team 으로 팀 엔티티를 참조할 수 있다. (반대로 팀에는 회원을 참조하는 필드가 없다.)
+ `@JoinColumn(name = "TEAM_ID")` : TEAM_ID 외래키와 매핑했다.

### 6.1.2 다대일 양방향 [N:1, 1:N]

```java
@Entity
public class Member {
    
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    private String username;
    
    // 연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    
    // 연관관계 설정
    public void setTeam(Team team) {
        this.team = team;

        // 무한 루프에 빠지지 않도록 체크
        if (!team.getMembers().contains(this)) {
            team.getMembers().add(this);
        }

    }
    // Getter, Setter...
}
```

#### Team.java
```java
@Entity
public class Team {
    
    @Id @GeneratedValue
    @Column(name = "TEAM_ID")
    private String id;
    
    private String name;
    
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();

    public void addMember(Member member) {
        this.members.add(member);
        if (member.getTeam() != this) {
            member.setTeam(this);
        }
    }
}
```
+ 양방향은 외래 키가 있는 쪽이 연관관계의 주인
+ 양방향 연관관계를 항상 서로를 참조해야 한다.
+ 항상 서로를 참조하게 하려면 연관관계 편의 메소드를 작성하는 것이 좋다. (양쪽에 다 작성할 경우 무한루프에 빠지므로 주의. 또한 양쪽에 다 작성한 경우 둘 중 하나만 호출해도 된다.)

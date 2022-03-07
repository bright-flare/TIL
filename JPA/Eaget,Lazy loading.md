## Eager, Lazy loading

> **즉시로딩 (Eager) : 엔티티를 조회할 때 연관된 엔티티도 함께 조회한다.**

> **지연로딩 (Lazy) : 연관된 엔티티를 실제 사용할 때 조회한다. 처음 조회할 때에는 단일 Entity만 조회하지만 그 Entity를 실제로 사용할 때 연관된 Entity관련 Query가 발생한다.**


### 즉시 로딩

```java
@Entity
public class Member{

	@Id
	@GeneratedValue
	@Column(name = "member_id")
	private Long id;
	
	private int age;

	private String name;

	@ManyToOne(fetch = FetchType.EAGER) // 즉시 로딩
	@JoinColumn(name = "team_id")
	private Team team;
}
```

- 실행해보기
```java
Team team = new Team("team !");
entityManager.persist(team);

Member member = new Member("sseob");
member.setCreatedBy("심현섭");
member.setCreatedDate(LocalDateTime.now());
member.setTeam(team);
entityManager.persist(member);

// Fetch전략이 즉시로딩일 경우 join하여 즉시 로딩한다.
Member m = entityManager.find(Member.class, member.getId());
```

- 실행 query
```sql
    select
        member0_.member_id as member_i1_9_0_,
        -- ..생략
        team1_.team_id as team_id1_14_1_,
        team1_.createdBy as createdb2_14_1_,
        team1_.createdDate as createdd3_14_1_,
        team1_.lastModifiedBy as lastmodi4_14_1_,
        team1_.lastModifiedDate as lastmodi5_14_1_,
        team1_.name as name6_14_1_ 
    from
        Member member0_ 
    left outer join
        Team team1_ 
            on member0_.team_id=team1_.team_id 
    where
        member0_.member_id=?
```

1. 위와같은 코드가 있다고 해보자. Member Entity와 Team Entity는 `@ManyToOne`으로 연관관계가 설정되어 있다.
2. `@ManyToOne`은 Fetch Type이 기본적으로 Eager이다.
3. Fetch Type이 Eager이기 때문에 `m.getTeam()` 이러한 Team Entity관련 메소드를 호출하지 않더라도 Team을 join하여 결과값을 가져온다.

### 지연 로딩

> **Team Entity와의 연관관계 설정을 그대로 두고 `FetchType`을 `Lazy`로 설정하면**

```sql
    select
        member0_.member_id as member_i1_9_0_,
        member0_.createdBy as createdb2_9_0_,
        member0_.createdDate as createdd3_9_0_,
        member0_.lastModifiedBy as lastmodi4_9_0_,
        member0_.lastModifiedDate as lastmodi5_9_0_,
        -- ...생략
    from
        Member member0_ 
    where
        member0_.member_id=?
```

- 즉시 로딩과는 달리 Team을 join한 query가 발생하지 않는다. 
- 그리고 아래와 같이 `m.getTeam()`으로 Team객체를 얻어 print해보면 프록시 객체가 조회 되는 것을 확인할 수 있다.
```java
Member m = entityManager.find(Member.class, member.getId());
System.out.println(m.getTeam().getClass());

// print result
// class me.sseob.jpa.practice.basic.Team$HibernateProxy$l5MmBI0U
```
- 즉, 지연 로딩은 Team객체를 프록시 객체로 제공하며 Team객체를 실제 사용할 때에 뒤늦게(지연하여) DB에서 조회하여 Team 객체를 제공한다.
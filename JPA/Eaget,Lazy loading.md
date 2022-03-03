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
List<Member> resultList = entityManager.createQuery("select m from Member m ", Member.class).getResultList();
resultList.forEach(System.out::println);
```

1. 위와같은 코드가 있다고 해보자. Member Entity와 Team Entity는 `@ManyToOne`으로 연관관계가 설정되어 있다.
2. `@ManyToOne`은 Fetch Type이 기본적으로 Eager이다.
3. Fetch Type이 Eager이기 때문에 
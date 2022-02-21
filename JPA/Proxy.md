## Proxy

> JPA 핵심적인 기능중 하나인 지연로딩을 지원하기위해 존재하는 필수적인 개념인 Proxy에 대해서 알아본다.

```java
@Entity
public class Member extends BaseEntity {

	@Id
	@GeneratedValue
	@Column(name = "member_id")
	private Long id;
	
	private int age;

	private String name;

	@ManyToOne(fetch = FetchType.LAZY) // 지연 로딩. proxy객체를 조회한다.
	@JoinColumn(name = "team_id")
	private Team team;

    ...getter
    ...setter
```

# 🛠 Spring boot properties


## 🧑🏻‍💻 프로퍼티 적용 우선순위.
- 우선순위는 아래 링크 참조.
- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config

## 🧑🏻‍💻 Type safety 프로퍼티 만들기 !

```java
@Component
@ConfigurationProperties("sseob")
@Validated
public class SeobProperties {
	private String name;
	private int age;
	private String fullName;

    // @DurationUnit(ChronoUnit.SECONDS) 프로퍼티 파일에 30s라고 적어주면 이 어노테이션은 생략 가능하다.
	private Duration sessionTimeout = Duration.ofSeconds(30); // 기본값 30초
    ... (생략)getter, setter
}
```

- `@ConfigurationProperties` 어노테이션을 통해 만들 수 있다.
    - 프로퍼티를 설정한다는 `@EnableConfigurationProperties` 어노테이션의 설정과 같이 사용 되어야 하지만, springboot application에서는 생략 가능하다.
    - prefix가 `sseob`인 property값을 읽어와 매핑한다.
    - properties file에서 설정된 prefix의 설정값을 사용할 때에 IDE의 적극적인 지원을 받아 편리하게 이용 가능하다.
    - 카멜케이스, 스네이크 케이스, 케밥 케이스 등 다양한 text 문자 형식으로 properties 파일에서 작성해도 적절히 매핑된다.
        ex) fullName, full-name, full_name, FULLNAME -> 모두 fullName 필드로 매핑된다.

- @Component 어노테이션을 통해 `Bean`으로 등록시킬 수 있다.
    - 이렇게 `Bean`으로 만든다면 어디서든 주입받아 사용 가능하다.
    - 매핑된 프로퍼티 Class가 없는 경우에는 @Value 어노테이션으로 프로퍼티 값을 사용 했는데, 이렇게 매핑 Class를 만들어 놓으면 @Value 어노테이션을 대체하여 사용 가능하다. 이미 모든 값들이 해당 prefix에 매핑하여 Bean으로 만들어 놨으니 주입받아 사용하면 된다.

- 프로퍼티 값을 매핑할 때에 @Validated 어노테이션을 통해 validation check도 가능하다 !
- 프로퍼티 값을 매핑할 때에 Type conversion이 적용된다.
> ex) `sseob.age = 10` -> 10이라는 숫자는 properties file에서 단순한 text이지만 `int`로 타입이 바뀌어 매핑된다.
> ex) `sseob.sessionTimeout = 20s` -> properties file에서 s라는 seconds를 상징하는 문자를 붙여주면 Duration 타입으로 매핑된다.
> ex) `sseob.sessionTimeout = 20` -> 이 경우에는 ssesionTimeout field에 `@DurationUnit(ChronoUnit.SECONDS)` 라고 명시해야된다.


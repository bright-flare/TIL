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
    - 프로퍼티 매핑 Class에서 설정된 prefix값을 properties 파일에서 사용할 때에 IDE의 적극적인 지원을 받아 편리하게 이용 가능하다.
    - 카멜케이스, 스네이크 케이스, 케밥 케이스 등 다양한 text 문자 형식으로 properties 파일에서 작성해도 적절히 매핑된다.
        > ex) fullName, full-name, full_name, FULLNAME -> 모두 fullName 필드로 매핑된다.

- @Component 어노테이션을 통해 `Bean`으로 등록시킬 수 있다.
    - 이렇게 `Bean`으로 만든다면 어디서든 주입받아 사용 가능하다.
    - 매핑된 프로퍼티 Class가 없는 경우에는 @Value 어노테이션으로 프로퍼티 값을 사용 했는데, 이렇게 매핑 Class를 만들어 놓으면 @Value 어노테이션을 대체하여 사용 가능하다. 이미 모든 값들이 해당 prefix에 매핑하여 Bean으로 만들어 놨으니 주입받아 사용하면 된다.

- 프로퍼티 값을 매핑할 때에 @Validated 어노테이션을 통해 validation check도 가능하다 !
- 프로퍼티 값을 매핑할 때에 Type conversion이 적용된다.
    > ex) `sseob.age = 10` -> 10이라는 숫자는 properties file에서 단순한 text이지만 `int`로 타입이 바뀌어 매핑된다.

    > ex) `sseob.sessionTimeout = 20s` -> properties file에서 s라는 seconds를 상징하는 문자를 붙여주면 Duration 타입으로 매핑된다.

    > ex) `sseob.sessionTimeout = 20` -> 이 경우에는 ssesionTimeout field에 `@DurationUnit(ChronoUnit.SECONDS)` 라고 명시해야된다.


## 👋 Profile과 함께

> 여러가지 프로파일을 만들어 놓고, program argument 또는 application.properties에 설정하여 선택적인 프로퍼티 값을 사용할 수 있다.

> 선택적인 Bean설정을 사용할 수 있다.

```java
@Configuration
@Profile("hyun")
public class HyunProfileConfig {
	
	@Bean
	public String hello() {
		return "hello hyun !";
	}
}

@Configuration
@Profile("sseob")
public class SeobProfileConfig {
	
	@Bean
	public String hello() {
		return "hello sseob !";
	}
}
```

- `application properties` hyun profile 활성화 설정.
```xml
sseob.fullName= hyunSseob
spring.profiles.active=hyun
```

- `application-sseob.properties` sseob.fullName값 설정.
```xml
sseob.full-name=sseob profile was changed sseob full name
```

- `application-hyun.properties` sseob.fullName값 설정.
```xml
sseob.full-name=hyun profile was changed sseob full name
```

```java
/**
 * 프로파일 설정에 따라 어떻게 동작하는지 확인
 */
@Component
public class ProfileRunner implements ApplicationRunner {

	@Autowired
	private SeobProperties seobProperties;

	// profile config를 통해 생성한 bean 주입받음
	@Autowired
	private String hello;

	@Override
	public void run(ApplicationArguments args) throws Exception {
		System.out.println("============ProfileRunner============");
		System.out.println(hello);
		System.out.println(seobProperties.getFullName());
		System.out.println("============ProfileRunner============");
	}
}
```

### 출력
1. profile 활성화 설정에 따라 `HyunProfileConfig` Class의 Bean이 **주입** 되었음을 알 수 있다.
2. 프로퍼티 적용 우선순위에 따라 sseob.fullName 값이 application-hyun.properties 파일의 값으로 적용된것을 확인할 수 있다. 
```console
============ProfileRunner============
hello hyun !
hyun profile was changed sseob full name
============ProfileRunner============
```

### 프로파일 변경하기
- application.properties 파일의 설정을 통해 변경할 수 있지만 다른 방법도 있다.
```xml
spring.profiles.active=hyun
```

- program argument 사용하기.
    - 패키징 후 jar 파일을 실행할 때에 java -jar sseob.jar --spring.profiles.active=hyun
    - program argument 또한 프로퍼티 적용 우선순위에 따라 적용된다.
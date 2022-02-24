## Proxy

> JPA 핵심적인 기능중 하나인 지연로딩을 지원하기위해 존재하는 필수적인 개념인 `Proxy`에 대해서 알아본다.

> 지연 로딩 기능을 사용하려면 `실제 Entity 객체` 대신에 DB 조회를 지연할 수 있는 `가짜 객체`가 필요한데 이것을 `Proxy` 객체라고 한다.

> JPA 표준 Spec에서 지원하는 것이 아닌 JPA를 구현하는 구현체들이 갖는 특별한 개념이다.

### EntityManager.getReference()
- JPA에서 Id값으로 Entity를 조회할 때에는 `EntityManager.find()`를 사용한다. 이 method는 영속성 컨텍스트에 Entity가 없으면 DB를 조회한다. 이렇게 Entity를 직접 조회하게 되면 Entity를 사용하든 사용하지 않든 (영속성 컨텍스트에 해당 Entity가 없다는 전제하에) DB를 조회하게 된다.
- 그런데 Entity를 실제 사용하는 시점까지 DB조회를 미루고 싶으면 이 때 `EntityManager.getReference()` method를 사용할 수 있다. 이 method는 DB를 조회하지도 않고 실제 Entity 객체를 생성하지도 않는다. 대신에 DB접근을 위임한 `Proxy` 객체를 반환하게 된다.

### Proxy 특징
- `Proxy` 객체는 처음 사용할 때 한 번만 초기화 한다.
- 실제 Class를 상속 받아서 만들어진다.
    - 그래서 Type을 체크할 때, 주의해야 한다. `==` 연산자 비교대신 `instance of`를 사용해야 한다.
    - `Proxy` 객체와 Entity객체는 `==` 연산자로 비교하면 `false`이다. (같은 Type이 아니니까.)
- 실제 Class와 겉 모양이 같다.
- 이론상 진짜 객체인지 `Proxy` 객체인지 구분하지 않고 사용할 수 있다.
- `Proxy` 객체는 실제 객체의 `참조(target)`를 보관한다. 이로인해 `Proxy` 객체를 통해 실제 Entity에 접근할 수 있다.
- `Proxy` 객체를 호출하면 `Proxy` 객체는 `실제 객체의 method`를 호출한다.
- 영속성 컨텍스트에서 찾는 Entity가 이미 있으면 `EntityManager.getReference()` mehtod를 호출해도 **`Proxy` 객체를 반환하는게 아닌** 실제 Entity를 반환한다.
    - 이미 영속성 컨텍스트에 Entity가 있다면 굳이 `Proxy` 객체를 반환해도 큰 이점이 없다.
    - JPA는 한 Transaction에 대해서 같은 id값을 가진 Entity는 동일성에 대해 보장해준다. (영속화된 Entity에 대해 동일성을 보장).
    - 영속성 컨텍스트에 Entity에 대한 `Proxy` 객체가 존재한다면 동일성을 보장하기 위해 실제 Entity를 반환하는게 아닌 `Proxy` 객체를 반환한다.
- 영속성 컨텍스트의 도움을 받을 수 없는 `준영속(detached)` 상태일 때, `Proxy`를 초기화 하면 예외가 발생한다.
    - `Proxy` 객체는 `target`을 통해 실제 Entity의 method를 호출하게 되는데 영속성 컨텍스트에 해당 Entity가 비어있게 되면 영속성 컨텍스트에 초기화 요청을 하게된다. 이 때 실제 DB에서 조회를 하여 Entity를 생성하게 되는데 `준영속 (detached)` 상태는 영속성 컨텍스트의 관리를 받지 않는 상태이기 때문에 에외가 발생하게 된다.
    - hibernate일 경우 예외명은 `org.hibernate.LazyInitializationException`이다.

### Proxy 확인
- `Proxy` 인스턴스의 초기화 여부 확인 -> `PersistenceUnitUtil.isLoaded()`
- `Proxy` 클래스 확인 -> `System.out.println(entity.getClass().getName())`
- `Proxy` 강제 초기화 -> `org.hibernate.Hibernate.initialize()`

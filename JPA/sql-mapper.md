### Sql mapper와의 차이점.

### 보통 흔히 사용하는 sql mapper로 Mybatis를 예를 들어보자.
- sql mapper인 Mybatis는 sql을 mapping하기위한 mapper xml파일에 sql query문을 작성하여 객체와 sql을 매핑하게 된다.
- query문을 작성해 놓고 Mybatis에서 제공하는 동적 query 문법을 사용하면 편리하게 동적 query를 작성할 수 있다.
- 하지만, sql을 작성하는 주체는 개발자가 되어버리고 객체를 sql을 통해 매핑 하다보면 sql에 의존적인 개발을 하게된다.

### JPA
- JPA는 객체와 Database Table을 매핑하게 되는데, JPA구현체들(대표적으로 Hibernate)이 설정된 매핑정보를 토대로 sql query문을 만들어 준다.
- 개발자는 sql query문에 의존적인 개발을 하지 않고, 올바른 매핑 방법과 객체 중심의 비즈니스 로직을 작성하게 된다.
- 동적 query을 사용하는데에 Mybatis보다 불편한 점이 있지만, 객체 중심의 query를 생성할 수 있는 Jpql, QueryDSL로 충분히 보완 가능하다.

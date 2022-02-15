## 🥸 EntityManager
> `EntityManager`는 Entity를 저장, 수정, 삭제, 조회하는 등 Entity와 관련된 모든 일을 처리한다. 이름 그대로 Entity의 매니저다.

###  EntityManagerFactory
- Database를 하나만 사용할 경우 일반적으로 EntityManagerFactory는 하나만 생성한다.
- EntityManagerFactory를 통해 EntityManager를 얻는다.
```java
EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("sseob");
EntityManager em = entityManagerFactory.createEntityManager(); // entity manager 생성하여 얻음
```

- `Persistence.createEntityManagerFactory("sseob");` 를 실행하게 되면 META-INF/persistence.xml파일의 정보를 바탕으로 EntityManagerFactory를 생성한다. 
```xml
// persistence.xml
<persistence-unit name="sseob">
    <properties>
        <!-- 필수 속성. Database info -->
        <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
        <property name="javax.persistence.jdbc.user" value="sa"/>
        <property name="javax.persistence.jdbc.password" value=""/>
        <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/shop"/>
        <property name="hibernate.dialect" value="me.sseob.jpa.practice.sql.jpql.MyH2Dialect"/>
        <!-- 옵션 -->
        <property name="hibernate.show_sql" value="true"/>
        <property name="hibernate.format_sql" value="true"/>
        <property name="hibernate.use_sql_comments" value="true"/>
        <property name="hibernate.jdbc.batch_size" value="10"/>
        <property name="hibernate.hbm2ddl.auto" value="create" />
        <!-- fetch batch size. N + 1을 방지 -->
        <property name="hibernate.default_batch_fetch_size" value="100"/>
    </properties>
</persistence-unit>
```
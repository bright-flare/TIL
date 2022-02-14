## 🥸 EntityManager
> `EntityManager`는 Entity를 저장, 수정, 삭제, 조회하는 등 Entity와 관련된 모든 일을 처리한다. 이름 그대로 Entity의 매니저다.

###  EntityManagerFactory
- Database를 하나만 사용할 경우 EntityManagerFactory는 하나만 생성한다.
- EntityManagerFactory를 통해 EntityManager를 얻는다.
```java
EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("sseob");
EntityManager em = entityManagerFactory.createEntityManager(); // entity manager 생성하여 얻음
```
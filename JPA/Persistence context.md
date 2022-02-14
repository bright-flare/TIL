## 영속성 컨텍스트에 대해 알아보기 👀

- 엔티티를 영구 저장하는 환경이다.
- EntityManager를 통해서 영속성 컨텍스트에 접근할 수 있다.


### EntityManager 생명주기
1. 비영속(new/transient)
    - Account Class가 존재한다고 가정했을 때 new Account();를 통해 객체를 생성한 상태. JPA와 전혀 관계가 없는 상태.
2. 영속(managed)
    - EntityManager의 persist(account); 를 통해서  객체를 영속성 컨텍스트에 저장한 상태.
    - 영속성 컨텍스트를 통해서 account객체가 관리되기 시작한다.
    - Database Insert Query가 실행되지 않음. Transaction commit이 발생하는 시점에 Query가 실행된다.
3. 준영속(detached)
    - 영속성 컨텍스트에 저장되었다가 분리된 상태.
    - EntityManger detach() 를 통해서 영속성 분리가 가능하다.
4. 삭제(remove)
    - 실제 Database에서 삭제한다.

### 영속성 컨텍스트의 이점

1. 1차캐시를 사용한다.
    - 엔티티 조회시 영속성 컨텍스트 내부 1차캐시에서 먼저 해당 객체를 조회한다. (조회 Query가 실행되지 않는다.)
    - 1차캐시에 해당 객체가 존재하지 않는다면 DB에서 해당 객체를 조회하며, 1차캐시에 저장한다. 이후 똑같은 객체를 한번 더 `조회하게 되면 1차 캐시를 참조하게 된다.

2. 영속화된 Entity의 동일성을 보장한다.
    - 같은 Transaction안에서 1차캐시로 읽어진 객체는 
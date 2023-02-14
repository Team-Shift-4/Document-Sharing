[참고블로그](https://velog.io/@moonyoung/JPA-JPA-Repository-%EC%88%98%EC%A0%95%EC%82%AD%EC%A0%9C%EC%99%80-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98)



# `@transactional`

**저장,수정,갱신 로직에는 트랜잭션을 붙여주는 게 좋다**.
JpaRepostiroy의 실제 구현체인 SimpleJpaRepository 속을 들여다 보면 save, delete 관련 로직에는 트랜잭션이 붙어있다. 

**실제 구현체에서 제공하는 save,delete 메서드가 아니라면 트랜잭션을 붙여주자!**


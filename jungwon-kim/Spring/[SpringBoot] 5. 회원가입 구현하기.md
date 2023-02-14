[toc]



#  📌Model 생성

- src/main/java/com.example.backend/model/User.java

``` java
@Entity
@Getter
@Setter
@DynamicInsert
public class User {

    @Id
    @GeneratedValue
    String id;

    String password;

    String name;

    String birth;
}
```

### @Entity

- JPA에서 관리할 객체를 선언

### @Getter, @Setter

- Lombok 에서 사용
- 접근자와 설정자가 자동으로 생성

### @DynamicInsert

- null인 필드값이 insert시 제외
- update 시에도 null인 필드값 제외시키고싶으면 `@DymamicUpdate`

### @Id

- 객체의 Primary Key

### @GeneratedValue

- Primary Key의 자동 생성

  1. AUTO
     - 특정 DB에 맞게 자동 선택해줌 ( 오라클의 경우 시퀀스로 )
     - `@GeneratedValue(strategy = GenerationType.AUTO)`
  2. IDENTITY
     - DB의 identity 컬럼을 이용
     - `@GeneratedValue(strategy = GenerationType.IDENTITY)`
  3. SEQUENCE
     - DB의 시퀀스 컬럼을 사용
     - `@GeneratedValue(strategy = GenerationType.SEQUENCE)`
  4. TABLE
     - 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만드는 방법
     - `@GeneratedValue(strategy = GenerationType.TABLE)`

  [참고](https://ithub.tistory.com/24)

- `@GeneratedValue` 없이 `@Id` 어노테이션만 사용한다면 직접 할당

# 📌Controller 생성

- src/main/java/com.example.backend/controller/UserController.java

``` java
@RestController
@RequiredArgsConstructor
@Slf4j
public class UserController {

    private final UserService userService;

    // 회원가입
    @PostMapping("/user/save")
    public String createJsonTodo(@Valid @RequestBody UserForm form, BindingResult bindingResult) {
        log.info("Post : user Save");

        return validation(form, bindingResult);
    }

    // user 목록
    @GetMapping("/user/all")
    public List<User> list() {
        log.info("Get : user List");

        return userService.findAll();
    }

    // 요청 파라미터 validation 체크
    private String validation(@Valid @RequestBody UserForm form, BindingResult bindingResult) {

        if (bindingResult.hasErrors()) {
            return "user error";
        }

        User user = new User();
        user.setId(form.getId());
        user.setPassword(form.getPassword());
        user.setName(form.getName());
        user.setBirth(form.getBirth());

        userService.save(user);

        return "ok";
    }
}

```

### @RestController

- @ResponseBody 어노테이션을 붙이지 않아도 됨

### @RequiredArgsConstructor

- Lombok 에서 사용

- final 혹은 @NotNull이 붙은 필드의 생성자를 자동으로 만들어줌

  [참고 블로그](https://computer-science-student.tistory.com/622)

  ``` java
  // @RequiredArgsConstructor 사용
  @Service
  @RequiredArgsConstructor
  public class TestService {
      private final TestRepository1 testRepository1;
      private final TestRepository2 testRepository2;
  }
  // 생성자(Constructor) 방식
  @Service
  public class TestService {
      private final TestRepository1 testRepository1;
      private final TestRepository2 testRepository2;
  
      @Autowired
      public TestService(TestRepository1 testRepository1, TestRepository2 testRepository2) {
          this.testRepository1 = testRepository1;
          this.testRepository2 = testRepository2;
      }
  }
  ```


### @Slf4j

- 로깅에 대한 추상 레이어를 제공하는 인터페이스의 모음

- 추후에 필요로 의해 로깅 라이브러리를 변경할 때 코드의 변경 없이 가능하다는 장점

- 로깅이 필요한 부분에는 log 변수로 로그를 생성해서 사용하면 됨

  ``` java
  log.trace("가장 디테일한 로그");
  log.warn("경고");
  log.info("정보성 로그");
  log.debug("디버깅용 로그");
  log.error("에러", e);
  ```

### @PostMapping("경로")

- @RequestMapping(value="경로", method=RequestMethod.POST) 를 줄여쓴 것

  [참고 블로그](https://change-words.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-RequestMapping-%EB%8C%80%EC%8B%A0-PostMapping-GetMapping-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0)

###  @Valid

- @Valid 를 이용해 @RequestBody 객체 검증
- 객체에 들어가는 값을 검증
- `hibernate-validator`  

### BindingResult

- Validator를 상속받는 클래스에서 객체값을 검증하는 방식
- 객체에 들어가는 값 검증
- ` validation-api 라이브러리`

### Validator 

- 객체를 검증할 validator 클래스를 생성
- bindingResult.hasErrors() 함수로 값이 알맞은지 체크

[Valid방식과 BindingResult방식 참고 블로그](https://parkwonhui.github.io/spring/2019/04/22/spring-valid-bindingresult.html)

# 📌 Service 생성 

``` java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class UserService {
    private final UserRepository userRepository;
    // 회원가입
    @Transactional
    public String save(User user){
        userRepository.save(user);

        return user.getId();
    }

    // user 전체 조회
    public List<User> findAll(){
        return userRepository.findAll();
    }

    // user 단건 조회
    public User findOne(String userId){
        return userRepository.findOne(userId);
    }
}
```

### @Transactional

- 랜잭션을 선언하고 begin, commit까지 자동으로 수행

### @Transactional(readOnly = true)

- 트랜잭션을 읽기 전용 모드로 변경 ⇒ 성능 향상

  

  [@Transactional 자세히 알아보기](https://resilient-923.tistory.com/391)

# 📌 Repository 생성

``` java
@Repository
@RequiredArgsConstructor
public class UserRepository {

    private final EntityManager em;

    // 작성
    public void save(User user) {
        em.persist(user);
    }

    // 단건 조회
    public User findOne(String id) {
        return em.find(User.class, id);
    }

    // 전체 조회
    public List<User> findAll() {
        List<User> userList = null;
        
        userList = em.createQuery("select u from User u", User.class).getResultList();
        return userList;
    }
}
```

### EntityManager

- Entity의 생명 주기와 트랜잭션 등을 관리하며, 영속성 컨텍스트에 접근하는 객체

### EntityManager.persist(Object entity)

- INSERT 기능 (영속성)

### EntityManager.find(Class<T> entityClass, Object primaryKey)

- SELECT 기능 (영속성)

### EntityManager.createQuery(String query)

- JPQL 문법의 쿼리를 인자로 넣으면 SQL로 변환되어 DB에 전송되고, 이 과정에서 flush 메소드가 자동으로 호출

  - JPQL ?

     DB 테이블을 대상으로 쿼리하는 SQL과 달리 클래스와 필드를 대상으로 쿼리하는 객체 지향 쿼리

- 예제

  ``` java
  String jpql = "select m from Member m join m.team t where t.name=:teamName"; 
  // 앞에 :가 붙은 변수에 파라미터를 바인딩 받는다.
  List<Member> memberList = em.createQuery(jpql, Member.class)
      .setParameter("teamName", "group"); // :teamName에 파라미터 바인딩
      .getResultList();
  ```



👉 `EntityManager` 는 추후 게시글로 자세히 다룰 예정!

다 작성했으면 postman으로 테스트하면 된다.

[postman 프로그램이 없다면 다운로드하러가기](https://www.postman.com/downloads/)



다음 게시글에선 로그인 화면을 구현해서 테스트해볼 예정이다!

어노테이션이 너무 많아서 하나하나 검색하면서 익히는 것도 시간이 많이 드는 것같다.

하지만 jsp에 비해 굉장히 간결한 문법...! 열심히 해봐야겠다.

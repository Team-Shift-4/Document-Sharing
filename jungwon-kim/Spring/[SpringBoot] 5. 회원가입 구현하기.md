[toc]



#  ๐Model ์์ฑ

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

- JPA์์ ๊ด๋ฆฌํ  ๊ฐ์ฒด๋ฅผ ์ ์ธ

### @Getter, @Setter

- Lombok ์์ ์ฌ์ฉ
- ์ ๊ทผ์์ ์ค์ ์๊ฐ ์๋์ผ๋ก ์์ฑ

### @DynamicInsert

- null์ธ ํ๋๊ฐ์ด insert์ ์ ์ธ
- update ์์๋ null์ธ ํ๋๊ฐ ์ ์ธ์ํค๊ณ ์ถ์ผ๋ฉด `@DymamicUpdate`

### @Id

- ๊ฐ์ฒด์ Primary Key

### @GeneratedValue

- Primary Key์ ์๋ ์์ฑ

  1. AUTO
     - ํน์  DB์ ๋ง๊ฒ ์๋ ์ ํํด์ค ( ์ค๋ผํด์ ๊ฒฝ์ฐ ์ํ์ค๋ก )
     - `@GeneratedValue(strategy = GenerationType.AUTO)`
  2. IDENTITY
     - DB์ identity ์ปฌ๋ผ์ ์ด์ฉ
     - `@GeneratedValue(strategy = GenerationType.IDENTITY)`
  3. SEQUENCE
     - DB์ ์ํ์ค ์ปฌ๋ผ์ ์ฌ์ฉ
     - `@GeneratedValue(strategy = GenerationType.SEQUENCE)`
  4. TABLE
     - ํค ์์ฑ ์ ์ฉ ํ์ด๋ธ์ ํ๋ ๋ง๋ค๊ณ  ์ฌ๊ธฐ์ ์ด๋ฆ๊ณผ ๊ฐ์ผ๋ก ์ฌ์ฉํ  ์ปฌ๋ผ์ ๋ง๋๋ ๋ฐฉ๋ฒ
     - `@GeneratedValue(strategy = GenerationType.TABLE)`

  [์ฐธ๊ณ ](https://ithub.tistory.com/24)

- `@GeneratedValue` ์์ด `@Id` ์ด๋ธํ์ด์๋ง ์ฌ์ฉํ๋ค๋ฉด ์ง์  ํ ๋น

# ๐Controller ์์ฑ

- src/main/java/com.example.backend/controller/UserController.java

``` java
@RestController
@RequiredArgsConstructor
@Slf4j
public class UserController {

    private final UserService userService;

    // ํ์๊ฐ์
    @PostMapping("/user/save")
    public String createJsonTodo(@Valid @RequestBody UserForm form, BindingResult bindingResult) {
        log.info("Post : user Save");

        return validation(form, bindingResult);
    }

    // user ๋ชฉ๋ก
    @GetMapping("/user/all")
    public List<User> list() {
        log.info("Get : user List");

        return userService.findAll();
    }

    // ์์ฒญ ํ๋ผ๋ฏธํฐ validation ์ฒดํฌ
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

- @ResponseBody ์ด๋ธํ์ด์์ ๋ถ์ด์ง ์์๋ ๋จ

### @RequiredArgsConstructor

- Lombok ์์ ์ฌ์ฉ

- final ํน์ @NotNull์ด ๋ถ์ ํ๋์ ์์ฑ์๋ฅผ ์๋์ผ๋ก ๋ง๋ค์ด์ค

  [์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://computer-science-student.tistory.com/622)

  ``` java
  // @RequiredArgsConstructor ์ฌ์ฉ
  @Service
  @RequiredArgsConstructor
  public class TestService {
      private final TestRepository1 testRepository1;
      private final TestRepository2 testRepository2;
  }
  // ์์ฑ์(Constructor) ๋ฐฉ์
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

- ๋ก๊น์ ๋ํ ์ถ์ ๋ ์ด์ด๋ฅผ ์ ๊ณตํ๋ ์ธํฐํ์ด์ค์ ๋ชจ์

- ์ถํ์ ํ์๋ก ์ํด ๋ก๊น ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๋ณ๊ฒฝํ  ๋ ์ฝ๋์ ๋ณ๊ฒฝ ์์ด ๊ฐ๋ฅํ๋ค๋ ์ฅ์ 

- ๋ก๊น์ด ํ์ํ ๋ถ๋ถ์๋ log ๋ณ์๋ก ๋ก๊ทธ๋ฅผ ์์ฑํด์ ์ฌ์ฉํ๋ฉด ๋จ

  ``` java
  log.trace("๊ฐ์ฅ ๋ํ์ผํ ๋ก๊ทธ");
  log.warn("๊ฒฝ๊ณ ");
  log.info("์ ๋ณด์ฑ ๋ก๊ทธ");
  log.debug("๋๋ฒ๊น์ฉ ๋ก๊ทธ");
  log.error("์๋ฌ", e);
  ```

### @PostMapping("๊ฒฝ๋ก")

- @RequestMapping(value="๊ฒฝ๋ก", method=RequestMethod.POST) ๋ฅผ ์ค์ฌ์ด ๊ฒ

  [์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://change-words.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-RequestMapping-%EB%8C%80%EC%8B%A0-PostMapping-GetMapping-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0)

###  @Valid

- @Valid ๋ฅผ ์ด์ฉํด @RequestBody ๊ฐ์ฒด ๊ฒ์ฆ
- ๊ฐ์ฒด์ ๋ค์ด๊ฐ๋ ๊ฐ์ ๊ฒ์ฆ
- `hibernate-validator`  

### BindingResult

- Validator๋ฅผ ์์๋ฐ๋ ํด๋์ค์์ ๊ฐ์ฒด๊ฐ์ ๊ฒ์ฆํ๋ ๋ฐฉ์
- ๊ฐ์ฒด์ ๋ค์ด๊ฐ๋ ๊ฐ ๊ฒ์ฆ
- ` validation-api ๋ผ์ด๋ธ๋ฌ๋ฆฌ`

### Validator 

- ๊ฐ์ฒด๋ฅผ ๊ฒ์ฆํ  validator ํด๋์ค๋ฅผ ์์ฑ
- bindingResult.hasErrors() ํจ์๋ก ๊ฐ์ด ์๋ง์์ง ์ฒดํฌ

[Valid๋ฐฉ์๊ณผ BindingResult๋ฐฉ์ ์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://parkwonhui.github.io/spring/2019/04/22/spring-valid-bindingresult.html)

# ๐ Service ์์ฑ 

``` java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class UserService {
    private final UserRepository userRepository;
    // ํ์๊ฐ์
    @Transactional
    public String save(User user){
        userRepository.save(user);

        return user.getId();
    }

    // user ์ ์ฒด ์กฐํ
    public List<User> findAll(){
        return userRepository.findAll();
    }

    // user ๋จ๊ฑด ์กฐํ
    public User findOne(String userId){
        return userRepository.findOne(userId);
    }
}
```

### @Transactional

- ๋์ญ์์ ์ ์ธํ๊ณ  begin, commit๊น์ง ์๋์ผ๋ก ์ํ

### @Transactional(readOnly = true)

- ํธ๋์ญ์์ ์ฝ๊ธฐ ์ ์ฉ ๋ชจ๋๋ก ๋ณ๊ฒฝ โ ์ฑ๋ฅ ํฅ์

  

  [@Transactional ์์ธํ ์์๋ณด๊ธฐ](https://resilient-923.tistory.com/391)

# ๐ Repository ์์ฑ

``` java
@Repository
@RequiredArgsConstructor
public class UserRepository {

    private final EntityManager em;

    // ์์ฑ
    public void save(User user) {
        em.persist(user);
    }

    // ๋จ๊ฑด ์กฐํ
    public User findOne(String id) {
        return em.find(User.class, id);
    }

    // ์ ์ฒด ์กฐํ
    public List<User> findAll() {
        List<User> userList = null;
        
        userList = em.createQuery("select u from User u", User.class).getResultList();
        return userList;
    }
}
```

### EntityManager

- Entity์ ์๋ช ์ฃผ๊ธฐ์ ํธ๋์ญ์ ๋ฑ์ ๊ด๋ฆฌํ๋ฉฐ, ์์์ฑ ์ปจํ์คํธ์ ์ ๊ทผํ๋ ๊ฐ์ฒด

### EntityManager.persist(Object entity)

- INSERT ๊ธฐ๋ฅ (์์์ฑ)

### EntityManager.find(Class<T> entityClass, Object primaryKey)

- SELECT ๊ธฐ๋ฅ (์์์ฑ)

### EntityManager.createQuery(String query)

- JPQL ๋ฌธ๋ฒ์ ์ฟผ๋ฆฌ๋ฅผ ์ธ์๋ก ๋ฃ์ผ๋ฉด SQL๋ก ๋ณํ๋์ด DB์ ์ ์ก๋๊ณ , ์ด ๊ณผ์ ์์ flush ๋ฉ์๋๊ฐ ์๋์ผ๋ก ํธ์ถ

  - JPQL ?

     DB ํ์ด๋ธ์ ๋์์ผ๋ก ์ฟผ๋ฆฌํ๋ SQL๊ณผ ๋ฌ๋ฆฌ ํด๋์ค์ ํ๋๋ฅผ ๋์์ผ๋ก ์ฟผ๋ฆฌํ๋ ๊ฐ์ฒด ์งํฅ ์ฟผ๋ฆฌ

- ์์ 

  ``` java
  String jpql = "select m from Member m join m.team t where t.name=:teamName"; 
  // ์์ :๊ฐ ๋ถ์ ๋ณ์์ ํ๋ผ๋ฏธํฐ๋ฅผ ๋ฐ์ธ๋ฉ ๋ฐ๋๋ค.
  List<Member> memberList = em.createQuery(jpql, Member.class)
      .setParameter("teamName", "group"); // :teamName์ ํ๋ผ๋ฏธํฐ ๋ฐ์ธ๋ฉ
      .getResultList();
  ```



๐ `EntityManager` ๋ ์ถํ ๊ฒ์๊ธ๋ก ์์ธํ ๋ค๋ฃฐ ์์ !

๋ค ์์ฑํ์ผ๋ฉด postman์ผ๋ก ํ์คํธํ๋ฉด ๋๋ค.

[postman ํ๋ก๊ทธ๋จ์ด ์๋ค๋ฉด ๋ค์ด๋ก๋ํ๋ฌ๊ฐ๊ธฐ](https://www.postman.com/downloads/)



๋ค์ ๊ฒ์๊ธ์์  ๋ก๊ทธ์ธ ํ๋ฉด์ ๊ตฌํํด์ ํ์คํธํด๋ณผ ์์ ์ด๋ค!

์ด๋ธํ์ด์์ด ๋๋ฌด ๋ง์์ ํ๋ํ๋ ๊ฒ์ํ๋ฉด์ ์ตํ๋ ๊ฒ๋ ์๊ฐ์ด ๋ง์ด ๋๋ ๊ฒ๊ฐ๋ค.

ํ์ง๋ง jsp์ ๋นํด ๊ต์ฅํ ๊ฐ๊ฒฐํ ๋ฌธ๋ฒ...! ์ด์ฌํ ํด๋ด์ผ๊ฒ ๋ค.

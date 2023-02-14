

## 👉 @RequestMapping

> 우리는 특정 url로 요청을 보내면 Controller에서 어떠한 방식으로 처리할지 정의를 한다.
>
> 이때 들어온 요청을 특정 메서드와 매핑하기 위해 사용하는 것이 @RequestMapping이다.
>
> @RequestMapping에서 가장 많이사용하는 부분은 value와 method이다.
>
> value는 요청받을 url을 설정하게 되고, 
>
> method는 어떤 요청으로 받을지 정의하게 된다.(GET, POST, PUT, DELETE 등)

- URL 을 컨트롤러의 메서드와 매핑할 때 사용하는 어노테이션
- 요청 주소(url) 설정, 요청 방식(GET, POST, DELETE, PATCH) 설정

``` java
@RequestMapping(value="/login", method = RequestMethod.POST)

@RequestMapping(value = "/get/{id}", method = RequestMethod.GET)
```



아래와 같이 만들 수 있다.

``` java
@RestController
public class exampleController {

    @RequestMapping(value = "/example/get", method = RequestMethod.GET)
    public String exampleGet(...) {
        ...
    }

    @RequestMapping(value = "/example/post", method = RequestMethod.POST)
    public String examplePost(...) {
        ...
    }

    @RequestMapping(value = "/example/put", method = RequestMethod.PUT)
    public String examplePut(...) {
        ...
    }

    @RequestMapping(value = "/example/delete", method = RequestMethod.DELETE)
    public String exampleDelete(...) {
        ...
    }
}
```

- 여기서 `/example/` 이라는 url 모두 공통되는 사항이므로 아래와 같이 클래스명 위에서도 정의하여 사용할 수도 있다.
- 정의하여 상세 어노테이션을 사용해서 정의해보자면 아래와 같다.

``` java
@RestController
@RequestMapping(value = "/example")
public class exampleController {

    @GetMapping("/get")
    public String exampleGet(...) {
        ...
    }

    @PostMapping("/post")
    public String examplePost(...) {
        ...
    }

    @PutMapping("/put")
    public String examplePut(...) {
        ...
    }

    @DeleteMapping("/delete")
    public String exampleDelete(...) {
        ...
    }
}
```



***

Spring 4.3 버전부터 추가된 RequestMapping 관련 상세 어노테이션 5개!

1. @PostMapping
2. @GetMapping
3. @PutMapping
4. @DeleteMapping
5. @PatchMapping



## 👉 @PostMapping

-  axios 통신시 get방식으로 받을 때 PostMapping사용
-  @RequestBody으로 받을 것

``` java
    @PostMapping("/signup")
    public void signup(@RequestBody @Valid UserForm userForm) throws Exception{
        userService.signUpUser(userForm.toEntity());
    }
```



***

## 👉 @GetMapping

- axios 통신시 get방식으로 받을 때 GetMapping 사용
- @RequestParam으로 받을 것

```java
    @GetMapping("/list")
    public List<Board> allList(@RequestParam Map<String, String> params){
        List<Board> BoardList = BoardService.BoardList(params.get("email"));
        return BoardList;
    }
```



***

## 👉 @DeleteMapping

- 기존의 정보 삭제할 때 사용

```java
    @DeleteMapping("/delete/{num}")
    public void deleteBoard(@PathVariable("num") Long num){
        BoardService.deleteBoard(num);
    }
```



***



## 👉 @PutMapping, @PatchMapping

- 정보 수정
- put은 전체 교체, patch는 부분 교체

```java
	@CrossOrigin
    @PutMapping("/update/{num}")
    public void updateBoard(@PathVariable("num") Long num, @RequestBody BoardForm BoardForm) throws Exception {
        BoardService.updateBoard(num, BoardForm);
    }
```





***

## 👉 axios 로 보낼 때

[axios 참고 게시글](https://wonny.kim/26)

- HttpMethod와 맞춰서 요청 보낼 것!
- 형식 안맞춰주면 `request method 'get' not supported 405` 와 같은 에러 발생


```javascript
axios.get("/list", { email: sessionStorage.getItem("email") })
            .then((res) => {
                   console.log(res);
             })
			.catch(() => )
```

```javascript
axios.delete("/delete/"+num,)
            .then((res) => {
                  alert("삭제 완료");
             })
```

```javascript
 axios.put("/update/"+num, this.Form)
            .then(() => {
                alert("수정 완료");
            })
```



[참고블로그](https://mungto.tistory.com/436)
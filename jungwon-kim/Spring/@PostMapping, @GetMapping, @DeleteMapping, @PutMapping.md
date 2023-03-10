

## ๐ @RequestMapping

> ์ฐ๋ฆฌ๋ ํน์  url๋ก ์์ฒญ์ ๋ณด๋ด๋ฉด Controller์์ ์ด๋ ํ ๋ฐฉ์์ผ๋ก ์ฒ๋ฆฌํ ์ง ์ ์๋ฅผ ํ๋ค.
>
> ์ด๋ ๋ค์ด์จ ์์ฒญ์ ํน์  ๋ฉ์๋์ ๋งคํํ๊ธฐ ์ํด ์ฌ์ฉํ๋ ๊ฒ์ด @RequestMapping์ด๋ค.
>
> @RequestMapping์์ ๊ฐ์ฅ ๋ง์ด์ฌ์ฉํ๋ ๋ถ๋ถ์ value์ method์ด๋ค.
>
> value๋ ์์ฒญ๋ฐ์ url์ ์ค์ ํ๊ฒ ๋๊ณ , 
>
> method๋ ์ด๋ค ์์ฒญ์ผ๋ก ๋ฐ์์ง ์ ์ํ๊ฒ ๋๋ค.(GET, POST, PUT, DELETE ๋ฑ)

- URL ์ ์ปจํธ๋กค๋ฌ์ ๋ฉ์๋์ ๋งคํํ  ๋ ์ฌ์ฉํ๋ ์ด๋ธํ์ด์
- ์์ฒญ ์ฃผ์(url) ์ค์ , ์์ฒญ ๋ฐฉ์(GET, POST, DELETE, PATCH) ์ค์ 

``` java
@RequestMapping(value="/login", method = RequestMethod.POST)

@RequestMapping(value = "/get/{id}", method = RequestMethod.GET)
```



์๋์ ๊ฐ์ด ๋ง๋ค ์ ์๋ค.

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

- ์ฌ๊ธฐ์ `/example/` ์ด๋ผ๋ url ๋ชจ๋ ๊ณตํต๋๋ ์ฌํญ์ด๋ฏ๋ก ์๋์ ๊ฐ์ด ํด๋์ค๋ช ์์์๋ ์ ์ํ์ฌ ์ฌ์ฉํ  ์๋ ์๋ค.
- ์ ์ํ์ฌ ์์ธ ์ด๋ธํ์ด์์ ์ฌ์ฉํด์ ์ ์ํด๋ณด์๋ฉด ์๋์ ๊ฐ๋ค.

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

Spring 4.3 ๋ฒ์ ๋ถํฐ ์ถ๊ฐ๋ RequestMapping ๊ด๋ จ ์์ธ ์ด๋ธํ์ด์ 5๊ฐ!

1. @PostMapping
2. @GetMapping
3. @PutMapping
4. @DeleteMapping
5. @PatchMapping



## ๐ @PostMapping

-  axios ํต์ ์ get๋ฐฉ์์ผ๋ก ๋ฐ์ ๋ PostMapping์ฌ์ฉ
-  @RequestBody์ผ๋ก ๋ฐ์ ๊ฒ

``` java
    @PostMapping("/signup")
    public void signup(@RequestBody @Valid UserForm userForm) throws Exception{
        userService.signUpUser(userForm.toEntity());
    }
```



***

## ๐ @GetMapping

- axios ํต์ ์ get๋ฐฉ์์ผ๋ก ๋ฐ์ ๋ GetMapping ์ฌ์ฉ
- @RequestParam์ผ๋ก ๋ฐ์ ๊ฒ

```java
    @GetMapping("/list")
    public List<Board> allList(@RequestParam Map<String, String> params){
        List<Board> BoardList = BoardService.BoardList(params.get("email"));
        return BoardList;
    }
```



***

## ๐ @DeleteMapping

- ๊ธฐ์กด์ ์ ๋ณด ์ญ์ ํ  ๋ ์ฌ์ฉ

```java
    @DeleteMapping("/delete/{num}")
    public void deleteBoard(@PathVariable("num") Long num){
        BoardService.deleteBoard(num);
    }
```



***



## ๐ @PutMapping, @PatchMapping

- ์ ๋ณด ์์ 
- put์ ์ ์ฒด ๊ต์ฒด, patch๋ ๋ถ๋ถ ๊ต์ฒด

```java
	@CrossOrigin
    @PutMapping("/update/{num}")
    public void updateBoard(@PathVariable("num") Long num, @RequestBody BoardForm BoardForm) throws Exception {
        BoardService.updateBoard(num, BoardForm);
    }
```





***

## ๐ axios ๋ก ๋ณด๋ผ ๋

[axios ์ฐธ๊ณ  ๊ฒ์๊ธ](https://wonny.kim/26)

- HttpMethod์ ๋ง์ถฐ์ ์์ฒญ ๋ณด๋ผ ๊ฒ!
- ํ์ ์๋ง์ถฐ์ฃผ๋ฉด `request method 'get' not supported 405` ์ ๊ฐ์ ์๋ฌ ๋ฐ์


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
                  alert("์ญ์  ์๋ฃ");
             })
```

```javascript
 axios.put("/update/"+num, this.Form)
            .then(() => {
                alert("์์  ์๋ฃ");
            })
```



[์ฐธ๊ณ ๋ธ๋ก๊ทธ](https://mungto.tistory.com/436)
https://wikidocs.net/161302

#  ๐Spring Boot Validation

- ์ ๋ฌ๋ฐ์ ์๋ ฅ ๊ฐ์ ๊ฒ์ฆ

  ```
  implementation 'org.springframework.boot:spring-boot-starter-validation'
  ```

- ๋ฌธ๋ฒ

  [๊ณต์๋ฌธ์](https://beanvalidation.org/)

	| ํญ๋ชฉ             | ์ค๋ช                              |
    | ---------------- | --------------------------------- |
    | @Size            | ๋ฌธ์ ๊ธธ์ด ์ ํ                    |
    | @NotNull         | Null์ ํ์ฉํ์ง ์์              |
    | @NotEmpty        | Null ๋๋ ๋น ๋ฌธ์์ด ํ์ฉํ์ง ์์ |
    | @Past            | ๊ณผ๊ฑฐ ๋ ์ง๋ง ๊ฐ๋ฅ                  |
    | @Future          | ๋ฏธ๋ ๋ ์ง๋ง ๊ฐ๋ฅ                  |
    | @FutureOrPresent | ๋ฏธ๋ ๋๋ ์ค๋ ๋ ์ง๋ง ๊ฐ๋ฅ        |
    | @Max             | ์ต๋๊ฐ                            |
    | @Min             | ์ต์๊ฐ                            |
    | @Pattern         | ์ ๊ท์ ๊ฒ                         |


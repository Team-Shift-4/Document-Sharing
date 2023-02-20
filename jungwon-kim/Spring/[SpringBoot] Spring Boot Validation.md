https://wikidocs.net/161302

#  📌Spring Boot Validation

- 전달받은 입력 값을 검증

  ```
  implementation 'org.springframework.boot:spring-boot-starter-validation'
  ```

- 문법

  [공식문서](https://beanvalidation.org/)

	| 항목             | 설명                              |
    | ---------------- | --------------------------------- |
    | @Size            | 문자 길이 제한                    |
    | @NotNull         | Null을 허용하지 않음              |
    | @NotEmpty        | Null 또는 빈 문자열 허용하지 않음 |
    | @Past            | 과거 날짜만 가능                  |
    | @Future          | 미래 날짜만 가능                  |
    | @FutureOrPresent | 미래 또는 오늘 날짜만 가능        |
    | @Max             | 최대값                            |
    | @Min             | 최소값                            |
    | @Pattern         | 정규식 검                         |


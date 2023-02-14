# 📌 타임리프

[공식사이트](https://www.thymeleaf.org/)

### 템플릿 엔진이란 ?

- HTML(Markup)과 데이터를 결합한 결과물을 만들어 주는 도구
- 타임리프는 템플릿 엔진 중 하나로, Spring Boot에서는 JSP가 아닌 Thymeleaf 사용을 권장하고 있다.

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

혹은

``` html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```



[문법 보기](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standard-expression-syntax)

- 대부분의 html 속성을 th:xxx 로 변경할 수 있다.

  ex: th:text="${변수명}"

|   표현    |      설명       |
| :-------: | :-------------: |
| @{ ... }  | URL 링크 표현식 |
| \| ... \| |   리터럴대체    |
| ${ ... }  |      변수       |
|  th:each  |    반복 출력    |
| *{ ... }  |    선택 변수    |
| #{ ... }  |     메시지      |



``` html
<table>
	<thead>
		<tr>
			<th>제목</th>
			<th>작성일자</th>
		</tr>
	</thead>
	<tbody>
		<tr th:each="question: %{questionList}">
			<td th:text="${question.subject}"></td>
			<td th:text="${question.createDate}"></td>
		</tr>
	</tbody>
</table>
```

-  `th:` 로 시작하는 속성은 타임리프 템플릿 엔진이 사용하는 속성



### layout참고

https://bamdule.tistory.com/33
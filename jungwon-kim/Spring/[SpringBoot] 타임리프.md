# π νμλ¦¬ν

[κ³΅μμ¬μ΄νΈ](https://www.thymeleaf.org/)

### ννλ¦Ώ μμ§μ΄λ ?

- HTML(Markup)κ³Ό λ°μ΄ν°λ₯Ό κ²°ν©ν κ²°κ³Όλ¬Όμ λ§λ€μ΄ μ£Όλ λκ΅¬
- νμλ¦¬νλ ννλ¦Ώ μμ§ μ€ νλλ‘, Spring Bootμμλ JSPκ° μλ Thymeleaf μ¬μ©μ κΆμ₯νκ³  μλ€.

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

νΉμ

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



[λ¬Έλ² λ³΄κΈ°](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standard-expression-syntax)

- λλΆλΆμ html μμ±μ th:xxx λ‘ λ³κ²½ν  μ μλ€.

  ex: th:text="${λ³μλͺ}"

|   νν    |      μ€λͺ       |
| :-------: | :-------------: |
| @{ ... }  | URL λ§ν¬ ννμ |
| \| ... \| |   λ¦¬ν°λ΄λμ²΄    |
| ${ ... }  |      λ³μ       |
|  th:each  |    λ°λ³΅ μΆλ ₯    |
| *{ ... }  |    μ ν λ³μ    |
| #{ ... }  |     λ©μμ§      |



``` html
<table>
	<thead>
		<tr>
			<th>μ λͺ©</th>
			<th>μμ±μΌμ</th>
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

-  `th:` λ‘ μμνλ μμ±μ νμλ¦¬ν ννλ¦Ώ μμ§μ΄ μ¬μ©νλ μμ±



### layoutμ°Έκ³ 

https://bamdule.tistory.com/33
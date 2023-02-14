[toc]

***



# 📌 Spring Framework?

**자바 엔터프라이즈 개발을 편하게 해주는 오픈 소스 경량급 애플리케이션 프레임워크**

경량급이라고 말하는 의미 ⇒  EJB에서 POJO로 변경되면서 클래스가 구조적으로 간결해졌다는 의미

## POJO?

- Plain Old Java Object

> 말 그대로 해석을 하면 오래된 방식의 간단한 자바 오브젝트라는 말로서 Java EE 등의 중량 프레임워크들을 사용하게 되면서 해당 프레임워크에 종속된 "무거운" 객체를 만들게 된 것에 반발해서 사용되게 된 용어이다.



## Spring Architect

<img src="https://melonicedlatte.com/assets/images/2021_3Q/spring_architect.png" style="zoom: 67%;" /> 

[참고 블로그](https://melonicedlatte.com/2021/07/11/174700.html)

# 📌 Spring Boot?

- 기존 스프링(Spring) 프레임워크에 톰캣 서버를 내장하고 여러 편의 기능들을 추가한 프레임워크

  == 스프링부트는 자바로 만들어진 웹 프레임워크다!

많은 블로그글들과 책에서도 스프링부트는 웹 프레임워크라고 하는데, 사실 웹 프레임워크라고는 할 수 없는 것 같다.

([공식 문서](https://spring.io/projects/spring-boot#overview)에도 웹프레임워크라는 말은 없음)

그냥 기능들이 대부분 웹에서 쓰기 편한거라서 웹 프레임워크라고 하는 것 같다.



## **프레임워크?**

- ### 애플리케이션 프레임워크 == 프레임워크
- 소프트웨어 개발자가 응용 소프트웨어의 표준 구조를 구현하기 위해 사용하는  소프트웨어 프레임워크로 구성된다.
- 즉, 프로그래밍에서 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임

## **웹 프레임워크?**

- ### 웹 프레임워크 == 웹 애플리케이션 프레임워크

- 웹 서비스 개발을 위한 프레임워크 즉, 웹 프로그램을 만들기 위한 스타터 키트

- 동적인 웹 페이지나, 웹 애플리케이션, 웹 서비스 개발 보조용으로 만들어지는 애플리케이션 프레임워크의 일종



# 📌 스프링부트 특징

- 내장 서버: `WAR` 파일을 배포할 필요 없이 내장된 `Tomcat`, `Jetty`, `Unertow` 를 이용해 실행할 수 있습니다.
- 간단한 라이브러리 관리: 많이 사용하는 라이브러리를 모아놓은 스타터 (Starter) `POM` 파일로 메이븐 설정이 쉬워집니다.
- 자동 설정: 더 이상 `XML` 설정이 필요하지 않습니다.
- 레퍼런스 가이드 (Reference Guide): 문서화가 잘 되어 있어서 개발할 때 찾아보기 편합니다.

​	[참고 블로그](https://futurecreator.github.io/2016/06/18/spring-boot-get-started/)

# 📌 프로젝트 생성하기

 **인텔리제이(IntelliJ)** 사용

### File - new - New Project - Spring Initializr 클릭

- Name : 프로젝트 이름 (backend로 진행)

- Language : Java

- Type : Gradle - Groovy

- Spring Boot : 3.0.1 버전

- Dependencies : 추후에 추가 가능

  - 기본적으로 Spring Web를 선택합니다.

  

### 🤔 Gradle 과 Maven 의 차이?

1. Maven ? 
   - 아파치 메이븐은 자바용 프로젝트 관리 도구
   - 아파치 Ant의 대안
   - 아파치 라이센스로 배포되는 오픈 소스 소프트웨어
2. Gradle ?
   - 빌드, 프로젝트 구성/관리, 테스트, 배포 도구
   - 안드로이드 앱의 공식 빌드 시스템
   - 빌드 속도가 Maven에 비해 10~100배 가량 빠름
   - JAVA, C/C++M Python 등을 지원
   - 빌트툴인 Ant Builder와 Groovy 스크립트 기반으로 만들어져 기존 Ant의 역할과 배포 스크립트의 기능을 모두 사용 가능
3. 차이
   - 스크립트 길이와 가독성 면에서 gradle이 우세
   - 빌드와 테스트 실행 결과 gradle이 더 빠름
   - 의존성이 늘어날 수록 성능과 스크립트 품질의 차이가 심해질 것

[참고 블로그](https://dev-coco.tistory.com/65#Gradle%EC%-D%B-%EB%-E%--%-F)

[공식문서](https://spring.io/quickstart)

# 📌 스프링부트 도구 설치하기

## Spring Boot Devtool

- 스프링부트 개발시 도움을 주는 도구

- 서버 재시작 없이도 클래스 변경시 서버가 자동으로 재기동

- `build.gradle` 파일 - `dependencies` 에 추가

- ```
  developmentOnly 'org.springframework.boot:spring-boot-devtools'
  ```

- 이 후  [Gradle - Refresh Gradle Project] 클릭해 라이브러리를 다운로드



## Lombok

- 자바 클래스에 Getter, Setter, 생성자 등을 자동으로 만들어 주는 도구

- https://projectlombok.org/download 

- 다운로드, 설치 후 `build.gradle` 파일 - `dependencies` 에 추가

- ```
  compileOnly 'org.projectlombok:lombok'
  annotationProcessor 'org.projectlombok:lombok'
  ```



# 📌 Controller

- src - main - java - com.example.backend(프로젝트명) 안에 
  - controller 패키지 - WebController 클래스 생성

``` java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MainController {

    @GetMapping("/hello")
    @ResponseBody
    public String index() {
    	return "hello";
    }
}
```

## @Controller

- 이 WebController 클래스는 스프링부트의 컨트롤러가 됨

## @GetMapping

- 요청된 URL과의 매핑을 담당

## @ResponseBody

- URL요청에 대한 응답으로 문자열 리턴하라는 의미

- 이 이노테이션 생략하면 hello라는 템플릿 파일을 찾게되는 것

- ex) http://localhost:8080/hello 입력시 hello가 나오는 것을 볼 수 있음

  들어가서 확인해보자! 




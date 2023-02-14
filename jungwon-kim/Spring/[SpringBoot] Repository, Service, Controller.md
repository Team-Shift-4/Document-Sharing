[toc]



대부분의 규모있는 스프링부트 프로젝트는 

컨트롤러에서 리포지터리를 직접 호출하지 않고 중간에 서비스(Service)를 두어 데이터를 처리

`Controller -> Service -> Repository` 구조



<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyW031%2FbtqGvdxw1uy%2FfUvpO1u7geOUk5exDaoNT1%2Fimg.png" style="zoom: 67%;" />

# 📌 Repository

- Entity를 통해 데이터 테이블이 생성이 되면, 받아온 정보를 데이터베이스에 저장하고 조회하는 기능

- 실제 데이터베이스에 쿼리문 실행

# 📌 Service

- 스프링에서 데이터 처리를 위해 작성하는 클래스

- Repository에서 얻어온 정보를 자바로 가공 후 다시 Controller에게 정보를 보냄



## Service 필요한 이유

### 1. 모듈화

### 2. 보안

### 3. 앤티티 객체와 DTO 객체의 변환

[참고](https://wikidocs.net/161220)

# 📌 Controller

- 클라이언트 측의 요청이 가장 먼저 서버 측과 맞닿는 부분
- 데이터를 클라이언트에게 보낼 때도 컨트롤러를 통해 보냄




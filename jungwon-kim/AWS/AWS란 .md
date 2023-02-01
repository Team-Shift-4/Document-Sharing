[toc]

#  📌Servlet

- 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종
- 클라이언트가 어떠한 요청을 하면 그에 대한 결과를 다시 전송해주어야 하는데, 이러한 역할을 하는 자바 프로그램
- WAS내의 서블릿 컨테이너에서 동작
- 요청(Request)을 받으면 요청에 맞는 로직을 실행하고 클라이언트에게 HTTP 형식으로 응답(Response)

## Servlet 동작 방식

> ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F993A7F335A04179D20)
>
> 1. 사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송합니다.
> 2. 요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성합니다.
> 3. web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다.
> 4. 해당 서블릿에서 service메소드를 호출한 후 클리아언트의 GET, POST여부에 따라 doGet() 또는 doPost()를 호출합니다.
> 5. doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보냅니다.
> 6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킵니다.

[출처](https://mangkyu.tistory.com/14)

## Servlet Container(서블릿 컨테이너)

- 서블릿을 관리해주는 컨테이너
- 서블릿 컨테이너는 클라이언트의 요청(Request)을 받아주고 응답(Response)할 수 있게, 웹서버와 소켓으로 통신 
- 대표적인 예로 톰캣(Tomcat)
  - 실제로 웹 서버와 통신하여 JSP(자바 서버 페이지)와 Servlet이 작동하는 환경을 제공

# 📌 AWS(Amazon Web Service)

- 아마존(Amazon)에서 제공하는 클라우드 서비스
- 네트워킹을 기반으로 가상 컴퓨터와 스토리지, 네트워크 인프라 등 다양한 서비스를 제공

서블릿 기능 지원

클라우드가 서버 작동 관리, 메모리에 유연



# 📌 클라우딩 컴퓨팅

### 클라우드 컴퓨팅?

- IT 리소스를 인터넷을 통해 온디맨드로 제공하고 사용한 만큼만 비용을 지불하는 것

### 클라우드 컴퓨팅의 이점

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNuwij%2FbtqFzc7gI37%2FVElWL1PusZaZIRSEAvpKlK%2Fimg.jpg" style="zoom: 67%;" />

- 민첩성 
  - 클라우드를 통해 광범위한 기술에 쉽게 액세스할 수 있으므로, 더 빠르게 혁신하고 상상할 수 있는 거의 모든 것을 구축할 수 있습니다.
- 탄력성
- 비용 절감
- 몇 분 만에 전 세계에 배포

[출처: AWS](https://aws.amazon.com/ko/what-is-cloud-computing/)



## 클라우드 컴퓨팅 유형

### Infrastructure as a Service(IaaS)

### Platform as a Service(PaaS)

### Software as a Service(SaaS)



> \- **SaaS** (Software as a Service): 서비스에서 호스팅 방식으로 소프트웨어를 제공하는 것
>
>  일반적으로 웹에 접속을 하여 로그인등을 통해 사용할 수 있게 되는 서비스다. 대표적으로 웹 메일, 구글 클라우드, 네이버 클라우드, MS OFFICE 365 등이 있다.
>
>  
>
> \- **PaaS** (Platform as a Service): 플랫폼을 빌려 개발할 수 있게 해주는 서비스
>
>  응용 프로그램 자체를 제공하기 때문에 개발자는 빠르고 안전하게 할 수 있다. 또한 데이터베이스 관리, 운영체제 등이 포함된다. 대표적으로 구글 앱 엔진, 오라클 클라우드 플랫폼 등이 있다.
>
>  
>
> \- **IaaS** (Infrastructure as a Service): 인프라스트럭처, 즉 기반 시설 (서버, 스토리지 등)을 빌려주는 서비스
>
>  이 글에서 다룰 AWS처럼 사용한 만큼의 비용을 지불하며 스토리지, 호스팅, 컴퓨팅, 네트워킹등의 인프라를 빌려 사용하는 것이다.
>
> [출처](https://jaymunsh.tistory.com/29)
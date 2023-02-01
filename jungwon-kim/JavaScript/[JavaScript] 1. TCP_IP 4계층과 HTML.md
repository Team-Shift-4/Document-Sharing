- 네트워크 통신은 OSI 7계층의 방식으로 체계적으로 다루고 있고,

  OSI 7계층 이론을 실제 사용하는 인터넷 표준이 TCP/IP 4계층 방식이다.
  
  `OSI 7 계층?`
  
  -   네트워크 프로토콜이 통신하는 구조를 7개의 계층으로 분리하여 각 계층간 상호 작동하는 방식을 정해 놓은 것
  
  > ** OSI 모델과 TCP/IP 모델 비교**
  >
  > - TCP/IP 프로토콜은 OSI 모델보다 먼저 개발되었다. 그러므로 TCP/IP 프로토콜의 계층은 OSI 모델의 계층과 정확하게 일치하지 않는다.
  >
  > - 두 계층을 비교할 때 , 세션(Session)과 표현(presentation) 2개의 계층이 TCP/IP프로토콜 그룹에 없다는 것을 알 수 있다.
  >
  > - 두 모델 모두 계층형 이라는 공통점을 가지고 있으며 **TCP/IP는 인터넷 개발 이후 계속 표준화되어 신뢰성이 우수**인 반면, OSI 7 Layer는 표준이 되기는 하지만 실제적으로 구현되는 예가 거의 없어 신뢰성이 저하되어있다.
  >
  > - OSI 7 Layer는 장비 개발과 통신 자체를 어떻게 표준으로 잡을지 사용되는 반면에 실 질적인 통신 자체는 TCP/IP 프로토콜을 사용한다.
  >
  > 출처: [http://goitgo.tistory.com/25](http://goitgo.tistory.com/25)
  
  
  
  ---
  
  ## 📌 TCP/IP 4계층
  
  **현재의 인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 통신규약 (프로토콜)의 모음**
  
  4층 - 애플리케이션 계층(HTTP, FTP, DNS, SMTP)
  
  3층 - 전송 계층(TCP, UDP)
  
  2층 - 인터넷 계층(IP)
  
  1층 - 네트워크 엑세스 계층(이더넷)
  
  ### IP(인터넷 프로토콜)
  
  -   지정한 IP주소에 패킷을 빨리 목적지로 보내는 역할
  -   순서가 바뀌거나 누락되어도 상관X
  -   \=> 패킷 순서를 보장할 수 없고, 사라질 수도 있음
  
  ### TCP(전송 제어 프로토콜)
  
  -   데이터 주고받는 쌍방이 서로 인지(신뢰, 연결 지향적)
  -   패킷 데이터를 보낸 순서대로 받음
  -   중간에 없어졌거나 망가지면 다시 요청하는 방식
  -   like 전화
  
  ### UDP
  
  -   상대방을 정하지 않고 뿌리고, 받는 쪽에서 관심이 있으면 받음 (비신뢰, 비연결 지향적)
  -   like 라디오
  
  ### HTTP
  
  -   **HyperText Transfer Protocol**
  -   서버나 클라이언트 응용 프로그램 동작
  -   IP 주소를 이용해서 접근하는 건 똑같으나 1회용 통신
      -   응답하고 끊어버림 (요청 - 응답 - 통신 끝)
      -   file로 응답(리턴)
          -   text
              -   파일을 열었을 때 사람이 읽을 수 있는 것
              -   ex) html, css, js 등 글자로 읽을 수 있는 것
              -   새로 고침해도 맨날 똑같은 게 오는 게 웹서버 html (같은 파일) : 이게 아파치 서버
              -   새로 고침해도 다른 게 오면 웹 애플리케이션 서버(웹 앱 서버) - 이게 node, 스프링
              -   **얘네는 종류를 알려줘야 함 - content type**
          -   binary
              -   text 외 이미지나 동영상 등 읽을 수 없는 것(원본 그대로 날아감)
              
              
  
  ---
  
  # 📌 웹 브라우저 특성
  
  -   HTTP는 file로 응답하는데 text는 구체적인 종류를 알아야 하기 때문에 content type을 알려주고,
      -   jsp 파일 같은 경우엔 톰캣이 자동으로 렌더링함
  -   서버에서 Content-Type을 text/html로 전송하면 렌더링을 진행
  -   **제일 윗 줄부터 해석**
      -   css, js 등 있으면 다운로드해서 사용
  -   해석하면서 메모리에 쌓아두는 데 이게 DOM (DOM Tree)
  
  ## DOM
  
  > 문서 객체 모델(The Document Object Model, 이하 DOM) 은 HTML, XML 문서의 프로그래밍 interface 이다. DOM은 문서의 구조화된 표현(structured representation)을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다.
  
  [WDN 보러가기](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)
  
  ![](http://www.tcpschool.com/lectures/img_js_htmldom.png)
  
  출처: [TCP School](http://www.tcpschool.com/javascript/js_dom_concept)
  
  ## Document
  
  -   자바스크립트 입장에서 나를 부른 문서(html)
  -   노드 js 는 서버에 있는거라서 Document가 없음
  
  ---
  
  # 📌 HTML
  
  ### Hyper-Text Markup Language
  
  -   `Hyper-Text`
      -   다른 문서로 이동할 수 있는(링크)
  -   `Markup Language`
      -   어떤 문서의 **구조**를 나타내는 데 사용
      -   어디서 어디까지가 리스트인 지, 제목인 지 등을 알 수 있게 하는 것
  
  #### element
  
  -   시작부터 끝을 지정
  
  #### Attribute
  
  -   element의 추가 정보
  -   Attribute 앞에는 반드시 띄어쓰기 한 칸 필요, 따옴표로 묶여야
  
  #### id
  
  -   element의 식별자
  -   html 파일 안에서 유일
  
  #### class
  
  -   분류
  -   여러 개 가능(공백으로 구분)
  
  #### name
  
  -   form에서 사용
  -   서버에서 받을 때 변수 이름으로 사용
  
  ### HTML 선언 방법
  
  HTML5
  
  ```
  <!DOCTYPE html>
  ```
  
  HTML4
  
  ```
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  ```
  
  [HTML태그 연습해보기](https://www.w3schools.com/html/)
  
  ### 예제
  
  ```
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  
  <body>
      <form>
          <table>
              <tr>
                  <td><label for="userid">아이디</label></td>
                  <td><input type="text" name="userid" id="userid"></td>
              </tr>
              <tr>
                  <td><label for="userpw">비밀번호</label></td>
                  <td><input type="password" name="userpw" id="userpw"></td>
              </tr>
              <tr>
                  <td colspan="2"><input type="submit" value="확인"></td>
              </tr>
          </table>
     </form>
  </body>
  
  </html>
  ```
  
  -   form 안에 table 넣어서 줄 맞출 때 많이 씀
  -   반응형을 생각하면 label 추가 해주는 게 좋다

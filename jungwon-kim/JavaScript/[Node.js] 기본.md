[toc]

***



# 📌 Node.js

노드는 그냥 네트워크 서버, 범용!! ( 웹서버 전용X )

http, https, tcp 등 다 가능



``` javascript
const http = require('http');
const server = http.createServer((request, response) => {
    response.writeHead('200', {'Content-Type': 'text/html'});
    response.end('<html><head></head><body><h1>Hello<h1></body></html>');
});

server.listen(3000);
```

- `response.writeHead()` `response.setHeader()`
  - 200은 OK, 컨텐트타입 적어줘야 html을 렌더링함 
- `response.end()` : 이걸 끝으로 응답
- `server.listen(3000)` : 3000번 서버 켜는거



## 간단한 라우팅

``` javascript
const http = require('http');
const server = http.createServer((request, response) => {
    response.writeHead('200', {'Content-Type': 'text/html'});

    if(request.url == '/hello'){
        response.end('<html><head></head><body><h1>Hello<h1></body></html>');
    } else {
        response.end('<html><head></head><body><h1>Welcome<h1></body></html>')
    }
});

server.listen(3000);
```



## Dependency 관리

`npm init` 혹은 `package.json` 파일 생성



## package 관리

```
npm install 모듈이름
npm i 모듈이름

npm install -g 모듈이름 (전역 설치)
```

- node_modules 폴더에 추가
- 전역설치하면 노드모듈에 추가안되므로 팀프일땐 주의
- npm install 하면 `package.json`에 있는 데 나한테 없는 거  다운해줌
  - 깃허브에 올릴 땐 node_modules 폴더 X



# Express

- 라우팅, static resource, 뷰 렌더링을 대신 해줌
- `http.createServer()` 대신 express() 로 생선된 객체를 전달
- 최근엔 nest.js 사용하기도 함

```
npm i express
```



## 예제

``` javascript
// app.js 파일

const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Home with express');
});

app.get('/hello', (req, res)=>{
    res.send('Hello with express');
});

module.exports = app;
```

- app이 express 를 받아서 내용 넣고 exports 해줌
- `get` : '/' 주소가 들어오면  콜백을 얘기해라



``` javascript
// index.js 파일에서 app을 가져와서 사용

const http = require('http');
const app = require('./app');

const server = http.createServer(app); // 만들어둔 처리 코드 app을 가져옴
const port = 3000;
server.listen(port);
```

- ` http.createServer()` 에 직접 코딩 대신 express 객체인 app을 전달



# Router 분리(모듈화)

``` javascript
// index.js

const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
    res.send('Welcome');
})

module.exports = router;
```

``` javascript
// hello.js

const express = require('express');
const router = express.Router();

router.get('/' , (req, res) => {
    res.send('Hello');
});

module.exports = router;
```

``` javascript
// app.js

const express = require('express');

const indexRouter = require('./routes/index');
const helloRouter = require('./routes/hello')

const app = express();

app.use('/', indexRouter);
app.use('/hello', helloRouter);

module.exports = app;
```



# Project Scaffolding

- node.js 는 프로젝트 구조에 대한 제약이 없음 ⇒ 뼈대를 잡아주는 작업!
- 사이트 추천
  - [yeoman](https://yeoman.io/generators/)
  - [express generator](https://expressjs.com/en/starter/generator.html)



## express generator

1. **전역으로 설치**
   - `npm i -g express-generator` - `express myapp`
   - 자주 사용할 때

2. **npx**

   - 일회용(새 프로젝트 생성시 지움)

   - 자주 사용하지 않을 때

   - 새 폴더를 만들어주기 때문에 내가 만들고자 하는 폴더 한 단계 상위에서 만들어야 함

   - `npx express-generator --view=pug 프로젝트명 `

     - pug 혹은 ejs

     - 생성 후 package.json 가서 dependencies 버전 올려야 함 

       (저는 수업 시간에 진행한 버전으로 수정했습니다)

       ```json
        "dependencies": {
           "cookie-parser": "~1.4.6",
           "debug": "~4.3.3",
           "express": "~4.17.2",
           "http-errors": "~2.0.0",
           "morgan": "~1.10.0",
           "pug": "3.0.2"
         }
       ```

     - 이 후 `npm i`로 버전 업그레이드



## pug

- 들여쓰기로 태그 구분

- layout.pug 에 틀 만들어두고 다른 파일에서 만든거 불러옴 

- `extends layout` 키워드로 보냄



# 👩‍💻 

# DB

## SQLite

- DBMS 설치없어도 가능
- DB를 생성하면 파일이 생기고, 파일 삭제하면 데이터 삭제됨
- 작음
- `npm install sqlite3`

## MariaDB

- `npm install mariadb`
- priomise, Async/await 다 가능

## MySQL

- `npm install mysql2`
- mysql2는 Async/await 지원



Connection Pool? DBCP?

[Sequelize](https://sequelize.org/docs/v6/getting-started/)
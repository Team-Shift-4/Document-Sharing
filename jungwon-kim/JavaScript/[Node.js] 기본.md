[toc]

***



# π Node.js

λΈλλ κ·Έλ₯ λ€νΈμν¬ μλ², λ²μ©!! ( μΉμλ² μ μ©X )

http, https, tcp λ± λ€ κ°λ₯



``` javascript
const http = require('http');
const server = http.createServer((request, response) => {
    response.writeHead('200', {'Content-Type': 'text/html'});
    response.end('<html><head></head><body><h1>Hello<h1></body></html>');
});

server.listen(3000);
```

- `response.writeHead()` `response.setHeader()`
  - 200μ OK, μ»¨ννΈνμ μ μ΄μ€μΌ htmlμ λ λλ§ν¨ 
- `response.end()` : μ΄κ±Έ λμΌλ‘ μλ΅
- `server.listen(3000)` : 3000λ² μλ² μΌλκ±°



## κ°λ¨ν λΌμ°ν

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



## Dependency κ΄λ¦¬

`npm init` νΉμ `package.json` νμΌ μμ±



## package κ΄λ¦¬

```
npm install λͺ¨λμ΄λ¦
npm i λͺ¨λμ΄λ¦

npm install -g λͺ¨λμ΄λ¦ (μ μ­ μ€μΉ)
```

- node_modules ν΄λμ μΆκ°
- μ μ­μ€μΉνλ©΄ λΈλλͺ¨λμ μΆκ°μλλ―λ‘ ννμΌλ μ£Όμ
- npm install νλ©΄ `package.json`μ μλ λ° λνν μλ κ±°  λ€μ΄ν΄μ€
  - κΉνλΈμ μ¬λ¦΄ λ node_modules ν΄λ X



# Express

- λΌμ°ν, static resource, λ·° λ λλ§μ λμ  ν΄μ€
- `http.createServer()` λμ  express() λ‘ μμ λ κ°μ²΄λ₯Ό μ λ¬
- μ΅κ·Όμ nest.js μ¬μ©νκΈ°λ ν¨

```
npm i express
```



## μμ 

``` javascript
// app.js νμΌ

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

- appμ΄ express λ₯Ό λ°μμ λ΄μ© λ£κ³  exports ν΄μ€
- `get` : '/' μ£Όμκ° λ€μ΄μ€λ©΄  μ½λ°±μ μκΈ°ν΄λΌ



``` javascript
// index.js νμΌμμ appμ κ°μ Έμμ μ¬μ©

const http = require('http');
const app = require('./app');

const server = http.createServer(app); // λ§λ€μ΄λ μ²λ¦¬ μ½λ appμ κ°μ Έμ΄
const port = 3000;
server.listen(port);
```

- ` http.createServer()` μ μ§μ  μ½λ© λμ  express κ°μ²΄μΈ appμ μ λ¬



# Router λΆλ¦¬(λͺ¨λν)

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

- node.js λ νλ‘μ νΈ κ΅¬μ‘°μ λν μ μ½μ΄ μμ β λΌλλ₯Ό μ‘μμ£Όλ μμ!
- μ¬μ΄νΈ μΆμ²
  - [yeoman](https://yeoman.io/generators/)
  - [express generator](https://expressjs.com/en/starter/generator.html)



## express generator

1. **μ μ­μΌλ‘ μ€μΉ**
   - `npm i -g express-generator` - `express myapp`
   - μμ£Ό μ¬μ©ν  λ

2. **npx**

   - μΌνμ©(μ νλ‘μ νΈ μμ±μ μ§μ)

   - μμ£Ό μ¬μ©νμ§ μμ λ

   - μ ν΄λλ₯Ό λ§λ€μ΄μ£ΌκΈ° λλ¬Έμ λ΄κ° λ§λ€κ³ μ νλ ν΄λ ν λ¨κ³ μμμμ λ§λ€μ΄μΌ ν¨

   - `npx express-generator --view=pug νλ‘μ νΈλͺ `

     - pug νΉμ ejs

     - μμ± ν package.json κ°μ dependencies λ²μ  μ¬λ €μΌ ν¨ 

       (μ λ μμ μκ°μ μ§νν λ²μ μΌλ‘ μμ νμ΅λλ€)

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

     - μ΄ ν `npm i`λ‘ λ²μ  μκ·Έλ μ΄λ



## pug

- λ€μ¬μ°κΈ°λ‘ νκ·Έ κ΅¬λΆ

- layout.pug μ ν λ§λ€μ΄λκ³  λ€λ₯Έ νμΌμμ λ§λ κ±° λΆλ¬μ΄ 

- `extends layout` ν€μλλ‘ λ³΄λ



# π©βπ» 

# DB

## SQLite

- DBMS μ€μΉμμ΄λ κ°λ₯
- DBλ₯Ό μμ±νλ©΄ νμΌμ΄ μκΈ°κ³ , νμΌ μ­μ νλ©΄ λ°μ΄ν° μ­μ λ¨
- μμ
- `npm install sqlite3`

## MariaDB

- `npm install mariadb`
- priomise, Async/await λ€ κ°λ₯

## MySQL

- `npm install mysql2`
- mysql2λ Async/await μ§μ



Connection Pool? DBCP?

[Sequelize](https://sequelize.org/docs/v6/getting-started/)
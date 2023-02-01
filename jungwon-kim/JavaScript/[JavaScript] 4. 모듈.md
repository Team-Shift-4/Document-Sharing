# 📌 모듈

 

## 모듈?

함수, 변수, 클래스 등으로 이루어진 파일이나 라이브러리

> 개발하는 애플리케이션의 크기가 커지면 언젠간 파일을 여러 개로 분리해야 하는 시점이 옵니다. 이때 분리된 파일 각각을 '모듈(module)'이라고 부르는데, 모듈은 대개 클래스 하나 혹은 특정한 목적을 가진 복수의 함수로 구성된 라이브러리 하나로 구성됩니다.
>
> [출처: 모던자바스크립트](https://ko.javascript.info/modules-intro)

***

## 1. CommonJS

- Node.js 의 default
- require 키워드 사용

-  내보내기 - `module.exports` 
- 사용하기 - `require('모듈')`

## 2. ES6 Module

-  ES6(ES2015)에서 새롭게 도입된 키워드
- 내보내기 - `export`
- 사용하기 
  - `import 이름 from 파일`
  - 일부만 : `import { 항목 } from 모듈`



- 서버에서 돌아가는 코드 - 모듈 지원함
- 웹 브라우저에서 사용되는 코드인 지(빈칸 있는지, 숫자만 들어갔는지 등 html) - import X



공통적으로 사용하고 싶으면 node를 **ES6 Module**로 바꿔서 사용하면 됨

###  👉설정 추가하는 법

``` javascript 
// package.json
{
    "type": "module"
}
```



## 📌 CommonJS - module.exports - require

방법은 여러가지임

``` javascript
// 1
module.exports = {
    userId : 'wonnykim',
    name : '이름'
 };

// 2
module.exports.userId='wonnykim';
module.exports.name='이름'; 

// 3
const user = {
   userId : 'wonnykim',
   name : '이름'
};

module.exports = user;
```



### 내보내기(module.exports)

``` javascript
// test1.js 파일

const data = {
    user: 'abc',
    role: 'admin',
    sayHello() {
        console.log('Hello!');
    }
};

module.exports = data; // 다른 파일에선 얘만 쓸 수 있는 것
```

``` javascript
// test2.js 파일

// 항목을 하나씩 exports 객체에 추가하는 방법
module.exports.user = 'abc';

module.exports.role = 'admin';

module.exports.sayHello = (user) => {
   console.log(`Hello! ${user}`);
};
```



### 불러쓰기(require)

``` javascript
const test1 = require('./test1');	// 확장자는 생략 가능
const test2 = require('./test2');

console.log(test1); // { user: 'abc', role: 'admin', sayHello: [Function: sayHello] }
test1.sayHello(); // Hello!

console.log(test2);	// { user: 'abc', role: 'admin', sayHello: [Function (anonymous)] }
test2.sayHello('wonny'); // Hello! wonny
```

- require 시 폴더 이름만 있고 파일 이름이 적혀있지 않으면 index.js를 자동으로 가져옴

  ⇒ 이 경우 그 폴더안에 있는 require를 index.js에 다 하면 됨



## 📌 ES6 Module - export - import 문

### 내보내기(export)

```javascript
// test1.js

export default {
    user: 'abc',
    role: 'admin',
    sayHello() {
        console.log('Hello!');
    }
};
```

``` javascript
// test2.js
// 항목을 하나씩 export 
const message = 'hi';

const hello = (user)=> {
   console.log(`Hello ${user}`);
};

const bye = () => {
   console.log('Bye');
};

export {
   message,
   hello,
   bye
}
```



### 불러쓰기 (import)

``` javascript
// main.js
import test from './test1.js';
import { message, hello, bye } from './test2.js';

console.log(test);
test.sayHello();

console.log(message);
hello(test.user); // 구조 분해 할당으로 받아서 모듈 이름없이 씀
bye();
```

- 구조 분해 할당으로 받을 경우 쓸 때 모듈 이름 없이 쓰면 됨

  ⇒ require 를 쓸 때는 구조 분해 할당으로 받을 수 없고 통째로 받아야 함


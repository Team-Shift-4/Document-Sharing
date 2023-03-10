# π λͺ¨λ

 

## λͺ¨λ?

ν¨μ, λ³μ, ν΄λμ€ λ±μΌλ‘ μ΄λ£¨μ΄μ§ νμΌμ΄λ λΌμ΄λΈλ¬λ¦¬

> κ°λ°νλ μ νλ¦¬μΌμ΄μμ ν¬κΈ°κ° μ»€μ§λ©΄ μΈμ  κ° νμΌμ μ¬λ¬ κ°λ‘ λΆλ¦¬ν΄μΌ νλ μμ μ΄ μ΅λλ€. μ΄λ λΆλ¦¬λ νμΌ κ°κ°μ 'λͺ¨λ(module)'μ΄λΌκ³  λΆλ₯΄λλ°, λͺ¨λμ λκ° ν΄λμ€ νλ νΉμ νΉμ ν λͺ©μ μ κ°μ§ λ³΅μμ ν¨μλ‘ κ΅¬μ±λ λΌμ΄λΈλ¬λ¦¬ νλλ‘ κ΅¬μ±λ©λλ€.
>
> [μΆμ²: λͺ¨λμλ°μ€ν¬λ¦½νΈ](https://ko.javascript.info/modules-intro)

***

## 1. CommonJS

- Node.js μ default
- require ν€μλ μ¬μ©

-  λ΄λ³΄λ΄κΈ° - `module.exports` 
- μ¬μ©νκΈ° - `require('λͺ¨λ')`

## 2. ES6 Module

-  ES6(ES2015)μμ μλ‘­κ² λμλ ν€μλ
- λ΄λ³΄λ΄κΈ° - `export`
- μ¬μ©νκΈ° 
  - `import μ΄λ¦ from νμΌ`
  - μΌλΆλ§ : `import { ν­λͺ© } from λͺ¨λ`



- μλ²μμ λμκ°λ μ½λ - λͺ¨λ μ§μν¨
- μΉ λΈλΌμ°μ μμ μ¬μ©λλ μ½λμΈ μ§(λΉμΉΈ μλμ§, μ«μλ§ λ€μ΄κ°λμ§ λ± html) - import X



κ³΅ν΅μ μΌλ‘ μ¬μ©νκ³  μΆμΌλ©΄ nodeλ₯Ό **ES6 Module**λ‘ λ°κΏμ μ¬μ©νλ©΄ λ¨

###  πμ€μ  μΆκ°νλ λ²

``` javascript 
// package.json
{
    "type": "module"
}
```



## π CommonJS - module.exports - require

λ°©λ²μ μ¬λ¬κ°μ§μ

``` javascript
// 1
module.exports = {
    userId : 'wonnykim',
    name : 'μ΄λ¦'
 };

// 2
module.exports.userId='wonnykim';
module.exports.name='μ΄λ¦'; 

// 3
const user = {
   userId : 'wonnykim',
   name : 'μ΄λ¦'
};

module.exports = user;
```



### λ΄λ³΄λ΄κΈ°(module.exports)

``` javascript
// test1.js νμΌ

const data = {
    user: 'abc',
    role: 'admin',
    sayHello() {
        console.log('Hello!');
    }
};

module.exports = data; // λ€λ₯Έ νμΌμμ  μλ§ μΈ μ μλ κ²
```

``` javascript
// test2.js νμΌ

// ν­λͺ©μ νλμ© exports κ°μ²΄μ μΆκ°νλ λ°©λ²
module.exports.user = 'abc';

module.exports.role = 'admin';

module.exports.sayHello = (user) => {
   console.log(`Hello! ${user}`);
};
```



### λΆλ¬μ°κΈ°(require)

``` javascript
const test1 = require('./test1');	// νμ₯μλ μλ΅ κ°λ₯
const test2 = require('./test2');

console.log(test1); // { user: 'abc', role: 'admin', sayHello: [Function: sayHello] }
test1.sayHello(); // Hello!

console.log(test2);	// { user: 'abc', role: 'admin', sayHello: [Function (anonymous)] }
test2.sayHello('wonny'); // Hello! wonny
```

- require μ ν΄λ μ΄λ¦λ§ μκ³  νμΌ μ΄λ¦μ΄ μ νμμ§ μμΌλ©΄ index.jsλ₯Ό μλμΌλ‘ κ°μ Έμ΄

  β μ΄ κ²½μ° κ·Έ ν΄λμμ μλ requireλ₯Ό index.jsμ λ€ νλ©΄ λ¨



## π ES6 Module - export - import λ¬Έ

### λ΄λ³΄λ΄κΈ°(export)

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
// ν­λͺ©μ νλμ© export 
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



### λΆλ¬μ°κΈ° (import)

``` javascript
// main.js
import test from './test1.js';
import { message, hello, bye } from './test2.js';

console.log(test);
test.sayHello();

console.log(message);
hello(test.user); // κ΅¬μ‘° λΆν΄ ν λΉμΌλ‘ λ°μμ λͺ¨λ μ΄λ¦μμ΄ μ
bye();
```

- κ΅¬μ‘° λΆν΄ ν λΉμΌλ‘ λ°μ κ²½μ° μΈ λ λͺ¨λ μ΄λ¦ μμ΄ μ°λ©΄ λ¨

  β require λ₯Ό μΈ λλ κ΅¬μ‘° λΆν΄ ν λΉμΌλ‘ λ°μ μ μκ³  ν΅μ§Έλ‘ λ°μμΌ ν¨


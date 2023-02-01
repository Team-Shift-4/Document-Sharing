실제 개발할 때 제일 많이 쓰이는 부분

# 📌 함수

함수형 언어의 특징?

- 함수랑 클래스를 동급으로 취급함

- 함수 안에 함수 가능

  

## 1. 선언문

`function`

``` javascript
function square(n){
    return n*n;
}
```

* return이 생략되면 **undefined!**
* **Hoisting** 적용
  * 자바 클래스에서 함수 선언할때 위에 있는 애가 밑에 있는 함수 불러쓸 수 있는 거랑 똑같음



### Hoisting?

- 함수의 '선언'을 스코프의 제일 앞으로 끌어 올리는 동작
- var, function 키워드

``` javascript
console.log(cat);  // undefined
var cat = 'cat';
var cat = 'new cat';
console.log(cat);	// new cat


console.log(hello());	// Hello
function hello() {
    return 'Hello';
}
```



## 2. 표현식

- 익명 함수를 만들어서 변수에 저장하는 것

- 일반적으로 한번 선언되면 내용이 잘 안바뀌므로 **const** 에 저장

``` javascript
const hi = function sayHi(name) { // 이름을 또 붙여줘도 되지만 sayHi() 불러서 쓰진 못함
    console.log('Hi '+name);
}

const bye = function(name) {
    console.log('Bye '+name);
}

hi('user');		// Hi user
bye('user');	// Bye user
```



## 3. 람다 "식"

- lambda expression
- 함수는 아니지만 파라미터를 받아서 새로운 값 리턴! 
  - a+b 와 (a, b) { return a+b; } 는 같음
  - 함수가 아니라, 사칙연산과 더 동급, **함수를 대체할 수 있는 연산이다!!!**
  - 다만 { } 사용으로 복잡한 문법을 수행할 준비가 되어있음

``` javascript
const hello = (name) => {
    console.log('Hello '+name);
}

hello('user');
```

- 무조건 리턴해줘야 하는데, 이 경우는 undefined 리턴하는 것
- 결국 ' => ' 는 매개변수를 받아서 { } 의 일을 하겠다는 의미

``` 
(name) => {	}	// javascript
(name) -> { }	// java
{ name -> 	}	// kotlin
lambda name: { }	// python
```



## 4. 일회용 함수

``` javascript
(function() {
    console.log('test');
})();
```

``` javascript
(()=> {
    console.log('test');
})();
```

- 한 번만 호출가능함
- 람다식도 가능



***

# 📌 객체

- 중괄호로 묶인 0개 이상의 속성 이름과 속성 값

  

## 1. 객체 속성 이름 규칙

  - $, _ , 유니코드 문자, 숫자 (숫자는 첫 글자에 불가능)
  - 그 외의 속성 이름 `[속성이름]:값`

  

## 2. 객체 불러쓰는 법

- 객체.이름
- 객체[이름]



## 3. 객체 생성시 주의할 점

### 1. 객체안에 객체로 추가

``` javascript
const obj = {
    values: { }
}
obj.values[0]= 'first';
obj.values[1]='second';

console.log(obj);	// { values: { '0': 'first', '1': 'second' } }
console.log(obj.values[0]);		//first
console.log(obj.values.length);		// undefined

delete obj.values[0];

console.log(obj); 	// { values: { '1': 'second' } }
console.log(obj.values[0]);  	// undefined
```
- 요소를 부르는 방법은 2번과 같음
- 길이를 알 수 없음
- 중간에 하나를 제거하면 인덱스 번호도 꼬임




### 2. 객체안에 배열로 추가

``` javascript
const obj = {
    values: [ ]
}
obj.values.push('first');
obj.values.push('second');

console.log(obj);	// { values: [ 'first', 'second' ] }
console.log(obj.values[0]);		// first
console.log(obj.values.length);		// 2
```

보통 이렇게 코드를 짜는 경우가 주문 옵션시 체크하면 재고나 수량이 바뀔 때 이런 방법을 사용함

프론트 엔진들은 이미 있는 변수에 대한 변화를 감지해서  ui를 새로 그리게 설정되어 있음 ( 배열의 경우 길이 탐지 )

1번으로 코드를 짜면 새로운 프로퍼티가 추가되는거라서 엔진이 감지 못 함

[Vue.js State Management](https://vuejs.org/guide/scaling-up/state-management.html)



## 4. 객체 생성

### 1. 객체 리터럴 방식 사용

``` javascript
const value = 'value'

const obj = {
    id: 1,
    _desc_: 'my obj',
    value, 		// value:value
    '?': '?',	// 유니코드 문자라서 따옴표도 가능
    [symbol]: 'abc',
    [value + (()=>1)()] : 1		// 동적 이름
}
```



### 2. new 연산자

- 자바스크립트에서 함수는 하나의 객체므로 함수와 new 연산자를 이용하면 객체 생성됨

``` javascript
// 객체를 만들기 위해 함수를 선언할 때는 대문자로 생성할 것
function User(name) {
    this.userName = name;
}
const user1=new User('won'); 
console.log(user1); 	// User { userName: 'won' }
```



## 5. 객체의 속성

만들어진 이 후에도 객체 속성을 추가하거나 삭제가 가능함 => 오타 주의!!

``` javascript
const obj = {
    id:1,
    arr:[1,2,3]
}
console.log(obj); // { id: 1, arr: [ 1, 2, 3 ] }

Object.defineProperty(obj, 'value', {
    configurable: false,
    enumerable: true, 
    value: 'abc',
    writable: false	
});

console.log(obj); // { id: 1, arr: [ 1, 2, 3 ], value: 'abc' }
console.log(delete obj.value); // false
console.log(obj); // { id: 1, arr: [ 1, 2, 3 ], value: 'abc' }
obj.value='123';
console.log(obj); // { id: 1, arr: [ 1, 2, 3 ], value: 'abc' }
```

수정/삭제 안되게 만들고 싶을 때 `Object.defineProperty()`

- `configurable` : 삭제하거나 접근성 변경 가능? (default - false)
- `enumerable` : 열거형인가? (default - false)
  - false 로 할 경우 console.log() 에서 확인X
- `writable` : 값 수정 가능? (default - false)



``` javascript
const rect = {
    width : 50,
    height : 50
}

Object.defineProperty(rect, 'area', {
    enumerable: true,
    get() { return this.width * this.height}
});

console.log(rect); // { width: 50, height: 50, area: [Getter] }
console.log(rect.area); // 2500

rect.width = 100;
console.log(rect.area); // 5000
```

- 이 경우 get() / set() 을 이용한 가상의 속성 사용 가능



## 6. Prototype

> 자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있다. 그리고 이것은 마치 객체 지향의 상속 개념과 같이 부모 객체의 프로퍼티 또는 메소드를 상속받아 사용할 수 있게 한다. 이러한 부모 객체를 **Prototype(프로토타입) 객체** 또는 줄여서 Prototype(프로토타입)이라 한다.
>
>[출처](https://poiemaweb.com/js-prototype)

***

# 📌 함수와 객체

- 함수의 파라미터는 원본이 아니고, 복사본⭐

- 원시타입(불변타입) 말고는 참조(reference, 주소 값) 전달 

  - 객체, 배열같은 경우

    => 주소도 복사본이지만 주소를 따라가서 바뀌니까 **원본도 바뀜**


## 1. 함수를 내부 객체로 선언

``` javascript
function outer() {
    function inner(){
        console.log('inner');
    }
    console.log('outer');
    inner();
    inner();
}
outer();
```

``` javascript
$(함수);

$(function(){  });

$(()=> {
   // 함수안에 함수 넣는 방식 <= jQuery
});
```



## 2. Generator

- `function*`
- next() 로 부르는 함수는 대부분 generator 함수임
- 자바로 치면 while(rs.next()) { } 같은 것 



## 3. 가변 매개변수

``` javascript
function test(...params){
    console.log(params.length);
}

test();		// 0
test(1,2,3); 	// 3
```



## 4. 객체의 함수

``` javascript
// ES6 문법
const obj = {
    id:1,
    test() {
        console.log(this.id);
    }
}

obj.test() 		// 1
```



## 5. 구조 분해 할당⭐

- 객체나 배열을 변수로 분해할 수 있게 해줌

``` javascript
function getInfo() {
    return {
        name: 'abc',
        price: 1000
    };
}

let {name, price} = getInfo();
console.log(name, price); 	// abc 1000
```

``` javascript
const {userid} = req.body;
```

- req.body.userid 입력해 찾아올 값을 userid 만 써서 불러 올 수 있음




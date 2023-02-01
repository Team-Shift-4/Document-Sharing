## ECMAScript

-   European Computer Manufacturer's Association
-   정보와 통신 시스템을 위한 국제적 표준화 기구
-   JavaScript와 같은 스크립트 언어의 표준

# 📌JavaScript

-   객체(object) 기반의 스크립트 언어
-   `ECMA - 262`의 규칙을 준수하는 언어
-   `ECMA 2015(ECMA6)` : V8 Engine의 기본 버전, 현재 버전
-   브라우저 별 지원 문법이 다름
-   [사이트에서 확인 (작성 날짜 기준 크롬은 올그린)](https://caniuse.com/es6)

## 👉기본 문법

-   파일 확장자 js
-   파일의 첫 줄부터 실행
-   문장 끝에 세미콜론을 생략하면 자동으로 삽입(ASI, Automatic Semicolon Insertion)
    -   특정한 경우에 추가되므로 적어주는 게 좋음

## 👉 변수

**선언과 할당은 분리해서 기억할 것**

### 1\. 원시타입(불변 타입)

-   let a="hello"; 일 때 불변 타입은 "hello"
    -   a="world"; 가 되면 hello가 사라지는 게 아니라 a가 다른 걸 만들어서 가르키는 것
    -   " "로 만드는 스트링은 무조건 메모리에 잡힘
-   boolean, null, undefined(선언은 되었으나 값이 없을 때), number, bigint, string, Symbol
-   자바스크립트의 숫자는 기본적으로 소수임

### 2\. 변수 선언과 할당

#### `var`

-   중복 선언 가능
-   자기를 감싸고있는 함수가 Scope라서 주의해야 함!​ => 0 1 2 3 출력
-   `function test() { for(var i=0; i<3; i++){ console.log(i); } console.log(i); }`
-   Hoisting 의 문제
  
    -   인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것
    -   var 로 선언한 변수의 경우 호이스팅 시 undefined로 변수를 초기화
    
    ```
      console.log(i);
      var i = 0;
    ```
    
     => undefined 출력
-   맨 위에 변수 할당을 다 했으면 써도 상관은 없음

#### `let, const`

-   **let** : 값을 여러 번 할당 가능 / **const** : 상수(선언 할당 동시에)
-   같은 범위 내 중복 선언 불가능
-   중괄호를 범위로 가짐
-   Hoisting X

### 3\. Symbol

-   내부적으로 절대 겹치지 않는 값을 생성
-   프로퍼티 네임으로도 만들 수 있음

### 4\. String Template Literals

```
let i = 0;
const str = `value is ${i+3}.`;
console.log(str); //value is 3

const str1=`Hello,
World`;
console.log(str1);
```

-   표현식은 `${expression}`
-   중괄호 생략 X
-   줄바꿈 적용 가능

## 👉연산자 주의할 점

[연산자 종류 자세히 보러가기](https://velog.io/@maxkmh/JavaScript-02-%EC%97%B0%EC%82%B0%EC%9E%90-%EC%A2%85%EB%A5%98)

### 1\. 비교연산자 주의할 점

```
console.log(1 == '1') // true
console.log(0 == false) // true
console.log('' == false) // true
console.log(undefined == 0) // false
console.log(null == 0) // false
console.log(undefined == null) // true 주의!
```

### 2\. 삼항 연산자 '?'

```
const score = 5;
let result = score > 2 ? true: false;

console.log(result); // true
```

### 3\. switch

-   switch는 타입에 엄격함 주의!

```
  const value = '1';

  switch(value) {
      case 1:
          console.log('case 1');
          break;
      default:
          console.log('default');
  }

  // default 출력
```

### 4\. 라벨

-   중첩된 반복문을 한번에 종료할 때(break, continue)
-   예약어 X
-   `continue` : 반복문에서만 라벨 사용 가능
-   `break` : 코드블록에 라벨 사용 가능

```
myBlock: {
    let i=0;
    console.log(i);
    if(i==0) break myBlock;
    console.log('Dead code...');
}

// 0 출력
```
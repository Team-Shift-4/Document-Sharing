## 노드

- Node.jsⓇ는 크롬 V8 자바스크립트 엔진으로 빌드된 **자바스크립트 런타임**

- 자바스크립트 런타임?

  - 자바스크립트를 실행할 수 있는 환경

- 노드는 서버와는 다르지만, 서버를 구성할 수 있게 하는 모듈(4장에서 설명)을 제공

  



## 노드의 특징

1. 이벤트 기반!
   - ex) 클릭, 네트워크 요청, 타이머 등

2. **논 블로킹 I/O**
   - 각각의 작업이 연관이 없다면, 동시에 실행하는 것이 논 블로킹
   - I/O 작업일 때(파일 업로드, 다운로드, 압축, 암호화 작업) ⇒ 빠름
   - 나머지 방식은 블로킹(하나의 작업이 끝나고 다음 작업)

3. **싱글 스레드**

   1. 블로킹
      - 블로킹이 발생하는 경우 나머지 작업은 모두 대기 ⇒ 비효율 발생
   2. 논블로킹
      - 요청을 먼저 받고, 완료될 때 응답함

   

   **멀티스레드와 비교**

   - 새로운 요청올 때마다 새로운 스레드 생성

   - 스레드 자체도 비용이기 때문에 비용이 많이 발생하고 스레드 수만큼 자원을 많이 사용





## 자바스크립트 문법

## function

- `function` ⇒ `return` 키워드 필수, hoisting 발생
- `const` 는 hoisting 발생하지 않으므로 반드시 변수 선언 이후 호출 할 것

``` javascript
function add1(x,y){
    return x+y;
}
const add2 = (x,y) => {
    return x+y;
};
const add3=(x,y) => x+y;
const add4 = (x,y) => (x+y);

// not1 과 not2 도 같은 기능
function not1(x) {
    return !x;
}
const not2 = x => !x;
```



- `function`은 객체임
  - **`function`안에서 `this`는 `function`자기 자신!**

``` javascript
var relationship1 = {
    name: 'zero',
    friends: ['nero', 'hero', 'xero'],
    logFriends: function() {
        var that = this; // relationship1
        this.friends.forEach(function(friend) {
            console.log(that.name, friend);
        });
    },
};

const relationship2 = {
    name: 'zero',
    friends: ['nero', 'hero', 'xero'],
    logFriends() {
        this.friends.forEach(friend => {
            console.log(this.name, friend);
        });
    },
};
```



- 화살표함수는 객체이긴하나  this를 가지지않는 객체로 생각하면 됨
- 편하게 생각하면 람다식으로 생각
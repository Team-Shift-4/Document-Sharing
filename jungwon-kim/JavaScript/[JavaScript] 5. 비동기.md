처음배울 때 나를 괴롭히ㄴ 비동기...



### 동기?

- 함수를 호출하면 그 함수가 끝날 때까지 다음 코드로 X

## 비동기?

- 함수를 호출하면 일단 OK하고 실행 결과 나오면 나중에 알려줌

  ⇒ 다했을 때 알려줄 콜백 함수를 파라미터로 미리 받아둠

  ⇒ **콜백지옥🤯**
  
  ⇒ 콜백의 사용을 줄이는 방법 등장
  
  

## 👉 Promise

### Pending

- 객체가 생성되었지만 동작은 완료되지 않은 상태
- 파라미터 `resolve`, `reject` 함수

### Fulfilled

- 작업 완료
- **resolve() ⇒ then()**

### Reject

- 실패
- **reject() ⇒ catch()**

``` javascript
function test(value) {
    return new Promise((resolve, reject) => {
        if(value>=0) 
            resolve('ok');
        else 
            reject('err');
    });
}

function test2(value) {
    return new Promise((resolve, reject) => {
        console.log('test2 '+value);
        resolve('ok2');
    });
}

function test3(value) {
    console.log(value);
}

// Promise 체인
test(1)
.then(test2)
.then(test3)
.catch((error)=>{ console.log(error); })
```

- 일반 함수와 호출이 너무 다르고
- return new Promise로 계속 감싸야하는 불편함이 있음 



## 👉 Async, await

- ECMAScript 2017(ES8)
  - node는 ES6버전이지만 Async, await 는 지원함!

- `await` 
  - 끝날 때까지 밑 줄로 안 내려감 (동기함수처럼 잡아놓는 것)
  - `async` 함수안에서만 사용 가능
- 예외 처리는 `try-catch`

``` javascript
function test(value) {
    return new Promise((resolve, reject) => {
        if(value>=0) 
            resolve('ok');
        else 
            reject('err');
    });
}

async function test4(){
    try {
        const result = await test(-1);
        console.log(result);
    } catch(error) {
        console.log(error);
    }
}

test4();

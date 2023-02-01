[toc]



# 📌Axois?

- 뷰에서 권고하는 HTTP 통신 라이브러리(Promise 기반)
  - Vue.js에 종속되는 것은 아니고, 다른 js에서도 많이 사용됨
- 상대적으로 다른 HTTP 통신 라이브러리들에 비해 문서화가 잘되어 있고 API가 다양함

# 📌설치

- NPM 방식

 ```
 npm install axios
 ```

# 📌 사용방법

``` html
<div id="app">
  <button v-on:click="fetchData">get data</button>
</div>
```

``` javascript
new Vue({
  el: '#app',
  methods: {
    fetchData: function() {
      axios.get('/todos/save')
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
})
```

## .then

- 비동기 통신 성공했을 때
- 콜백을 인자로 받아 결과값 처리

## .catch

- 오류 처리

# 📌요청 메소드

method의 기본값은 `get`

## axios.get(url[, config])

- 서버에서 데이터를 가져올 때 사용
- `config` : 헤더, 응답초과시간, 인자 값 등 요청 값

## axios.post(url[, data[, config]])

- 서버에 데이터를 새로 생성할 때 사용
- `data` : 생성할 데이터 전송

## axios.patch(url[, data[, config]])

- 서버의 특정 데이터를 수정

## axios.put(url[, data[, config]])

- 특정 데이터 수정을 요청할 때 사용
- 새로운 리소스 생성하거나 이미 존재하는 데이터 대체
- 한 번 요청을 하거나 여러 번 지속적으로 요청해도 결괏값이 동일



### patch와 put의 차이?

- patch : 부분 교체 
  - 보내지 않은 값은 변경이 없고, 보낸 값만 변경
- put : 전체 교체
  - 보내지 않은 값은 null로 변경



## axios.delete(url[, config])

- 특정 데이터 삭제 요청할 때 사용



##  📌예제

``` javascript
// 아이템 하나 삭제
const removeOneItem = (state, payload) => {
    /* 서버 통신 */
    axios
        .put('/todos/delete/' + payload.todoItem.id)
        .then(res => {
            if(res.data == "ok"){
                // ...
            } else {
                alert("삭제 실패");
            }
        });
}
```



[공식문서](https://github.com/axios/axios)

[참고 블로그](https://joshua1988.github.io/vue-camp/vue/axios.html#%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%89%E1%85%B5%E1%84%8B%E1%85%A9%E1%84%89%E1%85%B3-http-%E1%84%8B%E1%85%AD%E1%84%8E%E1%85%A5%E1%86%BC-config-%E1%84%8B%E1%85%A9%E1%86%B8%E1%84%89%E1%85%A7%E1%86%AB)
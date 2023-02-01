[toc]

# 📌 Vuex

프로젝트가 커질수록 props를 이용해 보내는 방식은 코드가 복잡해지고 유지 보수가 어려워 Vuex를 사용한다.

Vuex는 React의 Flux 패턴에서 기인했다. [자세히 기록된 블로그](https://ict-nroo.tistory.com/106)

- Vue.js 애플리케이션을 위한 **상태 관리 패턴 + 라이브러리**

- 모든 컴포넌트가 공유할 수 있는 싱글톤 방식의 데이터 저장소


```
npm install vuex
```



## State

```javascript
// store.js

import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export const store = new Vuex.Store({
  // counter라는 state 속성을 추가
  state: {
    counter: 0
  }
});
```

- state : 컴포넌트 간에 공유할 data 속성 
- 뷰에서 ⇒ `$store.state.counter` 로 사용



``` javascript
// main.js

import Vue from "vue";
import App from "./App.vue";
// store.js를 불러오는 코드
import { store } from "./store";

new Vue({
  el: "#app",
  // 뷰 인스턴스의 store 속성에 연결
  store: store,
  render: h => h(App)
});
```

- main.js 에서 store.js를 등록해서 사용



## Getters 

- state 값에 쉽게 접근

- Getters 등록

``` javascript
// store.js

export const store = new Vuex.Store({
  // ...
  getters: {
    getCounter: function (state) {
      return state.counter;
    }
  }
});
```

- 뷰에서  ⇒ `this.$store.getters.getCounter` 라고 사용

  -  Template 의 표현식은 최대한 간소화해야하기때문에 computed에 등록시켜놓고 사용

  

## Mutations 

- state 값을 변경

- Mutations 등록

``` javascript
// store.js

export const store = new Vuex.Store({
  // ...
  mutations: {
    addCounter: function (state, payload) {
      return state.counter++;
    }
  }
});
```

- 뷰에서  ⇒ `this.$store.commit('addCounter', 10);` method 내용에 추가해놓고 사용
- mutations을 이용하면 state 값이 변경됨



## actions 

- 비동기 함수를 호출하여 값 변경

- actions에 비동기 함수 작성

``` javascript
// store.js

export const store = new Vuex.Store({
  // ...
  actions: {
    TIME({commit}, value) {
      return setTimeout(() => {
        commit('SET_TEST', value);
      }, 1000);
    }
  },
});
```

- 뷰에서 mutations와 똑같이 사용



[공식문서](https://vuex.vuejs.org/#what-is-a-state-management-pattern)

[참고 블로그](https://joshua1988.github.io/web-development/vuejs/vuex-getters-mutations/)
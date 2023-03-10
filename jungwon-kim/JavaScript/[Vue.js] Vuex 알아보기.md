[toc]

# ๐ Vuex

ํ๋ก์ ํธ๊ฐ ์ปค์ง์๋ก props๋ฅผ ์ด์ฉํด ๋ณด๋ด๋ ๋ฐฉ์์ ์ฝ๋๊ฐ ๋ณต์กํด์ง๊ณ  ์ ์ง ๋ณด์๊ฐ ์ด๋ ค์ Vuex๋ฅผ ์ฌ์ฉํ๋ค.

Vuex๋ React์ Flux ํจํด์์ ๊ธฐ์ธํ๋ค. [์์ธํ ๊ธฐ๋ก๋ ๋ธ๋ก๊ทธ](https://ict-nroo.tistory.com/106)

- Vue.js ์ ํ๋ฆฌ์ผ์ด์์ ์ํ **์ํ ๊ด๋ฆฌ ํจํด + ๋ผ์ด๋ธ๋ฌ๋ฆฌ**

- ๋ชจ๋  ์ปดํฌ๋ํธ๊ฐ ๊ณต์ ํ  ์ ์๋ ์ฑ๊ธํค ๋ฐฉ์์ ๋ฐ์ดํฐ ์ ์ฅ์


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
  // counter๋ผ๋ state ์์ฑ์ ์ถ๊ฐ
  state: {
    counter: 0
  }
});
```

- state : ์ปดํฌ๋ํธ ๊ฐ์ ๊ณต์ ํ  data ์์ฑ 
- ๋ทฐ์์ โ `$store.state.counter` ๋ก ์ฌ์ฉ



``` javascript
// main.js

import Vue from "vue";
import App from "./App.vue";
// store.js๋ฅผ ๋ถ๋ฌ์ค๋ ์ฝ๋
import { store } from "./store";

new Vue({
  el: "#app",
  // ๋ทฐ ์ธ์คํด์ค์ store ์์ฑ์ ์ฐ๊ฒฐ
  store: store,
  render: h => h(App)
});
```

- main.js ์์ store.js๋ฅผ ๋ฑ๋กํด์ ์ฌ์ฉ



## Getters 

- state ๊ฐ์ ์ฝ๊ฒ ์ ๊ทผ

- Getters ๋ฑ๋ก

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

- ๋ทฐ์์  โ `this.$store.getters.getCounter` ๋ผ๊ณ  ์ฌ์ฉ

  -  Template ์ ํํ์์ ์ต๋ํ ๊ฐ์ํํด์ผํ๊ธฐ๋๋ฌธ์ computed์ ๋ฑ๋ก์์ผ๋๊ณ  ์ฌ์ฉ

  

## Mutations 

- state ๊ฐ์ ๋ณ๊ฒฝ

- Mutations ๋ฑ๋ก

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

- ๋ทฐ์์  โ `this.$store.commit('addCounter', 10);` method ๋ด์ฉ์ ์ถ๊ฐํด๋๊ณ  ์ฌ์ฉ
- mutations์ ์ด์ฉํ๋ฉด state ๊ฐ์ด ๋ณ๊ฒฝ๋จ



## actions 

- ๋น๋๊ธฐ ํจ์๋ฅผ ํธ์ถํ์ฌ ๊ฐ ๋ณ๊ฒฝ

- actions์ ๋น๋๊ธฐ ํจ์ ์์ฑ

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

- ๋ทฐ์์ mutations์ ๋๊ฐ์ด ์ฌ์ฉ



[๊ณต์๋ฌธ์](https://vuex.vuejs.org/#what-is-a-state-management-pattern)

[์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://joshua1988.github.io/web-development/vuejs/vuex-getters-mutations/)
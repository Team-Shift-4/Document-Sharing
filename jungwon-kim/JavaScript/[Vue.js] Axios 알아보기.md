[toc]



# ๐Axois?

- ๋ทฐ์์ ๊ถ๊ณ ํ๋ HTTP ํต์  ๋ผ์ด๋ธ๋ฌ๋ฆฌ(Promise ๊ธฐ๋ฐ)
  - Vue.js์ ์ข์๋๋ ๊ฒ์ ์๋๊ณ , ๋ค๋ฅธ js์์๋ ๋ง์ด ์ฌ์ฉ๋จ
- ์๋์ ์ผ๋ก ๋ค๋ฅธ HTTP ํต์  ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ค์ ๋นํด ๋ฌธ์ํ๊ฐ ์๋์ด ์๊ณ  API๊ฐ ๋ค์ํจ

# ๐์ค์น

- NPM ๋ฐฉ์

 ```
 npm install axios
 ```

# ๐ ์ฌ์ฉ๋ฐฉ๋ฒ

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

- ๋น๋๊ธฐ ํต์  ์ฑ๊ณตํ์ ๋
- ์ฝ๋ฐฑ์ ์ธ์๋ก ๋ฐ์ ๊ฒฐ๊ณผ๊ฐ ์ฒ๋ฆฌ

## .catch

- ์ค๋ฅ ์ฒ๋ฆฌ

# ๐์์ฒญ ๋ฉ์๋

method์ ๊ธฐ๋ณธ๊ฐ์ `get`

## axios.get(url[, config])

- ์๋ฒ์์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ฌ ๋ ์ฌ์ฉ
- `config` : ํค๋, ์๋ต์ด๊ณผ์๊ฐ, ์ธ์ ๊ฐ ๋ฑ ์์ฒญ ๊ฐ

## axios.post(url[, data[, config]])

- ์๋ฒ์ ๋ฐ์ดํฐ๋ฅผ ์๋ก ์์ฑํ  ๋ ์ฌ์ฉ
- `data` : ์์ฑํ  ๋ฐ์ดํฐ ์ ์ก

## axios.patch(url[, data[, config]])

- ์๋ฒ์ ํน์  ๋ฐ์ดํฐ๋ฅผ ์์ 

## axios.put(url[, data[, config]])

- ํน์  ๋ฐ์ดํฐ ์์ ์ ์์ฒญํ  ๋ ์ฌ์ฉ
- ์๋ก์ด ๋ฆฌ์์ค ์์ฑํ๊ฑฐ๋ ์ด๋ฏธ ์กด์ฌํ๋ ๋ฐ์ดํฐ ๋์ฒด
- ํ ๋ฒ ์์ฒญ์ ํ๊ฑฐ๋ ์ฌ๋ฌ ๋ฒ ์ง์์ ์ผ๋ก ์์ฒญํด๋ ๊ฒฐ๊ด๊ฐ์ด ๋์ผ



### patch์ put์ ์ฐจ์ด?

- patch : ๋ถ๋ถ ๊ต์ฒด 
  - ๋ณด๋ด์ง ์์ ๊ฐ์ ๋ณ๊ฒฝ์ด ์๊ณ , ๋ณด๋ธ ๊ฐ๋ง ๋ณ๊ฒฝ
- put : ์ ์ฒด ๊ต์ฒด
  - ๋ณด๋ด์ง ์์ ๊ฐ์ null๋ก ๋ณ๊ฒฝ



## axios.delete(url[, config])

- ํน์  ๋ฐ์ดํฐ ์ญ์  ์์ฒญํ  ๋ ์ฌ์ฉ



##  ๐์์ 

``` javascript
// ์์ดํ ํ๋ ์ญ์ 
const removeOneItem = (state, payload) => {
    /* ์๋ฒ ํต์  */
    axios
        .put('/todos/delete/' + payload.todoItem.id)
        .then(res => {
            if(res.data == "ok"){
                // ...
            } else {
                alert("์ญ์  ์คํจ");
            }
        });
}
```



[๊ณต์๋ฌธ์](https://github.com/axios/axios)

[์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://joshua1988.github.io/vue-camp/vue/axios.html#%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%89%E1%85%B5%E1%84%8B%E1%85%A9%E1%84%89%E1%85%B3-http-%E1%84%8B%E1%85%AD%E1%84%8E%E1%85%A5%E1%86%BC-config-%E1%84%8B%E1%85%A9%E1%86%B8%E1%84%89%E1%85%A7%E1%86%AB)
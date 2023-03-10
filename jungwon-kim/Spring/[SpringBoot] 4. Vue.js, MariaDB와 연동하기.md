[toc]

# ๐ Vue ํ๋ก์ ํธ ์์ฑ

```
npm install -g @vue/cli
vue create frontend
```



## vue.config.js

- SpringBoot ํ๋ก์ ํธ(ํ๋ก์ ํธ๋ช: backend)๊ฐ ์์ฑ๋์ด ์์ด์ผ ํจ!
- ์๋์ ๊ฐ์ด ์์ ํ๊ณ  ๋น๋ `npm run build`

```javascript
const path = require("path");

module.exports = {
  devServer: {
    proxy : 'http://localhost:9959'
  },
  indexPath: '../../templates/vue/index.html',
  publicPath: '/vue',
  outputDir: path.resolve(__dirname, "../backend/src/main/resources/static/vue")
}
```

### devServer/proxy

- Vue ๊ฐ๋ฐ์ฉ ์๋ฒ๊ฐ ์ฒ๋ฆฌํ์ง ๋ชปํ๋ ์์ฒญ์ 8787 ํฌํธ๋ก ์์ฒญ

### outputDir

- vue ํ๋ก์ ํธ ๋น๋ ๊ฒฝ๋ก(css,js ํ์ผ)

### publicPath

- ๋น๋ ์ js,css ์ ๊ฐ์ ์ธ๋ถ ์ฐธ์กฐ ํ์ผ์ ๊ฒฝ๋ก๋ฅผ ๋ณ๊ฒฝํ  ๋ ์ฌ์ฉ

### indexPath

- vue ํ๋ก์ ํธ๋ก ๋น๋๋ ํ์ผ๋ค์ html-webpack-plugin์ ์ํด ๋ง๋ค์ด์ง๋ index.html์ ์๋์ผ๋ก ํ์ผ์ด ํฌํจ



# ๐ SpringBoot ํ๋ก์ ํธ ํ๊ฒฝ ์ค์ 

## build.gradle

``` 
dependencies {
	// springboot ๊ธฐ๋ณธ ์ค์ 
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-devtools'
	implementation 'org.springframework.boot:spring-boot-starter-validation'

	// jpa ๋ก๊ทธ
	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.8.1'

	//mybatis
	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.1'    
	implementation 'junit:junit:4.13.2'
	runtimeOnly 'com.h2database:h2'

	// mariadb ์ฐ๋
	runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'

	// lombok ์ฌ์ฉ
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```



## application.properties

```
server.port = 9959

spring.datasource.driverClassName=org.mariadb.jdbc.Driver
spring.datasource.url = jdbc:mariadb://localhost:3306/mydays
spring.datasource.username = root
spring.datasource.password = root

spring.h2.console.enabled= true

spring.jpa.hibernate.ddl-auto = create
spring.jpa.properties.hibernate.format_sql = true
spring.jpa.properties.hibernate.default_batch_fetch_size= 100
spring.jpa.properties.generate-ddl = true

logging.level.org.hibernate.SQL = debug
logging.level.org.hibernate.type = trace
```

### server.port

- ํ๋ก์ ํธ ํฌํธ๋ฒํธ ์ค์ 
- `vue.config.js` ํ์ผ์ ์ ์๋ ํฌํธ๋ฒํธ์ ๋์ผ

### spring/jpa/hibernate,properties,generate-ddl

- jpa์ ๋ํ ๊ธฐ๋ณธ ์ค์ 



# ๐ ์ฐ๋ ํ์ธ

- src/main/java/com.example.backend(ํ๋ก์ ํธ๋ช)/controller/WebController.java

  ``` java
  @Controller
  public class WebController {
  
      @GetMapping("/vue")
      public String vue(){
  
          return "vue/index";
      }
  }
  ```

- ์์ฑ ํ http://localhost:9959/vue ๋ค์ด๊ฐ์ ํ๋ฉด๋จ๋์ง ํ์ธํด๋ณด์

์ฐ๋์ด ๋์์ผ๋ฉด ๋ค์ ๊ฒ์๊ธ์์ ๊ฐ๋จํ ํ์๊ฐ์์ ๊ตฌํํด๋ณผ ์์ !

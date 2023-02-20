[toc]

# 📌 Vue 프로젝트 생성

```
npm install -g @vue/cli
vue create frontend
```



## vue.config.js

- SpringBoot 프로젝트(프로젝트명: backend)가 생성되어 있어야 함!
- 아래와 같이 수정하고 빌드 `npm run build`

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

- Vue 개발용 서버가 처리하지 못하는 요청을 8787 포트로 요청

### outputDir

- vue 프로젝트 빌드 경로(css,js 파일)

### publicPath

- 빌드 시 js,css 와 같은 외부 참조 파일의 경로를 변경할 때 사용

### indexPath

- vue 프로젝트로 빌드된 파일들은 html-webpack-plugin에 의해 만들어지는 index.html에 자동으로 파일이 포함



# 📌 SpringBoot 프로젝트 환경 설정

## build.gradle

``` 
dependencies {
	// springboot 기본 설정
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-devtools'
	implementation 'org.springframework.boot:spring-boot-starter-validation'

	// jpa 로그
	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.8.1'

	//mybatis
	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.1'    
	implementation 'junit:junit:4.13.2'
	runtimeOnly 'com.h2database:h2'

	// mariadb 연동
	runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'

	// lombok 사용
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

- 프로젝트 포트번호 설정
- `vue.config.js` 파일에 적었던 포트번호와 동일

### spring/jpa/hibernate,properties,generate-ddl

- jpa에 대한 기본 설정



# 📌 연동 확인

- src/main/java/com.example.backend(프로젝트명)/controller/WebController.java

  ``` java
  @Controller
  public class WebController {
  
      @GetMapping("/vue")
      public String vue(){
  
          return "vue/index";
      }
  }
  ```

- 작성 후 http://localhost:9959/vue 들어가서 화면뜨는지 확인해보자

연동이 되었으면 다음 게시글에서 간단한 회원가입을 구현해볼 예정!

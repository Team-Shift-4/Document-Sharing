https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F#CORS%EB%A5%BC_%ED%95%B4%EA%B2%B0%ED%95%98%EB%8A%94_%EB%B0%A9%EB%B2%95_%EC%B4%9D%EC%A0%95%EB%A6%AC_%F0%9F%99%8C

``` java
// 스프링 서버 전역적으로 CORS 설정
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
        	.allowedOrigins("http://localhost:8080", "http://localhost:8081") // 허용할 출처
            .allowedMethods("GET", "POST") // 허용할 HTTP method
            .allowCredentials(true) // 쿠키 인증 요청 허용
            .maxAge(3000) // 원하는 시간만큼 pre-flight 리퀘스트를 캐싱
    }
}
```



## Vue 와 Spring boot 연동 후 CORS 오류

1. Spring boot에서 처리

``` java
@RestController
@RequiredArgsConstructor
public class UserController {
    private final UserRepository userRepository;

    @PostMapping("/api/user/login")
    @CrossOrigin(origins = "http://localhost:9960/")  // 해당 출처 허용
    public int login(@RequestBody Map<String, String> params) {
        ...
    }
}
```

- `@CrossOrigin(origins = "허용하고자 하는 출처_URL")`

  : 어노테이션을 사용해서 처리하는 방법으로 origins에 명시한 출처를 허용한다는 뜻이다.



2. `vue.config.js` 파일 변경

``` javascript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:9959',
        changeOrigin: true, // CORS 막기
      }
    }
  },
  lintOnSave: false
}
```


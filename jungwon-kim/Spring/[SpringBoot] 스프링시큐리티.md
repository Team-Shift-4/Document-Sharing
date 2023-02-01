[toc]

#  📌Spring Security

- 스프링 기반의 애플리케이션의 보안을 담당하는 스프링 하위 프레임워크
- **인증**(Authenticate, 누구인지?) 과 **인가**(Authorize, 어떤것을 할 수 있는지?)를 담당하는 프레임워크

```xml
implementation 'org.springframework.boot:spring-boot-starter-security'

implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6:3.1.1.RELEASE' 
```



[참고 블로그](https://azurealstn.tistory.com/91)



공식문서를 보면 Spring Security Servlet과 webflux 두가지가 있다.
https://velog.io/@neity16/WebFlux-2-WebFlux-%EB%9E%80-SpringMVC-vs-WebFlux



``` java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests().requestMatchers(
                new AntPathRequestMatcher("/**")).permitAll()
                ;
        return http.build();
    }
}
```

- @Configuration은 스프링의 환경설정 파일임을 의미하는 애너테이션이다. 여기서는 스프링 시큐리티의 설정을 위해 사용
- @EnableWebSecurity는 모든 요청 URL이 스프링 시큐리티의 제어를 받도록 만드는 애너테이션이다.



``` java
http.authorizeHttpRequests().requestMatchers(
    new AntPathRequestMatcher("/**")).permitAll(); 
```

- 모든 인증되지 않은 요청을 허락



# CSRF?

> CSRF(cross site request forgery)는 웹 사이트 취약점 공격을 방지를 위해 사용하는 기술이다. 
>
> 스프링 시큐리티가 CSRF 토큰 값을 세션을 통해 발행하고 웹 페이지에서는 폼 전송시에 해당 토큰을 함께 전송하여 실제 웹 페이지에서 작성된 데이터가 전달되는지를 검증하는 기술이다.
>
> [출처](https://wikidocs.net/162150)

``` java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
	@Bean
	 SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests().requestMatchers(
                new AntPathRequestMatcher("/**")).permitAll()
        .and()
        	.csrf().ignoringRequestMatchers(new AntPathRequestMatcher("/h2-console/**"))
        .and()
        	.headers().addHeaderWriter(new XFrameOptionsHeaderWriter(
        		XFrameOptionsHeaderWriter.XFrameOptionsMode.SAMEORIGIN))
                ;
        return http.build();
    }	
}
```

- `and()` - http 객체의 설정을 이어서 할 수 있게 하는 메서드이다.
- `csrf().ignoringRequestMatchers(new AntPathRequestMatcher("/h2-console/**"))` - `/h2-console/`로 시작하는 URL은 CSRF 검증을 하지 않는다는 설정이다.
- 위 처럼 URL 요청시 `X-Frame-Options` 헤더값을 `sameorigin`으로 설정하여 오류가 발생하지 않도록 했다. `X-Frame-Options` 헤더의 값으로 sameorigin을 설정하면 frame에 포함된 페이지가 페이지를 제공하는 사이트와 동일한 경우에는 계속 사용할 수 있다.

[toc]

#  ๐Spring Security

- ์คํ๋ง ๊ธฐ๋ฐ์ ์ ํ๋ฆฌ์ผ์ด์์ ๋ณด์์ ๋ด๋นํ๋ ์คํ๋ง ํ์ ํ๋ ์์ํฌ
- **์ธ์ฆ**(Authenticate, ๋๊ตฌ์ธ์ง?) ๊ณผ **์ธ๊ฐ**(Authorize, ์ด๋ค๊ฒ์ ํ  ์ ์๋์ง?)๋ฅผ ๋ด๋นํ๋ ํ๋ ์์ํฌ

```xml
implementation 'org.springframework.boot:spring-boot-starter-security'

implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6:3.1.1.RELEASE' 
```



[์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://azurealstn.tistory.com/91)



- SpringBoot 2.7+ ๋ฒ์ ์์ Spring Security์ WebSecurityConfigurerAdapter๋ฅผ ํตํด security config๋ฅผ override ํ  ๋ ์ค๋ฅ๊ฐ ๋ฐ์ํจ

``` java
@Configuration
@RequiredArgsConstructor
@EnableWebSecurity // Spring Security ์ค์  ํ์ฑํ
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final CustomOAuth2UserService customOAuth2UserService;
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http
                .csrf().disable()
                .headers().frameOptions().disable()
                .and()
                .authorizeRequests()
                .antMatchers("/", "/css/**", "/images/**",
                        "/js/**", "/h2-console/**").permitAll()
                .antMatchers("/api/v1/**").hasRole(Role.
                        USER.name())
                .anyRequest().authenticated()
                .and()
                .logout()
                .logoutSuccessUrl("/")
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .userService(customOAuth2UserService);
    }

}
```

์๋์ ๊ฐ์ด ๋ณ๊ฒฝ [์ฐธ๊ณ  ๋ธ๋ก๊ทธ](https://honeywater97.tistory.com/264)

``` java
@EnableWebSecurity
@RequiredArgsConstructor
@Configuration(proxyBeanMethods = false)
@ConditionalOnDefaultWebSecurity
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
public class SecurityConfig {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Bean
    @Order(SecurityProperties.BASIC_AUTH_ORDER)
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .headers().frameOptions().disable()
                .and()
                .authorizeRequests()
                .antMatchers("/", "/css/**", "/images/**",
                        "/js/**", "/h2-console/**").permitAll()
                .antMatchers("/api/v1/**").hasRole(Role.
                        USER.name())
                .anyRequest().authenticated()
                .and()
                .logout()
                .logoutSuccessUrl("/")
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .userService(customOAuth2UserService);

        return http.build();
    }
}
```





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

- @Configuration์ ์คํ๋ง์ ํ๊ฒฝ์ค์  ํ์ผ์์ ์๋ฏธํ๋ ์ ๋ํ์ด์์ด๋ค. ์ฌ๊ธฐ์๋ ์คํ๋ง ์ํ๋ฆฌํฐ์ ์ค์ ์ ์ํด ์ฌ์ฉ
- @EnableWebSecurity๋ ๋ชจ๋  ์์ฒญ URL์ด ์คํ๋ง ์ํ๋ฆฌํฐ์ ์ ์ด๋ฅผ ๋ฐ๋๋ก ๋ง๋๋ ์ ๋ํ์ด์์ด๋ค.



``` java
http.authorizeHttpRequests().requestMatchers(
    new AntPathRequestMatcher("/**")).permitAll(); 
```

- ๋ชจ๋  ์ธ์ฆ๋์ง ์์ ์์ฒญ์ ํ๋ฝ



# CSRF?

> CSRF(cross site request forgery)๋ ์น ์ฌ์ดํธ ์ทจ์ฝ์  ๊ณต๊ฒฉ์ ๋ฐฉ์ง๋ฅผ ์ํด ์ฌ์ฉํ๋ ๊ธฐ์ ์ด๋ค. 
>
> ์คํ๋ง ์ํ๋ฆฌํฐ๊ฐ CSRF ํ ํฐ ๊ฐ์ ์ธ์์ ํตํด ๋ฐํํ๊ณ  ์น ํ์ด์ง์์๋ ํผ ์ ์ก์์ ํด๋น ํ ํฐ์ ํจ๊ป ์ ์กํ์ฌ ์ค์  ์น ํ์ด์ง์์ ์์ฑ๋ ๋ฐ์ดํฐ๊ฐ ์ ๋ฌ๋๋์ง๋ฅผ ๊ฒ์ฆํ๋ ๊ธฐ์ ์ด๋ค.
>
> [์ถ์ฒ](https://wikidocs.net/162150)

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

- `and()` - http ๊ฐ์ฒด์ ์ค์ ์ ์ด์ด์ ํ  ์ ์๊ฒ ํ๋ ๋ฉ์๋์ด๋ค.
- `csrf().ignoringRequestMatchers(new AntPathRequestMatcher("/h2-console/**"))` - `/h2-console/`๋ก ์์ํ๋ URL์ CSRF ๊ฒ์ฆ์ ํ์ง ์๋๋ค๋ ์ค์ ์ด๋ค.
- ์ ์ฒ๋ผ URL ์์ฒญ์ `X-Frame-Options` ํค๋๊ฐ์ `sameorigin`์ผ๋ก ์ค์ ํ์ฌ ์ค๋ฅ๊ฐ ๋ฐ์ํ์ง ์๋๋ก ํ๋ค. `X-Frame-Options` ํค๋์ ๊ฐ์ผ๋ก sameorigin์ ์ค์ ํ๋ฉด frame์ ํฌํจ๋ ํ์ด์ง๊ฐ ํ์ด์ง๋ฅผ ์ ๊ณตํ๋ ์ฌ์ดํธ์ ๋์ผํ ๊ฒฝ์ฐ์๋ ๊ณ์ ์ฌ์ฉํ  ์ ์๋ค.





๋ด๊ฐ ์ฌ์ฉ์ค์ธ ๊ฒ

``` java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .httpBasic()
                .and()
                .authorizeRequests()
                .antMatchers("/api/user/**").permitAll()
                .and()
                .csrf().disable()
        ;

        return http.build();
    }
}
```


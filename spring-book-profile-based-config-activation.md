# Spring Boot: Security Configuration by Active Profile

Spring Boot supports environment-based (profile-based) bean loading.  
You can leverage this to load different `SecurityConfig` classes per environment (`dev`, `stage`, `prod`).

---

## ✅ Profiles in Spring

You can activate a profile using:

**application.yml**
```yaml
spring:
  profiles:
    active: dev
```

***Class Level: Create separate classes and annotate them with @Profile.**

```java

@Configuration
@Profile("dev")
@EnableWebSecurity
public class DevSecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
                .anyRequest().permitAll();
        return http.build();
    }
}


```

***Multiple Security Chains in One Class (Alternative) ie, Method Level***

```java
@Bean
@Profile("dev")
SecurityFilterChain devChain(HttpSecurity http) throws Exception { ... }

@Bean
@Profile("prod")
SecurityFilterChain prodChain(HttpSecurity http) throws Exception { ... }

```

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

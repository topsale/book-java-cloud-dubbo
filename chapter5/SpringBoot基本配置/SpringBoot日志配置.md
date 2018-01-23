# Spring Boot 日志配置

---

Spring Boot 对各种日志框架都做了支持，我们可以通过配置来修改默认的日志的配置

默认情况下，Spring Boot 使用 Logback 作为日志框架

## 配置日志文件

```
logging:
  file: ../logs/spring-boot-hello.log
  level.org.springframework.web: DEBUG
```
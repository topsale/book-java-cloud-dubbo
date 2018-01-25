# Spring Boot 与 Thymeleaf

---

如果希望以 Jar 形式发布模块则尽量不要使用 JSP 相关知识，这是因为 JSP 在内嵌的 Servlet 容器上运行有一些问题 (内嵌 Tomcat、 Jetty 不支持 Jar 形式运行 JSP，Undertow 不支持 JSP)。

Spring Boot 中推荐使用 Thymeleaf 作为模板引擎，因为 Thymeleaf 提供了完美的 Spring MVC 支持

Spring Boot 提供了大量模板引擎，包括：

* FreeMarker
* Groovy
* Mustache
* Thymeleaf
* Velocity
* Beetl
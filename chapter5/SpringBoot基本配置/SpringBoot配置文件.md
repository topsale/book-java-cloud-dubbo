# Spring Boot 配置文件

---

Spring Boot项目使用一个全局的配置文件 `application.properties` 或者是 `application.yml`，在 `resources` 目录下或者类路径下的 `/config` 下，一般我们放到 `resources` 下。

修改 tomcat 的端口为 9090，并将默认的访问路径 "/" 修改为 "boot"，可以在 `application.properties` 中添加：

```
server.port=9090
server.context-path=/boot
```

或在 application.yml 中添加：

```
server:
  port: 9090
  context-path: /boot
```

测试效果：

![](/assets/Lusifer1509896204.png)

修改 DispatcherServlet 的规则为：\*.html

```
server:
  port: 9090
  context-path: /boot
  servlet-path: '*.html'
```

测试效果：

![](/assets/Lusifer1509896991.png)

更多配置：https://docs.spring.io/spring-boot/docs/1.5.8.RELEASE/reference/html/common-application-properties.html
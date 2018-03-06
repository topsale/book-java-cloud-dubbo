# RESTful 风格

---

> 一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

## 风格路径配置

在 `application.yml` 配置文件中增加 RESTful 风格路径

```
# RESTful 风格路径
rest:
  path:
    api:
      v1: api/v1
```

## 创建与风格路径匹配的包和控制器

### package

```
com.ooqiu.gaming.server.api.controller.v1
```

### Controller

```
@RestController
@RequestMapping(value = "${rest.path.api.v1}/channel")
public class ChannelControllerV1 {}
```
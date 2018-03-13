# Spring Boot 启用 Profile

---

在实际开发中，生产环境与测试环境的配置不同，我们需要在上线时手动修改相关的配置信息。但 Spring 为我们提供了 Profile 功能，我们只需要在项目启动时添加一个虚拟机参数，激活对应的 Profile 环境即可

## 创建不同环境的配置文件

![](/assets/Lusifer1520953856.png)

### 开发环境配置

application-dev.yml

![](/assets/Lusifer1520954038.png)

### 生产环境配置

application-prod.yml

![](/assets/Lusifer1520954076.png)

## 启动命令

增加 JVM 参数 `--spring.profiles.active=dev`

```
java -jar gaming-server-api-1.0.0-SNAPSHOT.jar --spring.profiles.active=dev
```

## 启动效果图

![](/assets/Lusifer1520954249.png)
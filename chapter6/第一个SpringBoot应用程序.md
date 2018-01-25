# 第一个 Spring Boot 应用程序

---

这里我们使用 Intellij IDEA 来新建一个 Spring Boot 项目。

## 新建 Spring Initializr 项目

![](/assets/微信截图_20171105211722.png)

## 填写项目信息

![](/assets/Image 7.png)

## 选择项目使用技术

![](/assets/Image 8.png)

## 填写项目名称

![](/assets/Image 9.png)

新建 Spring Boot 项目后，会在根包目录下有一个 artifactId + Application 命名规则的入口类

![](/assets/Image 10.png)

## 添加测试控制器

```
package com.lusifer.spring.boot.hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class HelloApplication {

    @RequestMapping(value = "/")
    public String index() {
        return "Hello Spring Boot.";
    }

    public static void main(String[] args) {
        SpringApplication.run(HelloApplication.class, args);
    }
}
```

## 代码解释

`@SpringBootApplication`：是 Spring Boot 项目的核心注解，主要目的是开启自动配置。

`main()` 方法：是一个标准的 Java 应用的 main 方法，主要作用是作为项目启动的入口。

## 运行效果

HelloApplication 文件中单击右键，在右键菜单中选择 Run "HelloApplication" 运行项目

![](/assets/Lusifer1509890030.png)

访问 http://localhost:8080

![](/assets/Lusifer1509890223.png)
# 第一个 Thymeleaf 模板页

---

新建一个名为 spring-boot-thymeleaf 的 Spring Boot 项目，并引入 Thymeleaf 的 starter pom

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

<dependency>
    <groupId>net.sourceforge.nekohtml</groupId>
    <artifactId>nekohtml</artifactId>
    <version>1.9.22</version>
</dependency>
```

## 示例 JavaBean

此类用来在模板页面展示数据用，包含 name 和 age 属性

```
package com.lusifer.spring.boot.thymeleaf.bean;

public class PersonBean {
    private String name;
    private Integer age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

## 脚本样式静态文件

根据默认原则，脚本样式、图片等静态文件应该放置在 src/main/resources/static 下，这里引入 Bootstrap 和 jQuery

![](/assets/Lusifer1509938815.png)

## 演示页面

根据默认原则，页面应该放置在 `src/main/resources/templates` 下，在该目录下新建 index.html

![](/assets/Lusifer1509944305.png)

```
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta content="text/html;charset=UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width-device-width, initial-scale=1" />
    <link th:src="@{bootstrap/css/bootstrap.min.css}" rel="stylesheet" />
    <link th:src="@{bootstrap/css/bootstrap-theme.min.css}" rel="stylesheet" />
</head>
<body>
<div class="panel panel-primary">
    <div class="panel-heading">
        <h3 class="panel-title">访问 model</h3>
        <div class="panel-body">
            <span th:text="${singlePerson.name}" />
        </div>
    </div>
</div>

<div class="panel panel-primary">
    <div class="panel-heading">
        <h3 class="panel-title">列表</h3>
        <div class="panel-body">
            <ul class="list-group">
                <li class="list-group-item" th:each="person:${people}">
                    <span th:text="${person.name}"></span>
                    <span th:text="${person.age}"></span>
                    <button class="btn" th:onclick="'getName(\'' + ${person.name} + '\');'">获得名字</button>
                </li>
            </ul>
        </div>
    </div>
</div>

<script th:src="@{jquery.min.js}" type="text/javascript"></script>
<script th:src="@{bootstrap/js/bootstrap.min.js}" type="text/javascript"></script>
<script th:inline="javascript">
    var single = [[${singlePerson}]];
    console.log(single.name + "/" + single.age);

    function getName(name) {
        console.log(name)
    }
</script>
</body>
</html>
```

## 数据准备

```
package com.lusifer.spring.boot.thymeleaf;

import com.lusifer.spring.boot.thymeleaf.bean.PersonBean;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.ArrayList;
import java.util.List;

@Controller
@SpringBootApplication
public class ThymeleafApplication {

    @RequestMapping(value = "/")
    public String index(Model model) {
        PersonBean person = new PersonBean();
        person.setName("张三");
        person.setAge(22);

        List<PersonBean> people = new ArrayList<>();
        PersonBean p1 = new PersonBean();
        p1.setName("李四");
        p1.setAge(23);
        people.add(p1);

        PersonBean p2 = new PersonBean();
        p2.setName("王五");
        p2.setAge(24);
        people.add(p2);

        PersonBean p3 = new PersonBean();
        p3.setName("赵六");
        p3.setAge(25);
        people.add(p3);

        model.addAttribute("singlePerson", person);
        model.addAttribute("people", people);

        return "index";
    }

    public static void main(String[] args) {
        SpringApplication.run(ThymeleafApplication.class, args);
    }
}
```

## 在 application.yml 中配置属性解析器

```
# Thymeleaf Start
spring:
  thymeleaf:
    cache: false # 开发时关闭缓存,不然没法看到实时页面
    mode: LEGACYHTML5 # 用非严格的 HTML
    encoding: UTF-8
    content-type: text/html
# Thymeleaf End
```

## 测试运行

![](/assets/Lusifer1509948421.png)
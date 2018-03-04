# PageHelper 分页查询

---

## 定义接口

```
/**
 * 分页查询
 * @param page 页码（第几页）
 * @param pageSize 每页显示的笔数
 * @return
 */
public PageInfo<Article> page(int page, int pageSize);
```

## 实现接口

```
@Override
public PageInfo<Article> page(int page, int pageSize) {
    Example example = new Example(Article.class);
    PageHelper.startPage(page, pageSize);
    PageInfo<Article> pageInfo = new PageInfo<>(articleMapper.selectByExample(example));
    return pageInfo;
}
```

## 控制器代码

```
/**
 * 分页查询
 *
 * @return
 */
@ResponseBody
@RequestMapping(value = "data")
public DataTable<Article> data(HttpServletRequest request) {
    String strPage = request.getParameter("datatable[pagination][page]");
    String strPageSize = request.getParameter("datatable[pagination][perpage]");

    // 遍历客户端 POST 请求的参数
//        Enumeration<String> parameterNames = request.getParameterNames();
//        while (parameterNames.hasMoreElements()) {
//            String name = parameterNames.nextElement();
//            String value = request.getParameter(name);
//            System.out.println("name=" + name + "     value=" + value);
//        }

    int page = 1;
    int pageSize = 10;

    if (!StringUtils.isBlank(strPage)) {
        page = Integer.parseInt(strPage);
    }
    if (!StringUtils.isBlank(strPageSize)) {
        pageSize = Integer.parseInt(strPageSize);
    }

    PageInfo<Article> pageInfo = articleService.page(page, pageSize);
    return new DataTable<Article>(pageInfo);
}
```

## 关闭数据源的自动配置

由于 `gaming-server-service-admin-api`、`gaming-server-service-admin`、`gaming-server-web-admin` 三个项目都需要依赖 PageHelper 插件，所以我们只需要在 `gaming-server-service-admin-api`  项目中添加相关依赖

```
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
</dependency>
```

但这里依赖的是 PageHelper 的 Starter POM ，该 Starter POM 会自动配置数据源，所以我们需要在 `gaming-server-web-admin` 项目中排除数据源的自动配置功能，修改 `GamingServerWebAdminApplication`

```
package com.ooqiu.gaming.server.web.admin;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;

@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class GamingServerWebAdminApplication {
    public static void main(String[] args) {
        SpringApplication.run(GamingServerWebAdminApplication.class, args);
    }
}
```

使用 `@SpringBootApplication` 注解的 `exclude` 参数，即可解决该问题
# Swagger2 生成接口文档

---

## 手写文档存在的问题

* 文档需要更新的时候，需要再次发送一份给前端，也就是文档更新交流不及时。
* 接口返回结果不明确
* 不能直接在线测试接口，通常需要使用工具，比如：Postman
* 接口文档太多，不好管理

## 使用 Swagger 解决问题

Swagger 也就是为了解决这个问题，当然也不能说 Swagger 就一定是完美的，当然也有缺点，最明显的就是代码移入性比较强。

## Maven

```
<!-- Swagger2 Begin -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.8.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.8.0</version>
</dependency>
<!-- Swagger2 End -->
```

## Swagger2 配置类

注意：`RequestHandlerSelectors.basePackage("com.ooqiu.gaming.server.api.controller")` 为 Controller 包路径，不然生成的文档扫描不到接口

```
package com.ooqiu.gaming.server.api.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

/**
 * Swagger2 配置类
 * <p>Title: Swagger2Configuration</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/7 3:35
 */
@Configuration
public class Swagger2Configuration {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.ooqiu.gaming.server.api.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("我的头条 API 文档")
                .description("我的头条 API 网关接口，http://www.funtl.com")
                .termsOfServiceUrl("http://www.funtl.com")
                .version("1.0.0")
                .build();
    }
}
```

## Application

Application.class 加上注解 `@EnableSwagger2` 表示开启 Swagger

```
package com.ooqiu.gaming.server.api;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
@EnableSwagger2
public class GamingServerApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(GamingServerApiApplication.class, args);
    }
}
```

## 控制器代码

### 频道控制器

```
package com.ooqiu.gaming.server.api.controller.v1;

import com.alibaba.dubbo.config.annotation.Reference;
import com.ooqiu.gaming.server.domain.Channel;
import com.ooqiu.gaming.service.channel.api.ChannelService;
import com.ooqui.gaming.server.commons.constant.DubboVersionConstant;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * 频道接口
 * <p>Title: ChannelControllerV1</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/7 3:22
 */
@RestController
@RequestMapping(value = "${rest.path.api.v1}/channel")
public class ChannelControllerV1 {

    @Reference(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_CHANNEL)
    private ChannelService channelService;

    /**
     * 获取全部频道
     *
     * @return
     */
    @ApiOperation(value = "获取频道列表", notes = "获取全部频道列表")
    @RequestMapping(value = "data", method = RequestMethod.GET)
    private List<Channel> data() {
        return channelService.selectAll();
    }
}
```

### 文章控制器

```
package com.ooqiu.gaming.server.api.controller.v1;

import com.alibaba.dubbo.config.annotation.Reference;
import com.github.pagehelper.PageInfo;
import com.ooqiu.gaming.server.domain.Article;
import com.ooqiu.gaming.service.article.api.ArticleService;
import com.ooqui.gaming.server.commons.constant.DubboVersionConstant;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * 文章接口
 * <p>Title: ArticleControllerV1</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/7 3:22
 */
@RestController
@RequestMapping(value = "${rest.path.api.v1}/article")
public class ArticleControllerV1 {

    @Reference(version = DubboVersionConstant.DUBBO_VERSION_GAMING_SERVER_SERVICE_ARTICLE)
    private ArticleService articleService;

    /**
     * 获取文章列表
     * @param pageNum 页码
     * @param pageSize 笔数
     * @return
     */
    @ApiOperation(value = "获取文章列表", notes = "获取文章分页列表")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "pageNum", value = "页码", required = true, dataType = "int", paramType = "path"),
            @ApiImplicitParam(name = "pageSize", value = "笔数", required = true, dataType = "int", paramType = "path")
    })
    @RequestMapping(value = "data/list/{pageNum}/{pageSize}", method = RequestMethod.GET)
    public List<Article> data(
            @PathVariable("pageNum") int pageNum,
            @PathVariable("pageSize") int pageSize) {
        PageInfo<Article> pageInfo = articleService.selectAll(pageNum, pageSize);
        return pageInfo.getList();
    }

    /**
     * 获取频道列表
     * @param pageNum 页码
     * @param pageSize 笔数
     * @param channelId 频道 ID
     * @return
     */
    @ApiOperation(value = "获取文章列表", notes = "根据频道获取文章分页列表")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "pageNum", value = "页码", required = true, dataType = "int", paramType = "path"),
            @ApiImplicitParam(name = "pageSize", value = "笔数", required = true, dataType = "int", paramType = "path"),
            @ApiImplicitParam(name = "channelId", value = "频道 ID", required = true, dataType = "long", paramType = "path")
    })
    @RequestMapping(value = "data/channel/{channelId}/{pageNum}/{pageSize}", method = RequestMethod.GET)
    public List<Article> data(
            @PathVariable(value = "pageNum", required = true) int pageNum,
            @PathVariable(value = "pageSize", required = true) int pageSize,
            @PathVariable(value = "channelId", required = true) long channelId) {
        PageInfo<Article> pageInfo = articleService.selectByChannel(pageNum, pageSize, channelId);
        return pageInfo.getList();
    }

    /**
     * 获取频道列表
     * @param pageNum 页码
     * @param pageSize 笔数
     * @param type 文章类型
     * @return
     */
    @ApiOperation(value = "获取文章列表", notes = "根据类型获取文章分页列表")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "pageNum", value = "页码", required = true, dataType = "int", paramType = "path"),
            @ApiImplicitParam(name = "pageSize", value = "笔数", required = true, dataType = "int", paramType = "path"),
            @ApiImplicitParam(name = "type", value = "文章类型", required = true, dataType = "String", paramType = "path")
    })
    @RequestMapping(value = "data/type/{type}/{pageNum}/{pageSize}", method = RequestMethod.GET)
    public List<Article> data(
            @PathVariable(value = "pageNum", required = true) int pageNum,
            @PathVariable(value = "pageSize", required = true) int pageSize,
            @PathVariable(value = "type", required = true) String type) {
        PageInfo<Article> pageInfo = articleService.selectByType(pageNum, pageSize, type);
        return pageInfo.getList();
    }
}
```

## 查看 API 文档

访问地址：http://localhost:8500/swagger-ui.html

![](/assets/Lusifer1520371509.png)

## Swagger 注解说明

Swagger 通过注解表明该接口会生成文档，包括接口名、请求方法、参数、返回信息的等等。

* @Api：修饰整个类，描述 Controller 的作用
* @ApiOperation：描述一个类的一个方法，或者说一个接口
* @ApiParam：单个参数描述
* @ApiModel：用对象来接收参数
* @ApiProperty：用对象接收参数时，描述对象的一个字段
* @ApiResponse：HTTP 响应其中 1 个描述
* @ApiResponses：HTTP 响应整体描述
* @ApiIgnore：使用该注解忽略这个API
* @ApiError：发生错误返回的信息
* @ApiImplicitParam：一个请求参数
* @ApiImplicitParams：多个请求参数
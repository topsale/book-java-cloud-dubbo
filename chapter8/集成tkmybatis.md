# 集成 tk.mybatis

---

我们使用 tk.mybatis 简化 mybatis 开发，所以我们在 pom 中仅添加 tk.mybatis 的 starter pom

## Starter POM

```
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>1.1.5</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

## application.yml

```
mybatis:
    type-aliases-package: com.lusifer.spring.boot.pojo
    mapper-locations: classpath:mapper/*.xml
```

## 创建 MyMapper 通用接口

```
package com.lusifer.spring.boot.utils;

import tk.mybatis.mapper.common.Mapper;
import tk.mybatis.mapper.common.MySqlMapper;

/**
 * 自己的 Mapper
 * 特别注意，该接口不能被扫描到，否则会出错
 * <p>Title: MyMapper</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/11/7 10:43
 */
public interface MyMapper<T> extends Mapper<T>, MySqlMapper<T> {

}
```